# Bot APIs

Your bot can access additional context about the team or chat, such as user profile.  This information can be used to enrich your bot's functionality and provide a more personalized experience.

> Please note:  these Microsoft Teams-specific bot APIs are best accessed through our Bot Builder Extension.  For C#/.NET, download our [NuGet package](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams).  For Node.js development, you can install the [`botbuilder-teams` NPM package](https://www.npmjs.com/package/botbuilder-teams).  


## Fetching the team roster

Your bot can query for the list of team members and their basic profile, which includes Teams user ID and Azure ActiveDirectory information such as name and objectId. You can use this information to correlate user identities, for example to check if a user logged into a tab through AAD credentials is a member of the team.

#### REST API sample

You can directly issue a GET request to [`/conversations/{teamId}/members/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_GetConversationMembers) resource using the `teamId` as the parameter in the API call.

>Note: You must include the tenant ID in the X-MsTeamsTenantId HTTP request header. You can obtain the tenant ID from the `channelData` when your bot is first added to a team.

```json
GET /v3/conversations/19%3Aja0cu120i1jod12j%40skype.net/members
X-MsTeamsTenantId: 72f988bf-86f1-41af-91ab-2d7cd011db47

Response body
[{
    "id": "29:1GcS4EyB_oSI8A88XmWBN7NJFyMqe3QGnJdgLfFGkJnVelzRGos0bPbpsfJjcbAD22bmKc4GMbrY2g4JDrrA8vM06X1-cHHle4zOE6U4ttcc",
    "objectId": "9d3e08f9-a7ae-43aa-a4d3-de3f319a8a9c",
    "givenName": "Larry",
    "surname": "Brown",
    "email": "Larry.Brown@company.com",
    "userPrincipalName": "labrown@company.com"
}, {
    "id": "29:1bSnHZ7Js2STWrgk6ScEErLk1Lp2zQuD5H2qQ960rtvstKp8tKLl-3r8b6DoW0QxZimuTxk_kupZ1DBMpvIQQUAZL-PNj0EORDvRZXy8kvWk",
    "objectId": "76b0b09f-d410-48fd-993e-84da521a597b",
    "givenName": "John",
    "surname": "Patterson",
    "email": "johnp@company.com",
    "userPrincipalName": "johnp@company.com"
}, {
    "id": "29:1URzNQM1x1PNMr1D7L5_lFe6qF6gEfAbkdG8_BUxOW2mTKryQqEZtBTqDt10-MghkzjYDuUj4KG6nvg5lFAyjOLiGJ4jzhb99WrnI7XKriCs",
    "objectId": "6b7b3b2a-2c4b-4175-8582-41c9e685c1b5",
    "givenName": "Rick",
    "surname": "Stevens",
    "email": "Rick.Stevens@company.com",
    "userPrincipalName": "rstevens@company.com"
}]
```

#### .NET SDK sample

With the [new Microsoft Teams .NET SDK](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams), call `GetTeamsConversationMembersAsync()` using `Team.Id` and `Tenant.Id` obtained from `channelData` to return a list of user Ids.

```csharp
// Fetch the members in the current conversation
var connector = new ConnectorClient(new Uri(activity.ServiceUrl));
var members = await connector.Conversations.GetTeamsConversationMembersAsync(activity.Conversation.Id, activity.GetTenantId());

// Concatenate information about all the members into a string
var sb = new StringBuilder();
foreach (var member in members)
{
    sb.AppendFormat(
        "GivenName = {0}, Surname = {1}, Email = {2}, UserPrincipalName = {3}, AADObjectId = {4}, TeamsMemberId = {5}",
        member.GivenName, member.Surname, member.Email, member.UserPrincipalName, member.ObjectId, member.Id);
    
    sb.AppendLine();
}

// Post the member info back into the conversation
await context.PostAsync($"People in this conversation: {sb.ToString()}");
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

## Fetching user profile in 1:1 chat

You can also make the same API call for any 1:1 chat to obtain the profile information of the user chatting with your bot.

The API call and SDK methods are identical to fetching team roster, as is the response object. The only different is that you pass the 1:1 `conversationId` instead of the `teamId`.

## Fetching the list of channels in a team

Your bot can query the list of channels in a team. Note: right now the `General` channel is returned with `null` as the name, as this default channel allows for localization.  Also note: the `General` channel id always matchs the team Id.

#### REST API sample

You can directly issue a GET request to `/teams/{teamId}/conversations/` resource.

```json
GET /v3/teams/19%3A033451497ea84fcc83d17ed7fb08a1b6%40thread.skype/conversations

Response body
{
    "conversations": [{
        "id": "19:033451497ea84fcc83d17ed7fb08a1b6@thread.skype",
        "name": null
    }, {
        "id": "19:cc25e4aae50746ecbb11473bba24c70a@thread.skype",
        "name": "Materials"
    }, {
        "id": "19:b7b84cba410c406ba671dbbf5e0a3519@thread.skype",
        "name": "Design"
    }, {
        "id": "19:fc5db2aed489454e8f8c06829ed6c986@thread.skype",
        "name": "Marketing"
    }]
}
```

#### .NET SDK sample

Note: This sample uses the [new Microsoft Teams .NET SDK `FetchChannelList` call](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams):

```csharp
 ConversationList channels = client.GetTeamsConnectorClient().Teams.FetchChannelList(activity.GetChannelData<TeamsChannelData>().Team.Id);
```

#### Node SDK sample

Note: this sample uses [the new Microsoft Teams Node.js SDK `fetchChannelList` call](https://www.npmjs.com/package/botbuilder-teams):

```javascript
var teamId = session.message.sourceEvent.team.id;
  connector.fetchChannelList(
    (session.message.address).serviceUrl,
    teamId,
    (err, result) => {
      if (err) {
        session.endDialog('There is an error');
      }
      else {
        session.endDialog('%s', JSON.stringify(result));
      }
    }
);
```