# Bot menus

>Note: bot menus are available in [Public Developer Preview](publicpreview.md) only.  Additionally, many features in this document are under construction, and as such, subject to change.


To aid discovery and to help educate users about your bot's functionality, you can now add menus that surface whenever the user interacts with your bot. The menu will show the command text and also provide help text, such as a usage example or description of the command's purpose.

When a user selects a menu item, the command string is inserted into the text box to aid in user completion of the bot message.


![Bot menu example](images/Bot/botmenusmall.png)

## App manifest

To create a bot menu, add a new [`commandLists`](schema.md#bots-commandlists) object to your app manifest under the bot section. You can declare individual menus with separate commands for 1:1 chat (`scopes`: `personal`) and channels (`scopes`: `team`). Each menu supports up to 10 commands.

### Manifest excerpt - single menu for both scopes

```json
{
  ...
  "bots":[
    {
      "botId":"[your bot id]",
      "scopes": [
        "personal",
        "team"
      ],
      "commandLists":[
        {
          "scopes":[
            "team",
            "personal"
          ],
          "commands":[
            {
              "title":"Help",
              "description":"Displays this help message"
            },
            {
              "title":"Search Flights",
              "description":"Search flights from Seattle to Phoenix May 2-5 departing after 3pm"
            },
            {
              "title":"Search Hotels",
              "description":"Search hotels in Portland tonight"
            },
            {
              "title":"Best Time to Fly",
              "description":"Best time to fly to London for a 5 day trip this summer"
            }
          ]
        }
      ]
    }
  ],
  ...
}
```

#### Manifest excerpt - separate menu per scope

```json
{
  ...
  "bots":[
    {
      "botId":"[your bot id]",
      "scopes": [
        "personal",
        "team"
      ],
      "commandLists":[
        {
          "scopes":[
            "team"
          ],
          "commands":[
            {
            "title":"help",
            "description":"Displays this help message for channels"
            }
          ]
        },
        {
          "scopes":[
            "personal"
          ],
          "commands":[
            {
            "title":"help",
            "description":"Displays this help message for 1:1 chat"
            }
          ]
        }
      ]
    }
  ],
  ...
}
```

## Best practices

* Keep it simple: the bot menu is meant to present the key 3-5 capabilities or commands of your bot.

* Keep it short: menu options shouldn’t be extremely long and complex natural language statements – they should be simple commands.

* Always available: bot menu actions/commands should be always invokable, regardless of what state of the conversation or dialog the bot is in.