# Bots in Channels

Microsoft Teams allows users to bring bots into their channel conversations.  By adding a bot as a team member, all users of the team can take advantage of the bot functionality right in the channel conversation.  You can also access Teams-specific functionality within your bot like querying team information and @mentioning users.

# Designing a great teams bot

Bots added to a team become another team member, who can be @mentioned as part of the conversation.  In fact, bots only receive messages when they are @mentioned, so other conversations on the channel are not sent to the bot.

A bot in a channel should provide information relevant and appropriate for all members of the team.  While your bot can certainly provide any information relevant to the experience, keep in mind conversations with it are visible to all members of the channel.  Therefore, a great bot in a channel should add value to all users on the channel, and certainly not inadvertantly share information that would otherwise be more relevant in a personal context. 

Note that depending on your experience, the bot might be entirely relevant in both scopes (personal and team) as is, and in fact, no significant extra work is required to allow your bot to work in both.  In Microsoft Teams, there is no expectation that your bot function in all contexts, but you should ensure your bot makes sense, and provides user value, in whichever scope you choose to support.  For more information on scopes, see [Context in Teams](teamsapps.md#context-in-team-apps).

# Develop your bot

Developing a bot in a channel, at its core, is the same as developing a bot for 1:1 conversation.  There are additional events and data in the payload that provide Teams context.  Those differences, as well as key differences in common functionality are enumerated below.

## Receiving messages

For a bot in a channel, in addition to the [regular message schema](https://docs.botframework.com/en-us/core-concepts/reference/#activity), your bot will also receive the following properties:

* `channelData` - see [below](#teams-channel-data).
* `conversationData.id` - this value is the reply chain ID, consisting of channel id plus the id of the first message in the reply chain. 
* `conversationData.isGroup` - this value will be set to `true` for bot messages in channels
* `entities` - this object may contain one or more Mention objects (see [below](#mentions))

## Replying to messages

In order to reply to an existing message, call the `ReplyToActivity()` in [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#replying) or `session.send` in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/session/#sending-messages).  The BotFramework SDK handles all the details.

If you choose to use the REST API, you can also call the [/conversations/{conversationId}/activities/{activityId}`](https://docs.botframework.com/en-us/restapi/connector/#/Conversations) endpoint.  

Note that replying to a message in a channel shows as a reply to the initiating reply chain.  The `conversationId` contains the channel and the top level message id.  While the Bot Framework takes care of the details, you may cache that `conversationId` for future replies to that conversation thread as needed.

## Creating new channel messages

Your team-added bot can post into a channel to create a new reply chain.  With the BotBuilder SDK, call  `CreateConversation()` for [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple) or utilize Proactive Messaging techniques (`bot.send()` and `bot.beginDialog()`) in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/UniversalBot/#proactive-messaging).  

Alternatively, you can issue a POST request to the [`/conversations/{conversationId}/activities/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_SendToConversation) resource.

Note: at this point, bots in Microsoft Teams cannot initiate 1:many / group conversations.

### Example (.NET SDK)
```csharp
//Helper class:
public class ChannelData
{
    public string TeamsChannelId { get; set; }
    public string TeamsTeamId { get; set; }
    public Tenant Tenant { get; set; }
}
...
var channelData = new ChannelData { TeamsChannelId = yourTeamsChannelID };
IMessageActivity newMessage = Activity.CreateMessageActivity();
newMessage.Type = ActivityTypes.Message;
newMessage.Text = answer;
ConversationParameters conversationParams = new ConversationParameters(
    isGroup: true,
    bot: null,
    members: null,
    topicName: "Test Conversation",
    activity: (Activity)newMessage,
    channelData: channelData);
var result = await connector.Conversations.CreateConversationAsync(conversationParams);
```

#### Best Practice - Welcome message

When your bot is first added to the team, it is a best practice to send a Welcome Message to the team, to introduce it to all users of the team, and tell a bit about its functionality.  To do this, make sure your bot responds to the `conversationUpdate` message, with the `teamsAddMembers` eventType in the `channelData` object.  Note that you must ensure that the `memberAdded` id is the Bot id itself, as the same event is sent when a new user is added to a team.  See [here](botevents.md#team-member-or-bot-addition) for more details.

You may also wish to send a 1:1 message to each member of the team when the bot is added.  To do this, you could [query the team roster](botsinchannels.md#fetching-the-team-roster) and send each user a [direct messages](bots1on1.md#creating-direct-messages).

For more best practices, see our [design guidelines](design.md).

## Teams channel data

Teams-specific information is sent and received in the `channelData` object:.

* `channel` - this object is passed only in channel contexts, when the bot is @mentioned or for events in channels in teams where the bot has been added.
    - `id` - the GUID for the channel.
    - `name` - the channel name, passed only in cases of [channel modification events](botevents.md). 
* `eventType` - Teams event type - passed only in cases of [channel modification events](botevents.md).
* `team` - this object is passed only in channel contexts, not 1:1.
    - `id` - the GUID for the channel.
    - Note: there is no name value being passed at this time
* `tenant.id` - the O365 tenant id on which Teams is running.  This is passed in all contexts.

>**Note:** the payload also contains `channelData.teamsTeamId` and `channelData.teamsChannelId` properties for backwards compatibility.  These should be considered deprecated.

Please note that `channelData` should be used as the definitive information for team and channel Ids, for your use in cacheing and utilizing as key local storage.

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

## Mentions

Bots in a channel only respond when they themselves are @mentioned in a message.  That means every message received by a bot in a channel contains its own name, and you must make sure your message parsing handles that case (see below for sample).  In addition, bots can parse out other users @mentioned and @mention users as part of their message as well.

### Retrieving mentions

Mentions are returned in the `entities` object in payload, and contain both the unique ID of the user and in most cases, the name of user mentioned.  You can retrieve all mentions in the message by calling the `GetMentions()` function in the BotFramework .NET SDK.  This should return an array of Mentioned objects.

Per above, all messages your bot receives will have its name in the channel, and should accomodate that in its message parsing.

#### Sample code - check for and strip @bot mention (.NET SDK)
```csharp
Mention[] m = sourceMessage.GetMentions();
var messageText = sourceMessage.Text;

for (int i = 0;i < m.Length;i++)
{
    if (m[i].Mentioned.Id == sourceMessage.Recipient.Id)
    {
        //Bot is in the @mention list.  
        //The below example will strip the bot name out of the message, so you can parse it as if it wasn't included.  Note that the Text object will contain the full bot name, if applicable.
        if (m[i].Text != null)
            messageText = messageText.Replace(m[i].Text, "");
    }
}
```

#### Sample code - check for and stript @bot mention (Node.js)
```javascript

var text = message.text;
if (message.entities) {
    message.entities
        .filter(entity => (entity.type === "mention" && entity.id === yourbotid))
        .forEach(entity => {
            text = text.replace(entity.text, "");
        });
    text = text.trim();
}
```

### Constructing mentions

Your bot can @mention other users in messages posted into channels. To do this, your message must:
* Include <at>@username</at> in the message text. Note, at this time because we do not provide an API to return profile information, you can either obtain this value from a received message, cached data, or from an external lookup, e.g. Azure ActiveDirectory
* Include the `mention` object inside the entities collection

>**Note**: At this time, team and channel @mentions are not supported.

#### .NET SDK sample

Note: This sample uses the [new Microsoft Teams .NET SDK](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams):

```csharp
// Create reply activity
Activity replyActivity = activity.CreateReply();

// Construct text of the form @sender Hello
replyActivity.Text = "Hello ";
replyActivity.AddMentionToText(activity.From, MentionTextLocation.AppendText);

// Send the reply activity
await client.Conversations.ReplyToActivityAsync(replyActivity);
```

#### Node SDK sample

Note: this sample uses [the new Microsoft Teams Node.js SDK](https://www.npmjs.com/package/botbuilder-teams):

```javascript
// User to mention
var toMention: builder.IIdentity = {
    name: 'John Doe',
    id: userId
};

// Create a new message and add mention to it
var msg = new teams.TeamsMessage(session).text(teams.TeamsMessage.getTenantId(session.message));
var mentionedMsg = (<teams.TeamsMessage>msg).addMentionToText(toMention);

// Post the message
var generalMessage = mentionedMsg.routeReplyToGeneralChannel();
session.send(generalMessage);
```

#### Schema example - outgoing message with user @mentioned

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
            "text": "<at>@Larry Jin</at>"
        } 
    ], 
    "replyToId": "3UP4UTkzUk1zzeyW" 
}
```


## Fetching the team roster
Your bot can query for the list of team members. With the BotBuilder SDK, call  [`GetConversationMembersAsync()` in the .NET SDK](https://docs.botframework.com/en-us/csharp/builder/sdkreference/d7/d08/class_microsoft_1_1_bot_1_1_connector_1_1_conversations_extensions.html#a0a665865891d485956e52c64bce84d4b) to return a list of user Ids for the `team.id` retrieved from the `channelData` of the inbound schema.

#### .NET SDK sample

```csharp
ChannelAccount[] members = connector.Conversations.GetConversationMembers(sourceMessage.Conversation.Id);

replyMessage.Text = "These are the member userids returned by the GetConversationMembers() function:";

for (int i = 0; i < members.Length; i++)
{
    replyMessage.Text += "<br />" + members[i].Id; //Not currently supported: members[i].Name;
}
```

#### Node SDK sample

Note: this sample uses [the new Microsoft Teams Node.js SDK](https://www.npmjs.com/package/botbuilder-teams):

```js
var conversationId = session.message.address.conversation.id;
  connector.fetchMemberList(
    (<builder.IChatConnectorAddress>session.message.address).serviceUrl,
    conversationId,
    teams.TeamsMessage.getTenantId(session.message),
    (err, result) => {
      if (err) {
        session.endDialog('There is some error');
      }
      else {
        session.endDialog('%s', JSON.stringify(result));
      }
    }
);
```

Alternatively, you can issue a GET request to the [`/conversations/{teamId}/members/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_GetConversationMembers) resource, using `teamId` in the `conversationId` field in the API call.

## Fetching the list of channels in a team
Your bot can query the list of channels in a team using the FetchChannelList method.

#### .NET SDK sample

Note: This sample uses the [new Microsoft Teams .NET SDK](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams):

```csharp
 ConversationList channels = client.GetTeamsConnectorClient().Teams.FetchChannelList(activity.GetChannelData<TeamsChannelData>().Team.Id);
```

#### Node SDK sample

Note: this sample uses [the new Microsoft Teams Node.js SDK](https://www.npmjs.com/package/botbuilder-teams):

```javascript
var teamId = session.message.sourceEvent.team.id;
  connector.fetchChannelList(
    (<builder.IChatConnectorAddress>session.message.address).serviceUrl,
    teamId,
    (err, result) => {
      if (err) {
        session.endDialog('There is some error');
      }
      else {
        session.endDialog('%s', JSON.stringify(result));
      }
    }
);
```
