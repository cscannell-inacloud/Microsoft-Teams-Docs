# Sending and receiving messages

Much but not all of the functionality supported by the Bot Framework is supported in Bots on Microsoft Teams.  For more information about core messaging functionality of the Bot Framework, please review the [documentation](https://docs.botframework.com/en-us/core-concepts/messages/#navtitle).

##Messages
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

To send a welcome message, listen for the [conversationUpdate](https://docs.botframework.com/en-us/csharp/builder/sdkreference/activities.html#conversationUpdate) activity.


### Picture messages

Pictures are sent by adding attachments to a message.

Pictures can be PNG, JPEG or GIF up to 20Mb.

## Cards and buttons

Microsoft Teams supports the following cards which may have several properties and attachments. You can find information on how to use cards in the [.NET SDK](https://docs.botframework.com/en-us/csharp/builder/sdkreference/attachments.html#richcards) and [Node.js SDK](https://docs.botframework.com/en-us/node/builder/guides/examples/#demo-skype-calling) docs.

* Hero card
* Thumbnail card
* Carousel card (with hero or thumbnail cards)
* List card

> **Note:** Microsoft Teams cards currently only support openUrl and imBack actions. Receipt cards are not supported at this time.

### Inline card images

Your card can contain inline images by including a link to the image content hosted on a public CDN.

Images are scaled up or down in size while maintaining the aspect ratio to cover the image area, and then cropped from center to achieve the image aspect ratio for the card.

Images must be at most 1024x1024 and 1MB in size, and in either PNG or JPEG format.

| Property | Type  | Description |                                                           
|:-------|:-------|:------------|
| url | URL | HTTPS URL to the image |
| alt | String | Accessible description of the image |

### Buttons

Buttons are shown stacked at the bottom of the card. Button text is always on a single line and will be truncated if the text exceeds the button width. Any additional buttons beyond the maximum number supported by the card will not be shown.

### Actions

| Property | Type  | Description |                                                           
|:-------|:-------|:------------|
| type | String | Required field. One of openURL (opens the given URL) or imBack (posts a message in the chat to the bot that sent the card) |
| title | String | Text description that appears on the button |
| value | String |  For openURL is a URL, and for imBack is a user defined string |

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

