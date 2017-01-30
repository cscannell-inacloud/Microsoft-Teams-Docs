# Starting a conversation

Bots in Microsoft teams allow private conversations with a single user or a group conversation in a Teams channel.

## Replying to message
In order to reply to an existing message, you simply need to call the `ReplyToActivity()` [C#]()/[Node.JS]() method in the BotBuilder SDK, or call the [/conversations/{conversationId}/activities/{activityId}`]() Connector REST API endpoint.
Because channel messages are contained with reply chains, you will want to post the message back to an existing reply chain ID. To do this, make sure the conversation ID contains the “;messageid=xxx” portion.

### Schema example

```
{
"type": "message/card",
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
"text": "",
"attachments": [
    {
    "contentType": "image/png",
    "contentUrl": "https://upload.wikimedia.org/wikipedia/en/a/a6/Bender_Rodriguez.png",
    "name": "Bender_Rodriguez.png"
    }
],
"entities": [],
"replyToId": "3UP4UTkzUk1zzeyW"
}
```

## Creating a new reply chain
Your bot can also post into a channel to create a new reply chain via the `CreateConversation()` [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#conversationmultiple)/[Node.JS]() method of the BotBuilder SDK. Alternatively, you can issue a POST request to the [`/conversations/{conversationId}/activities/`]() resource.

### Schema example

```
{
"type": "message/card",
"timestamp": "2016-10-29T00:51:05.9908157Z",
"serviceUrl": "https://skype.botframework.com",
"channelId": "msteams",
"from": {
    "id": "28:9e52142b-5e5e-4d7b-bb3e- e82dcf620000",
    "name": "SchemaTestBot"
},
"conversation": {
    "id": "19:aebd0ad4d6ab42c8b9ed19c251c2fc37@thread.skype"
},
"recipient": {
    "id": "8:orgid:6aebbad0-e5a5-424a-834a-20fb051f3c1a",
    "name": "stlrgload100"
},
"text": "",
"attachments": [
    {
    "contentType": "image/png",
    "contentUrl": "https://upload.wikimedia.org/wikipedia/en/a/a6/Bender_Rodriguez.png",
    "name": "Bender_Rodriguez.png"
    }
],
"entities": [],
"replyToId": "3UP4UTkzUk1zzeyW"
}
```


## Receiving messages

In addition to the regular message schema, your bot will also receive the following properties:
•	Channel ID – That’s in the “ChannelData” -> “teamsChannelId” property.
•	Reply chain ID – That’s the channel id plus the id of the first message in the reply chain. It’s represented in the “Conversation” -> “id” property.
•	Detailed info about the mentioned bots/users – That will be included in the “entities” section (Not  yet deployed). The “text” will match the mentioned text in the top level “text” property.
```
      {
         "type":"mention",
         "mentioned":{
            "id":"8:orgid:72f988bf86f141af91ab2d7cd011db47_3cb37ec1d8c9494480171446ede5516b",
            "name":"DMX Test User 2"
          }
          “text”: <@DMX Test User 2>
      }
```

### Schema example
```
{
  "type": "message",
  "id": "1481567603816",
  "timestamp": "2016-12-12T18:33:23.73Z",
  "serviceUrl": "https://skype.botframework.com",
  "channelId": "msteams",
  "from": {
    "id": "8:orgid:a887e3b1-6e64-4eaa-bd55-149cb7cec1c0",
    "name": "Jingjing Xia"
  },
  "conversation": {
    "isGroup": true,
    "id": "19:aebd0ad4d6ab42c8b9ed19c251c2fc37@thread.skype;messageid=1481567603816"
  },
  "recipient": {
    "id": "28:9e52142b-5e5e-4d7b-bb3e-e82dcf620000",
    "name": "JingjingSkypeBot"
  },
  "textFormat": "plain",
  "text": "JingjingSkypeBot  How are you?",
  "attachments": [
    {
      "contentType": "text/html",
      "content": "<div><span contenteditable=\"false\" itemscope=\"\" itemtype=\"http://schema.skype.com/Mention\" itemid=\"0\">JingjingSkypeBot</span>&#160;How are you?</div>"
    }
  ],
  "entities": [],
  "channelData": {
    "teamsChannelId": "19:aebd0ad4d6ab42c8b9ed19c251c2fc37@thread.skype"
  }
}
```
