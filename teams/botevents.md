# Microsoft Teams bot events

Teams will send notifications to your bot for changes or events that happen in contexts where your bot is active.  You can use these events to trigger service logic, such as:

* Trigger welcome message when your bot is added to a team.
* Query and cache team information when the bot is added to a team.
* Upated cached information on team membership or channel information.
* Remove cached information for a team if the bot is removed.

All bot events are sent as an `Activity` object where `messageType` defines what information is in the `Activity` object. For messages of type `message`, see [Bot Conversation Overview](botsconversation.md).

Teams-specific events, usually triggered off the `conversationUpdate` type, will have additional Teams event information passed as part of the `channelData` object, and therefore your event handler must query the `channelData` payload for the Teams `eventType` and additional event-specific metadata.

The following enumerate the events that your bot can receive and take action on:
|Type|Payload object|Teams `eventType` |Description|
|---|---|---|---|
| `conversationUpdate` |`membersAdded`| `teamMemberAdded`|[Bot or team member was added to team](botevents.md#team-member-or-bot-addition)|
| `conversationUpdate` |`membersRemoved`| `teamMemberRemoved`|[Bot or team member was removed from team](botevents.md#team-member-or-bot-removed)|
| `conversationUpdate` | |`teamRenamed`| [Team in which bot is member was renamed](botevents.md#team-name-updates)|
| `conversationUpdate` | |`channelCreated`| In a team where bot is member, [a channel was created](botevents.md#channel-updates)|
| `conversationUpdate` | |`channelRenamed`| In a team where bot is member, [a channel was renamed](botevents.md#channel-updates)|
| `conversationUpdate` | |`channelDeleted`| In a team where bot is member, [a channel was deleted](botevents.md#channel-updates)|
| _`contactRelationUpdate`_ | | | This [Bot Framework provided ActivityType](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#contactrelationupdate) is not directly supported in Microsoft Teams. |
| _`deleteUserData`_ | | | This [Bot Framework provided ActivityType](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#deleteuserdata) is not directly supported in Microsoft Teams |

# Activities

## Team member or Bot addition

The [conversationUpdate](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#conversationUpdate) event is sent to your bot when it receives information on membership updates for teams where it has been added.  It will also receive an update when it has been added for the first time specifically for 1:1 conversations.  Note that the user information (`Id`) is unique for your bot, and may be cached for future use by your service (e.g. direct message a specific user.)

### Bot or user added to a team

The `conversationUpdate` event with the `membersAdded` object in the payload will be sent when either a bot is added to a team, or a new user is added to a team where a bot has been added. Teams also adds the `"eventType.teamMemberAdded"` in the `channelData` object.

Since this event is sent in both cases, you should parse out the `membersAdded` object to determine if the addition was a user or the bot itself.  For the latter, it is a best practice to send a Welcome Message to the channel to users can understand what features your bot provides.

#### Example code - checking to see if bot was the added member 

##### .NET SDK

```csharp
    for (int i = 0; i < sourceMessage.MembersAdded.Count; i++)
    {
        if (sourceMessage.MembersAdded[i].Id == sourceMessage.Recipient.Id)
        {
            addedBot = true;
            break;
        }
    }
```

##### Node.js
```javascript
const builder = require('botbuilder');

var c = new builder.ChatConnector({appId: BOT_APP_ID, appPassword: .BOT_SECRET});
var bot = new builder.UniversalBot(c);

bot.on('conversationUpdate', (msg) => {
    var members = msg.membersAdded;
    // Loop through all members that were just added to the team
    for (var i = 0; i < members.length; i++) {

        // See if the member added was our bot
        if (members[i].id.includes(BOT_APP_ID)) {
            var botmessage = new builder.Message()
                .address(msg.address)
                .text('Hello World!');

            bot.send(botmessage, function(err) {});
        }
    }
});
```

#### Schema example - bot added to team
```json
{     
    "membersAdded": [         
        {             
            "id": "28:f5d48856-5b42-41a0-8c3a-c5f944b679b0"
        }     
    ],     
    "type": "conversationUpdate",
    "timestamp": "2017-02-23T19:38:35.312Z",
    "id": "f:5f85c2ad",     
    "channelId": "msteams",     
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",     
    "from": {
        "id": "29:1I9Is_Sx0OIy2rQ7Xz1lcaPKlO9eqmBRTBuW6XzkFtcjqxTjPaCMij8BVMdBcL9L_RwWNJyAHFQb0TRzXgyQvA"
    },     
    "conversation": {         
        "isGroup": true,         
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype"     
    },     
    "recipient": {         
        "id": "28:f5d48856-5b42-41a0-8c3a-c5f944b679b0",         
        "name": "SongsuggesterBot"     
    },     
    "channelData": {         
        "team": {             
            "id": "19:efa9296d959346209fea44151c742e73@thread.skype"         
        },         
        "eventType": "teamMemberAdded",         
        "tenant": {             
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"         
        }     
    } 
} 
```

### Bot added for personal (1:1) context only

Your bot will receive a `conversationUpdate` with `membersAdded` when a users adds it directly for 1:1 chat.  In this case, the payload that your bot receives will not contain the `channelData.team` object.  You should use this as a filter in case you wish your bot to offer a different Welcome depending on personal or team addition.

## Team member or Bot removed

The `conversationUpdate` event with the `membersRemoved` object in the payload will be sent when either your bot is removed from a team, or a user is removed from a team where a bot has been added. Teams also adds the `"eventType.teamMemberRemoved"` in the `channelData` object.  Once again, you should parse the `membersRemoved` object for your bot id to determine who was removed.


#### Schema example - Team Member Removed
```json
{     
    "membersRemoved": [         
        {             
            "id": "29:1_LCi5Up14pAy65yZuaJzG1uIT7ujYhjjSTsUNqjORsZHjLHKiQIBJa4cX2XsAsRoaY7va2w6ZymA9-1VtSY_g"         
        }     
    ],     
    "type": "conversationUpdate",     
    "timestamp": "2017-02-23T19:37:06.96Z",     
    "id": "f:d8a6a4aa",     
    "channelId": "msteams",     
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",     
    "from": {         
        "id": "29:1I9Is_Sx0OIy2rQ7Xz1lcaPKlO9eqmBRTBuW6XzkFtcjqxTjPaCMij8BVMdBcL9L_RwWNJyAHFQb0TRzXgyQvA"     
    },     
    "conversation": {         
        "isGroup": true,         
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype"     
    },     
    "recipient": 
    {         
        "id": "28:f5d48856-5b42-41a0-8c3a-c5f944b679b0",         
        "name": "SongsuggesterBot"     
    },     
    "channelData": {         
        "team": {             
            "id": "19:efa9296d959346209fea44151c742e73@thread.skype"         
        },         
        "eventType": "teamMemberRemoved",         
        "tenant": {             
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"         
        }     
    } 
}
``` 

## Team name updates

>Note: at this time, there is no functionality to query all team names, and team name is not returned in other payloads beside this event.

Your bot will be notified when the team it is in has been renamed.  It will receive a `conversationUpdate` event with  `"eventType.teamRenamed"` in the `channelData` object.  Please note that there are no notifications for team creation or deletion, since bots only exist as part of teams and have no visibility outside the scope in which they have been added.

#### Schema example - Team renamed
```json
{ 
    "type": "conversationUpdate", 
    "timestamp": "2017-02-23T19:35:56.825Z", 
    "id": "f:1406033e", 
    "channelId": "msteams", 
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/", 
    "from": { 
        "id": "29:1I9Is_Sx0O-Iy2rQ7Xz1lcaPKlO9eqmBRTBuW6XzkFtcjqxTjPaCMij8BVMdBcL9L_RwWNJyAHFQb0TRzXgyQvA" 
    }, 
    "conversation": { 
        "isGroup": true, 
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype" 
    }, 
    "recipient": { 
        "id": "28:f5d48856-5b42-41a0-8c3a-c5f944b679b0", 
        "name": "SongsuggesterLocal" 
    }, 
    "channelData": { 
        "team": { 
            "id": "19:efa9296d959346209fea44151c742e73@thread.skype", 
            "name": "New Team Name" 
        }, 
        "eventType": "teamRenamed", 
        "tenant": { 
           "id": "72f988bf-86f1-41af-91ab-2d7cd011db47" 
        } 
    } 
} 
```
 
## Channel updates

Your bot will be notified when a channel is created, renamed, or deleted, in a team where it has been added.  Once again, the `conversationUpdate` event will be received, and Teams-specific event identifier will be sent as part of the `channelData.eventType` object, where the channel data's  `channel.id` is the GUID for the channel, and `channel.name` contains the channel name itself.  

The channel events are:
* **channelCreated** - a user adds a new channel to the team
* **channelRenamed** - a user renames an existing channel
* **channelDeleted** - a user removes a channel

#### Full schema example (channelCreated)
```json
{     
    "type": "conversationUpdate",     
    "timestamp": "2017-02-23T19:34:07.478Z",     
    "id": "f:dd6ec311",     
    "channelId": "msteams",     
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",     
    "from": {         
        "id": "29:1wR7IdIRIoerMIWbewMi75JA3scaMuxvFon9eRQW2Nix5loMDo0362st2IaRVRirPZBv1WdXT8TIFWWmlQCizZQ"     
    },     
    "conversation": {         
        "isGroup": true,         
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype" 
    },     
    "recipient": {         
        "id": "28:f5d48856-5b42-41a0-8c3a-c5f944b679b0",         
        "name": "SongsuggesterBot"     
    },     
    "channelData": {         
        "channel": {             
            "id": "19:6d97d816470f481dbcda38244b98689a@thread.skype",             
            "name": "FunDiscussions"         
        },         
        "team": {             
            "id": "19:efa9296d959346209fea44151c742e73@thread.skype"         
        },         
        "eventType": "channelCreated",         
        "tenant": {             
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"         
        }     
    } 
} 
```

#### Schema snippet - channelData for channelRenamed 
```json
...
"channelData": {         
    "channel": {             
        "id": "19:6d97d816470f481dbcda38244b98689a@thread.skype",             
        "name": "PhotographyUpdates"         
    },         
    "team": {             
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype"         
    },         
    "eventType": "channelRenamed",         
    "tenant": {             
        "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"         
    }     
} 
...
```

#### Schema snippet - channelData for channelDeleted 
```json     
...
"channelData": {         
    "channel": {             
        "id": "19:6d97d816470f481dbcda38244b98689a@thread.skype",               
        "name": "PhotographyUpdates"       
    },         
    "team": {             
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype"         
    },         
    "eventType": "channelDeleted",         
    "tenant": {             
        "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"         
    }     
} 
...
```


