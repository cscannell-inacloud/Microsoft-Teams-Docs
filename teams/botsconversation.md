# Overview of sending Messages

Bots in Microsoft Teams allow sending messages in either personal conversations with a single user or a group conversation in a Teams channel.  

## Initiating a conversation

Microsoft Teams currently supports four methods for conversation:
* One-on-One Response - Users can interact in a private conversation with a bot by simply selecting the added bot in the chat history, or typing its name or Bot ID in the To: box on a new chat.
* One-on-One Direct messages - Your bot can create 1:1 conversations with users.  This will allow your bot to proactively notify them.
* In Channel Response - A bot can be @mentioned in a channel if it has been added to the team.  Note that additional replies to a bot in a channel require @mentioning the bot - it will not respond to replies where it is not @mentioned.
* In Channel Conversation Creation - A bot in a channel may also initiate a new conversation in a channel.

Note that bots in private group chats are currently not supported.

## Conversation basics

Basic conversation is handled through the Bot Framework Connector, a single REST API to enable your bot to communicate with Teams and other channels.  The Bot Framework SDKs provide easy access to this API, and provides additional functionality to manage conversation flow and state, and simple ways to incorporate cognitive services such as natural language processing (NLP).

For further review on the types of bot interaction supported by the Bot Framework and therefore Microsoft Teams, please review the Bot Framework documentation on [conversation flow](https://docs.microsoft.com/en-us/bot-framework/bot-design-conversation-flow) principals and related concepts in the [.NET SDK](https://docs.microsoft.com/en-us/bot-framework/dotnet/) and [Node.js SDK](https://docs.microsoft.com/en-us/bot-framework/nodejs/) documentation.

### Receiving messages

For incoming messages, your bot will receive an [Activity](https://docs.botframework.com/en-us/core-concepts/reference/#activity) object, of type `message`.  While the Activity object may contain other types of information, like [channel events sent to your bot](botevents.md), the `message` type represents communication between bot and user.

Your bot will recieve a payload that contains the user message `Text` as well as other information about the user, the source of the message and Teams information.  Of note:
* `channelId` - the channel identifier will always be `msteams`
* `from.id` - this is a unique and encrypted id for that user for your bot, and is suitable as a key should your app wish to store user data.  Note, though, that this is unique for your bot and cannot be directly used outside your bot instance in any meaningful way to identify that user.
* `channelData.tenant.id` - this is the tenant id for the user

## Full inbound Schema example 
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
        "id": "a:17I0kl9EkpE1O9PH5TWrzrLNwnWWcfrU7QZjKR0WSfOpzbfcAg2IaydGElSo10tVr4C7Fc6GtieTJX663WuJCc1uA83n4CSrHSgGBj5XNYLcVlJAs2ZX8DbYBPck201w-"
    },
    "recipient": {
        "id": "28:c9e8c047-2a74-40a2-b28a-b162d5f5327c",
        "name": "Teams TestBot"
    },
    "textFormat": "plain",
    "text": "Hello Teams TestBot",
    "entities": [
        {
            "type": "clientInfo",
            "locale": "en-US",
            "country": "US",
            "platform": "Windows"
        }
    ],
    "channelData": {
        "tenant": {
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"
        }
    }
}
```
>Note: the text field for inbound messages sometimes contains @ mentions. Make sure to properly check and strip those. For more info see the mentions section [here](botsinchannels.md)

## Replying to messages
In order to reply to an existing message, call the `ReplyToActivity()` in [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#replying) or `session.send` in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/session/#sending-messages).  The Bot Framework SDK handles all the details.

If you choose to use the REST API, you can also call the [/conversations/{conversationId}/activities/{activityId}`](https://docs.botframework.com/en-us/restapi/connector/#/Conversations) endpoint.  

The message content itself can contain simple text or some of the Bot Framework-supplied [cards and action types](botsmessages.md).

Please note that in your outbound schema, you should always use the same `serviceUrl` as the one you received.

## Updating messages 

>New feature

Rather than have your messages be static snapshots of data, your bot can now dynamically update messages inline after sending them to users. You can use dynamic message updates for scenarios such as poll updates, modifying available actions after a button press, or any other asynchronous state change.

The new message need not match the original in type. For instance, if the original message contained an attachment, the new message can be a simple text message.

### Rest API

To issue a message update, simply perform a PUT request against the `/v3/conversations/<conversationId>/activities/<activityId>/` endpoint using a given activity ID. To complete this scenario, you should cache the activity ID returned by the original POST call.

```json
PUT /v3/conversations/19:ja0cu120i1jod12j@skype.net/activities/012ujdo0128
{
    "type": "message",
    "text": "This message has been updated"
}
```

### .NET SDK example

You can use the `UpdateActivityAsync` method in the Bot Framework SDK to update an existing message.

```csharp
public async Task<HttpResponseMessage> Post([FromBody]Activity activity)
{
  if (activity.Type == ActivityTypes.Message)
  {
    ConnectorClient connector = new ConnectorClient(new Uri(activity.ServiceUrl));
    Activity reply = activity.CreateReply($"You sent {activity.Text} which was {activity.Text.Length} characters");
    var msgToUpdate = await connector.Conversations.ReplyToActivityAsync(reply);
    Activity updatedReply = activity.CreateReply($"This is an updated message");
    await connector.Conversations.UpdateActivityAsync(reply.Conversation.Id, msgToUpdate.Id, updatedReply);
  }
}
```

### Node SDK example

You can use the `session.connector.update` method in the Bot Framework SDK to update an existing message.

```js
function sendCardUpdate(bot, session, originalMessage, address) {
	
	var origAttachment = originalMessage.data.attachments[0];
	origAttachment.content.subtitle = 'Assigned to Larry Jin';

	var updatedMsg = new builder.Message()
		.address(address)
		.textFormat(builder.TextFormat.markdown)
		.addAttachment(origAttachment)
		.toMessage();

	session.connector.update(updatedMsg, function(err, addresses) {
		if (err) {
			console.log(`Could not update the message`);
		}
	});
}
```

## Creating a new personal conversation

You can create a new message to either a user or start a new reply chain in a channel for your team bot.  This allows you to message your user(s) without prompting.  For more information see:
* [Creating a new 1:1 conversation](bots1on1.md#Starting-a-1:1-conversation)
* [Creating a new channel conversation](botsinchannels.md#creating-new-channel-conversations)

## Deleting messages

At this point, there is no way for you to delete messages via your bot.  You can update content in your message per above, but at this point there is no platform support to delete messages from users or your bot.

