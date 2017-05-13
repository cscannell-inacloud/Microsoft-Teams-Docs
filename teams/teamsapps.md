# Apps in Microsoft Teams

Apps in Microsoft Teams allow you to make your services available to users in contexts that make sense, all through one Teams App package that users can acquire through our in-product gallery, via the Office Store, or sideloaded directly by a team.  Whether assisting in chat with a bot or compose extension, or creating a rich web view of the content channel members care about, Teams provides a single packaging option for you to create new or leverage existing functionality within the Microsoft Teams experiences.

## Apps bar

>New feature

The Apps button on the left-hand navagation bar launches the Apps bar. We designed the Apps bar to serve as a central container giving users quick access to the features that they use the most.  Users can also discover and add new Microsoft Teams apps through the "Discover apps" button.  

All apps added by a user or included in any of their teams can be accessed from this one central location. This allows users to get quick access to their personal contexts with that app, showing for example their 1:1 conversations with the application's bot or static tabs.

![Microsoft Teams Apps bar](images/appbar_apps_flyout.png)

## Context in Team Apps

Teams apps are context-aware, and you can control which contexts, or "scopes", your experience will support.  "Personal" scoped apps appear in an individual context, via the app bar or with bots in one-on-one conversations.  "Team" scoped apps appear within team channels, for example with configurable tabs or bots as team members.  You designate which scopes your app supports via the `scopes` object for each capability in your app's [manifest file](schema.md).

Please note that these are non-exclusive options - one or both can be chosen, and can be independently set for each capability type.  Also note that not all capability types support both scopes, so please review the [capability](appcapabilities.md) information for more on scope behavior.

For advice on providing a great Microsoft Teams app experience in either or both scopes, please review our [design guidelines](design.md).

## Building an app

Ready to get started adding your experience into Teams?  Follow these steps:

### Build your app's rich capabilities
* [Set up for development](setup.md).
* Define [capabilities](appcapabilities.md).
* Code your app.

### Package & test your app within Teams
* [Create package](createpackage.md).
* [Side load](sideload.md) in Teams.
* Test functionality.

### Publish your app & drive engagement
* [Register and publish](submission.md) to Office Store.
* Embed deep link on website.
