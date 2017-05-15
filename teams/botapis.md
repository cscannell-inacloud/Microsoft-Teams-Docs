# Bot APIs

Your bot can access additional context about the team or chat, such as user profile.  This information can be used to enrich your bot's functionality and provide a more personalized experience.


## Fetching the team roster

Your bot can query for the list of team members. With the BotBuilder SDK, call  [`GetConversationMembersAsync()` in the .NET SDK](https://docs.botframework.com/en-us/csharp/builder/sdkreference/d7/d08/class_microsoft_1_1_bot_1_1_connector_1_1_conversations_extensions.html#a0a665865891d485956e52c64bce84d4b) to return a list of user Ids for the `team.id` retrieved from the `channelData` of the inbound schema.

#### REST API sample

You can directly issue a GET request to [`/conversations/{teamId}/members/`](https://docs.botframework.com/en-us/restapi/connector/#!/Conversations/Conversations_GetConversationMembers) resource using the `teamId` as the parameter in the API call.

```json
GET /v3/conversations/19:ja0cu120i1jod12j@skype.net/members

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

## Fetching user profile in 1:1 chat

You can also make the same API call for any 1:1 chat to obtain the profile information of the user chatting with your bot.

The API call and SDK methods are identical to fetching team roster, as is the response object. The only different is that you pass the 1:1 `conversationId` instead of the `teamId`.

## Fetching the list of channels in a team

Your bot can query the list of channels in a team. Note: right now the `General` channel is not returned in the list. You can reuse the team's ID as the channel ID.

#### REST API sample

You can directly issue a GET request to `/conversations/{teamId}/channels/` resource.

```json
GET /v3/conversations/19:ja0cu120i1jod12j@skype.net/channels

Response body
{
    "conversations": [{
        "id": "19:skypespaces_24bb9d6ac6264999b353ba860ecd4044@thread.skype",
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