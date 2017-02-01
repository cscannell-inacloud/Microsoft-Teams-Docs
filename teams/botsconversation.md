# Starting a conversation

Bots in Microsoft teams allow private conversations with a single user or a group conversation in a Teams channel.

## Initiating conversation

Bots can interact in three contexts:
* One-on-One Response - Users can interact in a private conversation with a bot by simply selecting the added bot in the chat history, or typing its name in the To: box on a new chat.
* In Channel Response - Bots can also be @mentioned in a channel if it has been added to the team.  Note that additional replies to a bot in a channel require @mentioning the bot - it will not respond to replies where it is not @mentioned.
* In Channel Conversation Creation - Bots can initiate new channel conversations via the CreateConversation method.

## Receiving messages

For a 1:1 conversation with a bot, the regular [Activity](https://docs.botframework.com/en-us/core-concepts/reference/#activity) message schema will apply. 
Of note:
* channelId is "msteams"
* From user id in "from" -> "id" - This is a unique and encrypted id for that user for your bot, and is suitable as a key should your app wish to store user data.  Note, though, that this is unique for your bot and cannot be directly used outside your bot instance in any meaningful way to identify that user.

For a bot in a channel, in addition to the regular message schema, your bot will also receive the following properties:
•	Teams ID - The id for the Team is in the "channelData" -> "teamsTeamId" property.
•	Teams channel ID – The id for the Teams channel is in the “channelData” -> “teamsChannelId” property.  
•	Reply chain ID – That’s the channel id plus the id of the first message in the reply chain. It’s represented in the “conversation” -> “id” property.
•	Detailed info about the mentioned bots/users – That will be included in the “entities” section with "type" = "mention". The “text” will match the mentioned text in the top level “text”, which will be wrapped with <at></at>.

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

### Full inboud Schema example
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

## Replying to message
In order to reply to an existing message, you simply need to call the `ReplyToActivity()` [C#]()/[Node.JS]() method in the BotBuilder SDK, or call the [/conversations/{conversationId}/activities/{activityId}`]() Connector REST API endpoint.
Because channel messages are contained with reply chains, you will want to post the message back to an existing reply chain ID. To do this, make sure the conversation ID contains the “;messageid=xxx” portion.

### Outbound Schema example

```
{
    "type": "message",
    "timestamp": "2017-02-01T21:27:37.4510923Z",
    "from": {
        "id": "28:c9e8c047-2a74-40a2-b28a-b162d5f5327c",
        "name": "Teams TestBot"
    },
    "conversation": {
        "id": "19:651ee00e13424b89802b56758e0a762f@thread.skype;messageid=1485982152079"
    },
    "recipient": {
        "id": "29:1XJKJMvc5GBtc2JwZq0oj8tHZmzrQgFmB39ATiQWA85gQtHieVkHilBZ9XHoq9j7Zaqt7CZ-NJWi7me2kHTL3Bw",
        "name": "Richard Moe"
    },
    "text": "",
    "attachments": [
    {
        "contentType": "application/vnd.microsoft.card.thumbnail",
        "content": {
            "title": "Homegrown Thumbnail Card",
            "subtitle": "Sandwiches and salads",
            "text": "104 Lake St, Kirkland, WA 98033<br />(425) 123-4567",
            "images": [
            {
                "url": "https://skypeteamsbotstorage.blob.core.windows.net/bottestartifacts/sandwich_thumbnail.png",
                "alt": "https://skypeteamsbotstorage.blob.core.windows.net/bottestartifacts/sandwich_thumbnail.png"
                }
            ],
            "buttons": [
            {
                "type": "imBack",
                "title": "View in article",
                "value": "View in article"
            },
            {
                "type": "imBack",
                "title": "See more like this",
                "value": "See more like this"
            }
            ]
        }
    }
    ],
    "replyToId": "1485984457450"
}



## Creating a new reply chain
Your bot can also post into a channel to create a new reply chain via the `CreateConversation()` [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple)/[Node.JS]() method of the BotBuilder SDK. Alternatively, you can issue a POST request to the [`/conversations/{conversationId}/activities/`]() resource.
