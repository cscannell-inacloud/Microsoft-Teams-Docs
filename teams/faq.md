# Frequently Asked Questions

# Setting up

## What languages can I build my bot or tab in?

As tabs are web content you build and deploy, you may use any technology you wish.  

As bots must be built with the Bot Framework, we recommend you use one of the languages supported by the SDKs provided by the framework:  .NET / C# or Node.js.  While the Bot Framework also provides REST APIs usable by any language you choose, the SDKs provide additional functionality and helper functions to make the development process easier.

## Where do I sign up to start building Teams apps?

There are no prerequisites or additional development accounts to start building app for Microsoft Teams.  While you must have access to Microsoft Teams via an Office 365 subscription, no other sign-up or program access is required.  You do need to create accounts for the Bot Framework should you build a bot, and create a Dev Center account should you submit your app for publication.

## What email should I use to register my bot / create a Dev Center account?

In general we recommend you use a Microsoft Account (MSA) specifically created for this purpose, as opposed to a personal account.  If you are building and testing on a O365 Developer tenant, **do not** use an email from this account for any registration as the Developer tenant is subject to expiration.

# Bots

## How can my bot access or listen to all messages in a channel?

Bots in channels only receive messages when they are explicitly @mentioned.  There is no way to grant your bot access to conversations in which they are not mentioned.

# Distribution

## How can I distribute my Microsoft Teams app?

Microsoft Teams apps can be distributed to end users via the Office Store and in-product app discover system.  For more information about submitting to the Store, see here.

Microsoft Teams app packages can be manually distributed to your colleagues end users, and sideloaded via the [sideloading process](sideload.md).  Please note that apps distributed in this format are not tested, validated, or trusted by Microsoft. 

>Not seeing your question?  Submit your own - we listen to the developer community across a [several channels](feedback.md).