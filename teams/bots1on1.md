# Bots in One-on-One conversations

Microsoft Teams allows users to engage in direct conversations with bots built on the [Microsoft Bot Framework](https://docs.botframework.com/en-us/).  Users can find bots in the Bots Gallery, and add them to their Teams experience.  Team owners and users with the appropriate permissions can also add bots as full team members (see [bots in channels](botsinchannels.md)), which not only makes them available in that team's channels, but for 1:1 chat for all of those users as well.

## Designing a great bot in a Microsoft Teams channel

A great bot in Microsoft Teams helps users get the information they need, all within the context of the Teams experience.  Conversations with a bot are private exchanges between a bot and its user, and is a great way to provide information specific and relevant to that user in the 1-on-1 context.  Whereas a bot in a channel might provide information specific for that team or channel's work, a bot in 1-on-1 is really a dialog between your service and the individual.  

Note that depending on your experience, the bot might be entirely relevant in both contexts as is, and in fact, no significant extra work is required to allow your bot to work in both.  In Microsoft Teams, there is no expectation that your bot function in all contexts, but you should ensure your bot makes sense, and provides user value, in whichever contexts you choose to support.

Please note that while all bots need not provide a channel experience, your bot must be responsive in one-on-one.

## Develop your bot

In general, developing a bot for Teams is the same as developing for any other channel in the Bot Framework.  See [here](botscreate.md) for more information on getting started creating a bot for Teams.

### Receiving messages

For a 1:1 conversation with a bot, the regular [Activity](https://docs.botframework.com/en-us/core-concepts/reference/#activity) message schema will apply. 

Of note:
* `channelId` - the channel identifier will always be `msteams`
* `from.id` - this is a unique and encrypted id for that user for your bot, and is suitable as a key should your app wish to store user data.  Note, though, that this is unique for your bot and cannot be directly used outside your bot instance in any meaningful way to identify that user.
* `channelData.tenant.id` - this is the tenant id for the user

### Replying to message
In order to reply to an existing message, you simply need to call the `ReplyToActivity()` in [C#](https://docs.botframework.com/en-us/csharp/builder/sdkreference/routing.html#replying) or 'session.send' in [Node.JS](https://docs.botframework.com/en-us/node/builder/chat/session/#sending-messages).  The BotFramework SDK handles all the details.

If you choose to use the REST API, you can also call the [/conversations/{conversationId}/activities/{activityId}`](https://docs.botframework.com/en-us/restapi/connector/#/Conversations) endpoint.  

Please note that in your outbound schema, you should alwas use the same `serviceUrl` as the one you received.

Also note: at this time, it is not possible for a bot to directly initiate a conversation with a user.  The user must first add the bot and/or send it a message before it has the appropriate user information to reply back to the individual user.

### Receiving events

For one-on-one chats, the main event you should be aware of is the `conversationUpdate` event, which your bot receives when a user first adds it for 1:1 conversation.  Bots added to channels receive additional information - more information, see [Bot Events](botevents.md).

#### Best Practice - Welcome message

For bots that are added directly to by a user, and not already part of any of the user's teams, it is a best practice to send a Welcome Message, to introduce it to all users of the team, and tell a bit about its functionality.  To do this, make sure your bot responds to the `conversationUpdate` message, with the `teamsAddMembers` eventType in the `channelData` object.



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