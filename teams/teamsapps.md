# Apps in Microsoft Teams

Apps in Microsoft Teams allow you to make your service available to users within Teams in contexts that make sense, all through one Teams App package that users can acquire through our in-product gallery, via the Office Store, or sideloaded directly by a team.  Whether assisting in chat with a bot or compose extension, or creating a rich web view of the content channel members care about, Teams provides a single packaging option for you to create new or leverage existing functionality within the Microsoft Teams experiences.

## App bar

>New feature

The app bar is the area on the left side of Microsoft Teams where your activity feed, chat, teams, files, and meetings buttons live. We designed the app bar to serve as a central container giving users quick access to the features that they use the most.

The app bar provides a quick and easy way to access and discover apps to help users and their teams be more productive in Microsoft Teams.  All users' existing Teams apps, no matter where they have been added and what context they are used, can be accessed from this one central location. This allows users to get quick access to their personal contexts as well as provide a quick way to jump to the channels where the app may be pinned.

![Microsoft Teams App bar](images/appbar_apps_flyout.png)

## Context in teams - Scopes

Your experience can choose which context, or "scopes", it supports in Microsoft Teams.  "Personal" scoped apps appear in an individual context, via the app bar or with bots in one-on-one conversations.  "Team" scoped apps appear within team channels, for example with configurable tabs or bots as team members.  You designate which scopes your app supports via the `scopes` object for each capability in your app's [manifest file](schema.md).

Please note that these are non-exclusive options - you can choose one or both, and can be independently set for each capability type.  Also note that not all capability types support both scopes, so please review the [capability](appcapabilities.md) information for more on scope behavior.

For advice on providing a great apps experience in either or both scopes, please review our [design guidelines](design.md).

# Building a Microsoft Teams app

Ready to get started adding your experience into Teams?  Here's how.

## Build your app's rich capabilities
* [Set up for development](setup.md)
* Define [capabilities](appcapabilities.md)
* Code your app

## Package & test your app within Teams
* [Create package](createpackage.md)
* [Side load](sideload.md) in Teams
* Test functionality

## Publish your app & drive engagement
* [Register and publish](submission.md) to Office Store
* Embed deep link on website
