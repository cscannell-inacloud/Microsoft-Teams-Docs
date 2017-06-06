# Bots in One-on-One conversations

Microsoft Teams allows users to engage in direct conversations with bots built on the [Microsoft Bot Framework](https://docs.botframework.com/en-us/).  Users can find bots in the Bots Gallery, and add them to their Teams experience for 1:1 conversations.  Team owners and users with the appropriate permissions can also add bots as full team members (see [bots in channels](botsinchannels.md)), which not only makes them available in that team's channels, but for 1:1 chat for all of those users as well.

## Designing a great personal (1:1) bot

A great bot in Microsoft Teams helps users get the information they need, all within the context of the Teams experience.  One-on-one conversations with a bot are private exchanges between a bot and its user, and are a great way to provide information specific and relevant to that user in the personal context.  Whereas a bot in a channel might provide information specific for that team or channel's work, a bot in 1-on-1 is really a dialog between your service and the individual.  

Note that depending on your experience, the bot might be entirely relevant in both scopes (personal and team) as is, and in fact, no significant extra work is required to allow your bot to work in both.  In Microsoft Teams, there is no expectation that your bot function in all contexts, but you should ensure your bot makes sense, and provides user value, in whichever scope you choose to support.  See [here](teamsapps.md) for more information on scopes.

## Starting a 1:1 conversation

Bots can create new conversations with an individual Microsoft Teams user, as long as your bot has user information obtained through previous addition in a personal or team scope. This will allow your bot to proactively notify them. For instance, if your bot was added to a team, it could query the team roster and send users individual messages in the private 1:1 chat, or a user could @mention another user to trigger the bot to send that user a direct message.

You can create the 1:1 chat with a user as long as you have the user’s unique ID and tenant ID. Typically, this information is obtained from a team context, either by obtaining the [team roster](botapis.md#fetching-the-team-roster) or when a user interacts with your [bot in a channel](botsinchannels.md).  For bots already added to the user's personal scope, you may already have cached user information via the [add user event](botevents.md#team-member-or-bot-addition).

Because your bot is able to proactively message users, you should use this capability sparingly and consider the user experience. Make sure not to spam end users and send only the minimum amount of information and number of messages needed to complete your scenario.

#### API Request

>**Note:** Make sure you authenticate and have a Bearer Token before creating a new conversation using the REST API

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

You must supply the user ID and the tenant ID.  If the call succeeds, the API will return with the following response object:

```json
{
  "id":"a:1qhNLqpUtmuI6U35gzjsJn7uRnCkW8NiZALHfN8AMxdbprS1uta2aT-jytfIlsZR3UZeg3TsIONNInBHsdjzj3PtfHuhkxxvS1jZZ61UAbw8fIdXcNSJyTJm7YvHFOgxo"
}
```
This ID is the unique 1:1 chat's conversation ID.  Please store this value and reuse it for future interactions with the user.


#### .NET SDK example

Note: This sample uses the [new Microsoft Teams .NET SDK](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams):

```csharp
// Create or get existing chat conversation with user
var response = client.Conversations.CreateOrGetDirectConversation(activity.Recipient, activity.From, activity.GetTenantId());

// Construct the message to post to conversation
Activity newActivity = new Activity()
{
    Text = "Hello",
    Type = ActivityTypes.Message,
    Conversation = new ConversationAccount
    {
        Id = response.Id
    },
};

// Post the message to chat conversation with user
await client.Conversations.SendToConversationAsync(newActivity, response.Id);
```

#### Node SDK example

Note: this sample uses [the Microsoft Teams Node.js SDK](https://www.npmjs.com/package/botbuilder-teams):

```js
var address = 
{ 
    channelId: 'msteams',
    user: { id: userId },
    channelData: {
        tenant: {
            id: tenantId
        }
    },
    bot:
    { 
        id: appId,
        name: appName 
    },
    serviceUrl: session.message.address.serviceUrl,
    useAuth: true
}

bot.beginDialog(address, '/');
```


## Best Practice - Welcome message

For bots that are added directly by a user, and not already part of any of the user's teams, it is a best practice to send a Welcome Message, to introduce it to all users of the team, and tell a bit about its functionality.  To do this, make sure your bot responds to the `conversationUpdate` message, with the `teamsAddMembers` eventType in the `channelData` object.

For more best practices, see our [design guidelines](design.md).
