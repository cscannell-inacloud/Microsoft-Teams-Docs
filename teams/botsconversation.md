# Starting a conversation

Bots in Microsoft Teams allow private conversations with a single user or a group conversation in a Teams channel.

## Initiating conversation

Microsoft Teams currently supports three methods for conversation:
* One-on-One Response - Users can interact in a private conversation with a bot by simply selecting the added bot in the chat history, or typing its name or Bot ID in the To: box on a new chat.
* In Channel Response - A bot can also be @mentioned in a channel if it has been added to the team.  Note that additional replies to a bot in a channel require @mentioning the bot - it will not respond to replies where it is not @mentioned.
* In Channel Conversation Creation - A bot in a channel may also initiate a new conversation in a channel.  

Note that neither One-on-One message creation, Group Respond, noor Group Conversation Creation are not currently supported


## One-on-One conversations

### Receiving messages

For a 1:1 conversation with a bot, the regular [Activity](https://docs.botframework.com/en-us/core-concepts/reference/#activity) message schema will apply. 
Of note:
* channelId is "msteams"
* From user id in "from" -> "id" - This is a unique and encrypted id for that user for your bot, and is suitable as a key should your app wish to store user data.  Note, though, that this is unique for your bot and cannot be directly used outside your bot instance in any meaningful way to identify that user.

### Replying to message
In order to reply to an existing message, you simply need to call the `ReplyToActivity()` in [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#replying) or 'session.send' in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/session/#sending-messages).  The BotFramework SDK handles all the details.

If you choose to use the REST API, you can also call the [/conversations/{conversationId}/activities/{activityId}`](https://docs.botframework.com/en-us/restapi/connector/#/Conversations) endpoint.  


## One to Many - Bots in Channels

### Receiving messages

For a bot in a channel, in addition to the [regular message schema](https://docs.botframework.com/en-us/core-concepts/reference/#activity) , your bot will also receive the following properties:
* Teams ID - The id for the Team is in the "channelData" -> "teamsTeamId" property.
* Teams channel ID – The id for the Teams channel is in the “channelData” -> “teamsChannelId” property.  
* Reply chain ID – That’s the channel id plus the id of the first message in the reply chain. It’s represented in the “conversation” -> “id” property.
* Detailed info about the mentioned bots/users – That will be included in the “entities” section with "type" = "mention". The “text” will match the mentioned text in the top level “text”, which will be wrapped with <at></at>.

### Replying to message
Replying to a message in a channel is the same as in 1:1.  Note that replying to a message in a channel shows as a reply to the initiating reply chain - for bots in channels, the conversationId contains channel and the top level message id.  While the BotFramework takes care of the details, you may cache that conversationId for future replies to that conversation thread as needed.


### Mentions

You can retrieve all menions in the message by calling the `GetMentions()` function in the BotFramework C# SDK.  This should return an array of all the @mentions sent in the "entities" section of the schema.

Note that bots in a channel only respond if @mentioned and therefore the body of the text message will always include the @Bot name.  Ensure your message parsing excludes that.  For example:

```
Mention[] m = sourceMessage.GetMentions();

for (int i = 0;i < m.Length;i++)
{
    if (m[i].Mentioned.Id == sourceMessage.Recipient.Id)
    {
        messageText = messageText.Replace(m[i].Text, "");
    }
}
```

## Channel Conversation Creation

Once your bot has been added to the team, it can also post into a channel to create a new reply chain.  With the BotBuilder SDK, call  `CreateConversation()` for [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple) or utilize Proactive Messaging techniques (`bot.send()` and `bot.beginDialog()`) in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/UniversalBot/#proactive-messaging).  

Alternatively, you can issue a POST request to the [`/conversations/{conversationId}/activities/`]() resource.

Note: at this point, bots in Microsoft Teams cannot initiate 1:1 / direct or 1:many / group conversations.

### Example
```
var channelData = new Dictionary<string, string>();
channelData["teamsChannelId"] = "19:dc5ba12695be4eb7bf457cad6b4709eb@thread.skype";
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

## Full inbound Schema example (bot in a channel)
```
{
    "type": "message",
    "id": "1485983408511",
    "timestamp": "2017-02-01T21:10:07.437Z",
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",
    "channelId": "msteams",
    "from": {
        "id": "29:1XJKJMvc5GBtc2JwZq0oj8tHZmzrQgFmB39ATiQWA85gQtHieVkHilBZ9XHoq9j7Zaqt7CZ-NJWi7me2kHTL3Bw",
        "name": "Richard Moe"
    },
    "conversation": {
        "id": "19:253b1f341670408fb6fe51050b6e5ceb@thread.skype;messageid=1485983194839"
    },
    "recipient": {
        "id": "28:c9e8c047-2a74-40a2-b28a-b162d5f5327c",
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
            "id": "28:c9e8c047-2a74-40a2-b28a-b162d5f5327c",
            "name": "Teams TestBot"
        },
        "text": "<at>Teams TestBot</at>"
    },
    {
    "type": "mention",
    "mentioned": {
            "id": "29:1jnFbZYs0qXMLH-O4S9-sDLNc3NVEIMWMnC-q0tVdEa-8BRosfojI35QdNoB-yW8iutWLJzHUm_mqEZSSU8si0Q",
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
        "teamsChannelId": "19:253b1f341670408fb6fe51050b6e5ceb@thread.skype",
        "teamsTeamId": "19:712c61d0ef384e5fa681ba90ca943398@thread.skype"
    }
}
```




