# Bots in One-on-One conversations



### Receiving messages

For a 1:1 conversation with a bot, the regular [Activity](https://docs.botframework.com/en-us/core-concepts/reference/#activity) message schema will apply. 
Of note:
* channelId is "msteams"
* From user id in "from" -> "id" - This is a unique and encrypted id for that user for your bot, and is suitable as a key should your app wish to store user data.  Note, though, that this is unique for your bot and cannot be directly used outside your bot instance in any meaningful way to identify that user.

### Replying to message
In order to reply to an existing message, you simply need to call the `ReplyToActivity()` in [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#replying) or 'session.send' in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/session/#sending-messages).  The BotFramework SDK handles all the details.

If you choose to use the REST API, you can also call the [/conversations/{conversationId}/activities/{activityId}`](https://docs.botframework.com/en-us/restapi/connector/#/Conversations) endpoint.  




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




