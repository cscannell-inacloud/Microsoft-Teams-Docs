# Starting a conversation

Bots in Microsoft Teams allow private conversations with a single user or a group conversation in a Teams channel.

## Initiating conversation

Microsoft Teams currently supports four methods for conversation:
* One-on-One Response - Users can interact in a private conversation with a bot by simply selecting the added bot in the chat history, or typing its name or Bot ID in the To: box on a new chat.
* One-on-One Direct messages - Your bot can create 1:1 conversations with users.  This will allow your bot to proactively notify them.
* In Channel Response - A bot can also be @mentioned in a channel if it has been added to the team.  Note that additional replies to a bot in a channel require @mentioning the bot - it will not respond to replies where it is not @mentioned.
* In Channel Conversation Creation - A bot in a channel may also initiate a new conversation in a channel.

Note that bots in private group chats are currently not supported.

## One-on-One conversations

### Receiving messages

For a 1:1 conversation with a bot, the regular [Activity](https://docs.botframework.com/en-us/core-concepts/reference/#activity) message schema will apply.

Of note:
* `channelId` - this value, which specifics with Bot Framework channel the message is coming from, will always be `msteams`
* `from.id` - This is a unique and encrypted id for that user for your bot, and is suitable as a key should your app wish to store user data.  Note, though, that this is unique for your bot and cannot be directly used outside your bot instance in any meaningful way to identify that user.

### Replying to message
In order to reply to an existing message, you simply need to call the [`ReplyToActivity()` in C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#replying) or [`session.send` in Node.JS](https://docs.botframework.com/en-us/node/builder/chat/session/#sending-messages).  The BotFramework SDK handles all the details.

If you choose to use the REST API, you can also call the [/conversations/{conversationId}/activities/{activityId}](https://docs.botframework.com/en-us/restapi/connector/#/Conversations) endpoint. Please note:  if you are constructing the outgoing payload yourself, always use the `serviceUrl` received in the inbound messages as your outbound `serviceUrl`.

### Direct Message Creation (1:1)

Bots can now create 1:1 conversations with Microsoft Teams users. This will allow your bot to proactively notify them. For instance, if your bot was added to a team, it could query the team roster and send users individual messages in the private 1:1 chat, or a user could @mention another user to trigger the bot to send that user a direct message.

You can create the 1:1 chat with a user as long as you have the user’s unique ID and tenant ID. Typically, this information is obtained from a team context, either by obtaining the team roster or when a user interacts with your bot in a channel.

To create the Conversation ID, which you can then use for future direct messages to the user, you can call [`CreateConversation()` for C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple) or utilize Proactive Messaging techniques (`bot.send()` and `bot.beginDialog()`) in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/UniversalBot/#proactive-messaging).  Note that at this point, [`CreateDirectConverstion()` for C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple) is not supported.

Alternatively, you can issue a POST request to the [`/conversations/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_CreateConversation) resource.

The call to create a chat, either via the SDK or the API request, returns a Conversation ID which is used to send the message itself, for example via  [SendToConversation() in C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#sendtoconversation).  This ID will be consistant for that user, so you can store it for future direct messages.

Because your bot is able to proactively message users, you should use this capability sparingly and consider the user experience. Make sure not to spam end users and send only the minimum amount of information and number of messages needed to complete your scenario.

#### API Request

```json
POST /v3/conversations 
{
  "bot": {
    "id": "28:10j12ou0d812-2o1098-c1mjojzldxcj-1098028n ",
    "name": "The Bot"
  },
  "members": [
    {
      "id": "29:012d20j1cjo20211"
    }
  ],
  "channelData": {
    "tenant": {
      "id": "197231joe-1209j01821-012kdjoj"
    }
  }
}
```

You must supply the user ID and the tenant ID.  If the call successed, the API will return with the following response object:

```json
{
  "id":"a:1qhNLqpUtmuI6U35gzjsJn7uRnCkW8NiZALHfN8AMxdbprS1uta2aT-jytfIlsZR3UZeg3TsIONNInBHsdjzj3PtfHuhkxxvS1jZZ61UAbw8fIdXcNSJyTJm7YvHFOgxo"
}
```
This ID is the unique 1:1 chat's conversation ID.  Please store this value and reuse it for future interactions with the user.


#### C# example

Note: the Microsoft.Bot.Builder must be at least 3.5.3:

```c#
//Helper classes:
public class ChannelData
{
    public string TeamsChannelId { get; set; }
    public string TeamsTeamId { get; set; }
    public Tenant Tenant { get; set; }
}

public class Tenant
{
    public string Id { get; set; }
}
...
var parameters = new ConversationParameters
  {
    Bot = new ChannelAccount(YourBotID, YourBotName),
    Members = new ChannelAccount[] { new ChannelAccount(userId) },
    ChannelData = new ChannelData { Tenant = new Tenant { Id = tenantId } }
  };
var conversationResource = await connector.Conversations.CreateConversationAsync(parameters);

if (conversationResource != null)
{
    IMessageActivity message = Activity.CreateMessageActivity();
    message.From = new ChannelAccount(YourBotID, YourBotName);
    message.Conversation = new ConversationAccount(id: conversationResource.Id);
    message.Text = "Hello";
    message.Locale = "en-Us";
    await connector.Conversations.SendToConversationAsync((Activity)message);
}
```


## One to Many - Bots in Channels

### Receiving messages

Bot in channels will only receive messages in which they are @mentioned.  Therefore you should look for and strip out the bot name in any message you receive.

For a bot in a channel, in addition to the [regular message schema](https://docs.botframework.com/en-us/core-concepts/reference/#activity), your bot will also receive the following properties:

* `channelData` - see [below](#teams-specific-functionality).
* `conversationData.id` - this value is the reply chain ID, consisting of channel id plus the id of the first message in the reply chain. 
* `conversationData.isGroup` - this value will be set to `true` for bot messages in channels
* `entities` - this object may contain one or more Mention objects (see [below](#mentions))

Please note that channelData should be used as the definitive information for team and channel Ids, for your use in cacheing and utilizing as key local storage.

### Replying to message
Replying to a message in a channel is the same as in 1:1.  Note that replying to a message in a channel shows as a reply to the initiating reply chain - for bots in channels, the conversationId contains channel and the top level message id.  While the BotFramework takes care of the details, you may cache that conversationId for future replies to that conversation thread as needed.  

## Mentions

Bots have the ability to be retrieve and construct @mentions in a message, and to be triggered in channel have to be @mentioned themselves to receive a message.  The users in question, including the bot itself in channels, are passed in the `entities` object, with the following properties:

* **`type`** - the string "mention"
* **`mentioned.id`** - the GUID for the user, which is unique for your bot
* **`mentioned.name`** - the name of the user.  Note, at this time because we do not provide an API to return profile information, you can either obtain this value from a received message or from an external lookup, e.g. Azure ActiveDirectory
* **`text`** - the @mention text in the body of the message itself, which may or may not be the full name of that user due to Teams' ability to shorten (hit *backspace* when doing @mention name support).  Note that the text here, like in the body of the message, will be wrapped with the <at></at> tag.

>**Note**: At this time, team and channel @mentions are not supported.

#### Example Entities object

```json
"entities": [ 
    { 
        "type":"mention", 
        "mentioned":{ 
            "id":"29:08q2j2o3jc09au90eucae",
            "name":"Larry Jin" 
        }, 
        "text": "<at>Larry Jin</at>"
    } 
] 
```

### Retrieving mentions


You can retrieve all mentions in the message by calling the `GetMentions()` function in the BotFramework C# SDK.  This should return an array of all the @mentions sent in the `entities` section of the schema.

Note that bots in a channel only respond if @mentioned and therefore the body of the text message will always include the @Bot name.  Ensure your message parsing excludes that.  For example:

#### .NET SDK - Strip Bot Name ####
```csharp
Mention[] m = sourceMessage.GetMentions();
var messageText = sourceMessage.Text;

for (int i = 0;i < m.Length;i++)
{
    if (m[i].Mentioned.Id == sourceMessage.Recipient.Id)
    {
        //Bot is in the @mention list.  
        //The below example will strip the bot name out of the message, so you can parse it as if it wasn't included.
        //Note that the Text object will contain the full bot name, if applicable.
        if (m[i].Text != null)
            messageText = messageText.Replace(m[i].Text, "");
    }
}
```

#### Node ####
```javascript
function(session) {
    var entities = session.message.entities;
    var i;
    for (i = 0; i < entities.length; i++) {
        var entity = entities[i];
        if (entity.type === 'mention' && entity.mentioned.name === 'MyTestBot') {
            // The bot is mentioned.
            // Do some thing here
            session.send("What's up?");
        }
    }
}
```

### Constructing mentions

Your bot can @mention other users in messages posted into channels. To do this, your message must:
* Include <at>@username</at> in the message text. Note, at this time because we do not provide an API to return profile information, you can either obtain this value from a received message or maintain your own key/value store to map Teams userids to names you have already collected.
* Include the `mention` object inside the `entities` collection

>**Note**: At this time, team and channel @mentions are not supported.

#### Schema example

```json
{ 
    "type": "message", 
    "text": "Hey <at>Larry Jin</at> check out this message", 
    "timestamp": "2016-10-29T00:51:05.9908157Z", 
    "serviceUrl": "https://skype.botframework.com", 
    "channelId": "msteams", 
    "from": { 
        "id": "28:9e52142b-5e5e-4d7b-bb3e- e82dcf620000", 
        "name": "SchemaTestBot" 
    }, 
    "conversation": { 
        "id": "19:aebd0ad4d6ab42c8b9ed19c251c2fc37@thread.skype;messageid=1481567603816" 
    }, 
    "recipient": { 
        "id": "8:orgid:6aebbad0-e5a5-424a-834a-20fb051f3c1a", 
        "name": "stlrgload100" 
    }, 
    "attachments": [ 
        { 
            "contentType": "image/png", 
            "contentUrl": "https://upload.wikimedia.org/wikipedia/en/a/a6/Bender_Rodriguez.png", 
            "name": "Bender_Rodriguez.png" 
        } 
    ], 
    "entities": [ 
        { 
            "type":"mention", 
            "mentioned":{ 
                "id":"29:08q2j2o3jc09au90eucae",
                "name":"Larry Jin" 
            }, 
            "text": "<at>Larry Jin</at>"
        } 
    ], 
    "replyToId": "3UP4UTkzUk1zzeyW" 
}
```

## Channel Conversation Creation

Once your bot has been added to the team, it can also post into a channel to create a new reply chain.  With the BotBuilder SDK, call  `CreateConversation()` for [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple) or utilize Proactive Messaging techniques (`bot.send()` and `bot.beginDialog()`) in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/UniversalBot/#proactive-messaging).  

Alternatively, you can issue a POST request to the [`/conversations/{conversationId}/activities/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_SendToConversation) resource.

Note: at this point, bots in Microsoft Teams cannot initiate 1:many / group conversations.

### Example (C#)
```csharp
var channelData = new Dictionary<string, string>();
channelData["teamsChannelId"] = yourTeamsChannelID;
IMessageActivity newMessage = Activity.CreateMessageActivity();
newMessage.Type = ActivityTypes.Message;
newMessage.Text = "Hello channel.  This is a newly created reply chain.";
ConversationParameters conversationParams = new ConversationParameters(
    isGroup: true,
    bot: null,
    members: null,
    topicName: "Test Conversation",
    activity: (Activity)newMessage,
    channelData: channelData);
var result = await connector.Conversations.CreateConversationAsync(conversationParams);
```

## Dynamic message updates (New)

Rather than have your messages be static snapshots of data, your bot can now dynamically update messages inline after sending them to users. You can use dynamic message updates for scenarios such as poll updates, modifying available actions after a button press, or any other asynchronous state change.

The new message need not match the original in type. For instance, if the original message contained an attachment, the new message can be a simple text message.

### Rest API

To issue a message update, simply perform a PUT request against the `/v3/conversations/<conversationId>/activities/<activityId>/` endpoint using a given activity ID. To complete this scenario, you should cache the activity ID returned by the original POST call.

#### Request example
```
PUT /v3/conversations/19:ja0cu120i1jod12j@skype.net/activities/012ujdo0128
{
  “type”: “message”,
  “text”: “This message has been updated”
}
```

### .NET SDK
You can use the UpdateActivityAsync method in the Bot Framework SDK to update an existing message.

```csharp
public async Task<HttpResponseMessage> Post([FromBody]Activity activity)
{
   if (activity.Type == ActivityTypes.Message)
   {
      ConnectorClient connector = new ConnectorClient(new Uri(activity.ServiceUrl));
      Activity reply = activity.CreateReply($"You sent {activity.Text} which was {length} characters");
      var msgToUpdate = await connector.Conversations.ReplyToActivityAsync(reply);
      Activity updatedReply = activity.CreateReply($"This is an updated message");
      await connector.Conversations.UpdateActivityAsync(reply.Conversation.Id, msgToUpdate.Id, updatedReply);
   }
}
```

## Teams-specific functionality

Bots in Teams have channel-specific data and functionality that you can leverage to differentiate your bot experience in Teams.  This includes such functionality as retrieving team and channel information, as well as [channel events](botevents.md) sent to your bot by Teams.

### Teams channel data

Teams-specific information is sent and received in the `channelData` object, which has been updated since Preview launch.

#### Example channelData object (channelCreated event)
```json
"channelData": {
    "channel": {
        "id": "19:693ecdb923ac4458a5c23661b505fc84@thread.skype",
        "name": "My New Channel"
    },
    "eventType": "channelCreated",
    "team": {
        "id": "19:693ecdb923ac4458a5c23661b505fc84@thread.skype"
    },
    "tenant": {
        "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
    }
}
```

`channelData` objects:
* `channel` - this object is passed only in channel contexts, when the bot is @mentioned or for events in channels in teams where the bot has been added.
    - `id` - the GUID for the channel.
    - `name` - the channel name, passed only in cases of [channel modification events](botevents.md). 
* `eventType` - Teams event type - passed only in cases of [channel modification events](botevents.md).
* `team` - this object is passed only in channel contexts, not 1:1.
    - `id` - the GUID for the channel.
    - Note: there is no name value being passed at this time
* `tenant.id` - the O365 tenant id on which Teams is running.  This is passed in all contexts.

>**Note:** the payload also may contain `channelData.teamsTeamId` and `channelData.teamsChannelId` properties for backwards compatibility.  These should be deprecated in favor of the above.

### Fetching the team roster
Your bot can query for the list of team members. With the BotBuilder SDK, call  `GetConversationMembersAsync()` for [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/d7/d08/class_microsoft_1_1_bot_1_1_connector_1_1_conversations_extensions.html#a0a665865891d485956e52c64bce84d4b) to return a list of userIds for the `team.id` retrieved from the `channelData` of the inbound schema.

```csharp
ChannelAccount[] members = connector.Conversations.GetConversationMembers(sourceMessage.Conversation.Id);

replyMessage.Text = "These are the member userids returned by the GetConversationMembers() function";

for (int i = 0; i < members.Length; i++)
{
    replyMessage.Text += "<br />" + members[i].Id; //Not currently supported: members[i].Name;
}
```

Alternatively, you can issue a GET request to the [`/conversations/{teamId}/members/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_GetConversationMembers) resource, using `teamId` in the `conversationId` field in the API call.

Currently, this only returns the userIds but we will be including profile information in the future.


## Full inbound Schema example (bot in a channel)
```json
{
    "type": "message",
    "id": "1485983408511",
    "timestamp": "2017-02-01T21:10:07.437Z",
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",
    "channelId": "msteams",
    "from": {
        "id": "29:1XJKJMvc5GBtc2JwZq0oj8tHZmzrQgFmB39ATiQWA85gQtHieVkKilBZ9XHoq9j7Zaqt7CZ-NJWi7me2kHTL3Bw",
        "name": "Richard Moe"
    },
    "conversation": {
        "id": "19:253b1f341670408fb6fe51050b6e6ceb@thread.skype;messageid=1485983194839"
    },
    "recipient": {
        "id": "28:c9e8c047-2a74-40a2-b28b-b262d5f5327c",
        "name": "Teams TestBot"
    },
    "textFormat": "plain",
    "text": "<at>Teams TestBot</at> Hello <at>Jingjing Xia</at>",
    "attachments": [
    {
        "contentType": "text/html",
        "content": "<div><span contenteditable=\"false\" itemscope=\"\" itemtype=\"http://schema.skype.com/Mention\" itemid=\"0\">Teams TestBot</span> Hello <span contenteditable=\"false\" itemscope=\"\" itemtype=\"http://schema.skype.com/Mention\" itemid=\"1\">Jingjing Xia</span></div>"
    }
    ],
    "entities": [
    {
        "type": "mention",
        "mentioned": {
            "id": "28:c9e8c047-2a74-40a2-b28b-b262d5f5327c",
            "name": "Teams TestBot"
        },
        "text": "<at>Teams TestBot</at>"
    },
    {
    "type": "mention",
    "mentioned": {
            "id": "29:1jnFbZYs0qXMLH-O4S9-sDLNc3NVEIMWWnC-q0tVdEa-8BRosfojI35QdNoB-yW8iutWLJzHUm_mqEZSSU8si0Q",
            "name": "Jingjing Xia"
        },
        "text": "<at>Jingjing Xia</at>"
    },
    {
        "type": "clientInfo",
        "locale": "en-US",
        "country": "US",
        "platform": "Windows"
    }
    ],
    "channelData": {
        "teamsChannelId": "19:253b1f352670408fb6fe51050b6e5ceb@thread.skype",
        "teamsTeamId": "19:712c61d0ef393e5fa681ba90ca943398@thread.skype",
        "channel": {
            "id": "19:253b1f352670408fb6fe51050b6e5ceb@thread.skype"
        },
        "team": {
            "id": "19:712c61d0ef393e5fa681ba90ca943398@thread.skype"
        },
        "onBehalfOf": "[]",
        "tenant": {
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
        }
    }
}
```




