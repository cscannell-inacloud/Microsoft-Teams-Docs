# Getting Started with Bots for Microsoft Teams (preview)

Build and connect intelligent bots to interact with Microsoft Teams users naturally through chat.  Or bring your existing bot from another channel, and add Teams-specific support to make your experience shine.  Or if you are just looking for a way to simply extend your team with basic messaging / control flow, check out our new Custom Bots feature. 

> **Don't have Microsoft Teams? Get a free Office 365 developer subscription or activate it for your existing Office 365 account. [Here's how](setup.md).**

## Bots in Microsoft Teams

At this time, Microsoft Teams bots support 1:1 and channel chats. They do not yet support group chats. 

Bots appear in Teams with a hexagonal avatar.  They always appear online and do not have a mood message.

You can also take advantage of Team's rich tabs web canvas view, to create a tab for your bot.  Use this to show information like FAQs, help, or other information that helps your user best interact with your experience.  See [here]() for more information.

## Overview of building a Microsoft Teams bot

Microsoft Teams supports much of the common BotFramework functionality.  Follow these steps to build a great Teams bot:

- Design a great bot: Microsoft Teams is the place for group and team collaboration.  Consider what functionality your bot can bring to this collaboration environment, via both 1:1 conversations, or when added as part of a channel conversation.  A great bot on Teams will also find ways to leverage the unique Tabs feature, via a [configurable channel tab](tabs.md), or a [pinned bot tab]().
- [Create and register your bot in the BotFramework](botscreate.md):  Take advantage of the great tools, documentation, and community provided by the BotFramework team.
- [Develop your bot](botsconversation.md): Add basic conversation flow and leverage channel-specific functionality. 
- Test your bot:  [Create a sideloadable bot package](createpackage.md) and [sideload it to a team](sideload.md) to see it in action.
- Publish your bot: _Coming soon_

## Overview of building a Custom bot

Custom bots allow you to create a simple bot for basic interaction, like kicking off a workflow or other simple commands you may need.  These Custom Bots live only in the team in which you create them, and is intended for simple processes specific to your and your company's workflow.  See [here](custombot.md) for more information.

>**Note:** We are looking for developers to build great bots for inclusion in Microsoft Teams app gallery.  Submit your proposal [here](https://aka.ms/microsoftteamsdeveloperpreviewinterestform).