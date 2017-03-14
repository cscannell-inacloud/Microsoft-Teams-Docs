# Sending and receiving messages

Much but not all of the functionality supported by the Bot Framework is supported in Bots on Microsoft Teams.  For more information about core messaging functionality of the Bot Framework, please review the [documentation](https://docs.botframework.com/en-us/core-concepts/messages/#navtitle).

## Messages
Your bot can send rich text, pictures and cards. Users can send rich text and pictures to your bot. You can specify the type of content your bot can handle in the Microsoft Teams settings page for your bot.

| Format | From user to bot  | From bot to user |  Notes |                                                           
|:-------|:-------|:------------|:-------|
| Rich text | ✔ | ✔ | No emoticons |  
| Pictures | ✔ | ✔ | PNG, JPEG or GIF up to 20Mb |
| Cards | ✘ | ✔ |  |

### Message format
You can set the optional [TextFormat](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html) property to control how your message's text content will be rendered.

Microsoft Teams supports the following formatting options

| TextFormat Value | Description |
|:----------------------|:------------------|
| plain | The text should be treated as raw text with no formatting applied at all |
| markdown | The text should be treated as markdown formatting and rendered on the channel as appropriate |
| xml | The text is simple XML markup (subset of HTML - see link above) |

> **Note:** On hero and thumbnail cards, message format is only supported on the text property. Formatting is not supported on the title and subtitle properties at this time.

### Welcome messages

To send a welcome message, listen for the [conversationUpdate](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#conversationUpdate) activity.  For more information, see [Team update events](botevents.md).


### Picture messages

Pictures are sent by adding attachments to a message.

Pictures can be PNG, JPEG or GIF up to 20Mb.

## Cards 

Microsoft Teams supports the following cards which may have several properties and attachments. You can find information on how to use cards in the [.NET SDK](https://docs.botframework.com/en-us/csharp/builder/sdkreference/attachments.html#richcards) and [Node.js SDK](https://docs.botframework.com/en-us/node/builder/guides/examples/#demo-skype-calling) docs.

* Hero card
* Thumbnail card
* Signin card

> **Note:** Receipt cards are not supported at this time.

Additionally, we support the following layouts:
* Horizontal carousel layout
* Vertical list layout

Both layouts support hero and thumbnail cards.

### Inline card images

Your card can contain inline images by including a link to the image content hosted on a public CDN.

Images are scaled up or down in size while maintaining the aspect ratio to cover the image area, and then cropped from center to achieve the image aspect ratio for the card.

Images must be at most 1024x1024 and 1MB in size, and in either PNG or JPEG format.

| Property | Type  | Description |                                                           
|:-------|:-------|:------------|
| url | URL | HTTPS URL to the image |
| alt | String | Accessible description of the image |


### Hero card

The [hero card](https://docs.botframework.com/en-us/csharp/builder/sdkreference/attachments.html#herocard) renders a title, subtitle, text, large image and buttons.

!["Example of a hero-card"](images/Bots_HeroCarExample.PNG)

| Property | Type  | Description |                                                           
|:-------|:-------|:------------|
| title | Rich text | Title of the card. Maximum 2 lines |
| subtitle | Rich text | Subtitle of the card. Maximum 2 lines |
| text | Rich text | Text appears just below the subtitle |
| images: [] | Array of images | Image displayed at top of card. Aspect ratio 16:9 |
| buttons: [] | Array of action objects | Set of actions applicable to the current card. Maximum 6. |
| tap | Action object | This action will be activated when the user taps on the card itself |

### Thumbnail card

The [thumbnail card](https://docs.botframework.com/en-us/csharp/builder/sdkreference/attachments.html#thumbnailcard) renders a title, subtitle, text, small thumbmail image and buttons.

!["Example of a thumbnail-card"](images/Bots_ThumbnailExample.png)

| Property | Type  | Description |                                                           
|:-------|:-------|:------------|
| title | Rich text | Title of the card. Maximum 2 lines |
| subtitle | Rich text | Subtitle of the card. Maximum 2 lines |
| text | Rich text | Text appears just below the subtitle |
| images: [] | Array of images | Image displayed at top of card. Aspect ratio 1:1 (square) |
| buttons: [] | Array of action objects | Set of actions applicable to the current card. Maximum 6. |
| tap | Action object | This action will be activated when the user taps on the card itself |


### Carousel layout

The [carousel layout](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html) can be used to show a carousel of cards, with associated action buttons.

!["Example of a carousel of cards"](images/Bots_CarouselExample.png)

Properties are the same as for the hero or thumbnail card.

### List layout

The list layout can be used to show a vertically stacked list of cards.

!["Example of a list of cards"](images/Bots_ListExample.PNG)

Properties are the same as for the hero or thumbnail card.

> **Note:** Some combinations of list cards may not be supported yet on iOS and Android.

## Buttons

Buttons are shown stacked at the bottom of the card. Button text is always on a single line and will be truncated if the text exceeds the button width. Any additional buttons beyond the maximum number supported by the card will not be shown.

**Action Types supported by Teams**

| Property | Type  | Description |                                                           
|:-------|:-------|:------------|
| type | String | Required field. Teams supports openURL, imBack, and invoke |
| title | String | Text description that appears on the button |
| value | String |  The payload of the action, which will vary based on the event (see below) |

### Action - openUrl

This action type specifies a URL to launch, in the default browser.  Note that your bot does not receive any notice on which button was clicked.

The `value` field must contain a full and properly formed URL.

```json
{
    "type": "openUrl",
    "title": "Tabs in Teams",
    "value": "https://msdn.microsoft.com/en-us/microsoft-teams/tabs"
}
```

### Action - imBack

This action basically trigger a return message to your bot, as if the user typed it in a normal chat message.  Thus, your user, and all other users if in a channel, will see that button response.

The `value` field should contain the text string echoed in the chat and therefore sent back to the bot.  This is the message text you will process in your bot to perform the desired logic.  Note: this field is a simple string - there is no support for formatting or hidden characters.

```json
{
    "type": "imBack",
    "title": "More",
    "value": "Show me more"
}
```

### Action - invoke (New)

The new invoke message type silently sends a JSON payload that you define to your bot.  This is useful if you want to send more detailed information back to your bot without having to send via a simple imBack text string.  Note that the user, in 1:1 or in channel, sees no notification as a result of their click.

The `value` field will contain a stringified JSON object.  You can include identifiers or any other context necessary to carry out the operation.

```json
{
    "type": "invoke",
    "title": "Option 1",
    "value": "{\"option\": \"opt1\"}"
}
```

Your bot will receive the message with `"type": "invoke"` instead of `"message"`, and `value` will contain the stringified JSON object.



#### Invoke inbound schema example
```json
{
    "type": "invoke",
    "value": {
        "option": "opt1"
    },
    "timestamp": "2017-02-10T04:11:19.614Z",
    "id": "f:6894910862892785420",
    "channelId": "msteams",
    "serviceUrl": "https://smba.trafficmanager.net/amer-client-ss.msg/",
    "from": {
        "id": "29:1Eniglq0-uVL83xNB9GU6w_G5a4SZF0gcJLprZzhtEbel21G_5h-
    NgoprRw45mP0AXUIZVeqrsIHSYV4ntgfVJQ"
    },
    "conversation": {
        "id": "19:97b1ec61-45bf-453c-9059-6e8984e0cef4_8d88f59b-ae61-4300-bec0-
    caace7d28446@unq.gbl.spaces"
    },
    "recipient": {
        "id": "28:8d88f59b-ae61-4300-bec0-caace7d28446",
        "name": "MyBot"
    },
    "entities": [
        {
            "locale": "en-US",
            "country": "US",
            "platform": "Web",
            "type": "clientInfo"
        }
    ]
}
```
