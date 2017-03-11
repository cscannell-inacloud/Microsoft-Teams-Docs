# Team update events 

Teams will send events to your bot based on changes that happen within the teams they've been added to.  The following enumerate the events that your bot can receive and take action on.

## Bot added to team 

When your bot is added to a team or by a user for 1:1 chat, it will receive a [conversationUpdate](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#conversationUpdate) event.  

For bots added to a team, the payload will include the `team` object in `channelData`, as well as the `"eventType": "teamMemberAdded"` in the `channelData` object.  You can tell that the event is for a team update by checking for that `team` object in channelData. 

### Schema example
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

## Team member added/removed

Your bot will be notified when a team member is added or removed.  For added members, the payload will look similar to the bot addition.


### Schema example - TeamMemberAdd 
```json 
{     
    "membersAdded": [         
        {             
            "id": "29:1_LCi5Up14pAy65yZuaJzG1uIT7ujYhjjSTsUNqjORsZHjLHKiQIBJa4cX2XsAsRoaY7va2w6ZymA9-1VtSY_g"         
        }     
    ],     
    "type": "conversationUpdate",     
    "timestamp": "2017-02-23T19:37:33.092Z", 
    "id": "f:9ffc452e",     
    "channelId": "msteams",     
    "conversation": {
        "isGroup": true,         
        "id": "19:efa9296d959346209fea44151c742e73@thread.skype"     
    }
}
```
 
### Schema example - Team Member Removed
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
 
## Channel created (new) 

Your bot will be notified when a channel is created. 

### Schema example 
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

## Channel renamed (new) 

Your bot will be notified when a channel is renamed. 

### Schema example 
```json
{     
    "type": "conversationUpdate",     
    "timestamp": "2017-02-23T19:25:10.172Z",     
    "id": "f:c7eaddbe",     
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
            "id": "19:e4039002cc994705b37783c98abb109a@thread.skype",             
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
} 
```

## Channel Deleted (new) 

Your bot will be notified of any channel deletion that occurs in a team it is a part of.
 
### Schema example
```json 
{     
    "type": "conversationUpdate",     
    "timestamp": "2017-02-23T19:35:32.847Z",     
    "id": "f:32277cde",     
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
        "channel": {             
            "id": "19:6d97d816470f481dbcda38244b98689a@thread.skype",             
            "name": "12sss"         
        },         
        "team": {             
            "id": "19:efa9296d959346209fea44151c742e73@thread.skype"         
        },         
        "eventType": "channelDeleted",         
        "tenant": {             
            "id": "72f988bf-86f1-41af-91ab-2d7cd011db47"         
        }     
    } 
} 
 ```


