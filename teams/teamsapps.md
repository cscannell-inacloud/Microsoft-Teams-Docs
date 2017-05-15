# Apps in Microsoft Teams

Apps in Microsoft Teams allow you to make your service available to users within Teams in the contexts - or "scopes" - that make the most sense.

* You can use most app capabilities (bots, tabs, etc) in most scopes.  Please review the [individual capabilities](appcapabilities.md) for more on their scope behavior.

* You offer all the capabilities you support, across all scopes, through one Teams app package that users can acquire through our in-product app gallery, via the Office Store, or sideloaded directly by your team.  Specifically, you declare this via the `scopes` object for each capability in your app package's [manifest file](schema.md).

## App bar (personal scope)

>New feature

The app bar is the area on the left side of Microsoft Teams where the user's activity feed, chat, teams, files, and meetings buttons live. We designed the app bar to serve as a central container giving users quick access to the features that they use the most.

![Microsoft Teams Apps bar](images/appbar_apps_flyout.png)

Users can access personal experiences from your app via the app bar, such as holding personal conversations with your app's bot or interacting with personal data in the a tab.

<!-- TODO screenshot of personal UIs bot and tab  -->

## Interacting in channel (team scope)

Users can access team experiences from your app in a channel, such as @mentioning your app's bot, configuring notififcations via a Connector, or interacting with team data in a tab.  

<!-- TODO screenshot of team UIs, bot and tab -->

## Building an app

Ready to get started adding your experience into Teams?  Follow these steps:

### Build your app's rich capabilities
* [Set up for development](setup.md).
* [Design your app](design.md).
* Define [capabilities](appcapabilities.md).
* Code your app.

### Package & test your app within Teams
* [Create package](createpackage.md).
* [Side load](sideload.md) in Teams.
* Test functionality.

### Publish your app & drive engagement
* [Register and publish](submission.md) to Office Store.
* Embed deep link on website.
