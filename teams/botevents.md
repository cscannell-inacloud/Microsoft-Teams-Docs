# Team update events 

> New features

Teams will send events to your bot based on changes that happen in contexts where they are active.  The following enumerate the events that your bot can receive and take action on.

## Adding and removing members

The [conversationUpdate](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#conversationUpdate) event is sent to your bot when it receives information on membership updates for teams where it has been added.  It will also receive an update when it has been added for the first time specifically for 1:1 conversations.

### Adding to a team

The `conversationUpdate` event with the `membersAdded` object in the payload will be sent when either a bot is added to a team, or a new user is added to a team where a bot has been added. Teams also adds the `"eventType.teamMemberAdded"` in the `channelData` object.

Since this event is sent in both cases, you should parse out the `membersAdded` object to determine if the addition was a user or the bot itself.  For the latter, it is a best practice to send a Welcome Message to the channel to users can understand what features your bot provides.

#### Example code - checking to see if bot was the added member (C#)

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

### Adding a bot for 1:1 context only

Your bot will also receive a `conversationUpdate` with `membersAdded` when a users adds it directly for 1:1 chat.  In this case, the payload that your bot receives will not contain the `channelData.teams` object.  You should use this as a filter in case you wish your bot to offer a different Welcome depending on 1:1 or team addition.

### Removing a team member

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

## Team updates

Your bot will be notified when the team its in has been renamed.  It will recieve a `conversationUpdate` event with  `"eventType.teamRenamed"` in the `channelData` object.  Please note that there is no notification for team creation or deletion, since bots only exist as part of teams and have no visibility outside the scope in which they have been added.

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


