# Frequently Asked Questions

## Setting up

### What technology should I use to build my bot or tab?

As tabs are web content you build and deploy, you may use any technology you wish.  

As bots must be built with the Bot Framework, we recommend you use one of the languages supported by the SDKs provided by the framework:  .NET / C# or Node.js.  While the Bot Framework also provides REST APIs usable by any language you choose, the SDKs provide additional functionality and helper functions to make the development process easier.

### Where do I sign up to start building Teams apps?

There are no prerequisites or additional development accounts to start building app for Microsoft Teams.  While you must have access to Microsoft Teams via an Office 365 subscription, no other sign-up or program access is required.  You do need to create accounts for the Bot Framework should you build a bot, and create a Dev Center account should you submit your app for publication.

## Bots

### How can my bot access or listen to all messages in a channel?

Bots in channels only receive messages when they are explicitly @mentioned.  There is no way to grant your bot access to conversations in which they are not mentioned.

## Distribution

### How can I distribute my Microsoft Teams app?

Microsoft Teams apps can be distributed to end users via the Office Store and in-product app discover system.  For more information, see [Submit your solutions to the Office Store](submission.md).

Microsoft Teams app packages can be manually distributed to your colleagues or other end users, and sideloaded by them via the [sideloading process](sideload.md).  Please note that apps distributed in this format are not tested, validated, or trusted by Microsoft. 

### What account do I use to create an Office Store / Dev Center account?

Microsoft Store developer accounts are based off of a [Microsoft Account (MSA)](https://account.microsoft.com/account), so you should use an existing MSA or create a new one for this purpose.  

If you already have a Microsoft Store developer account, you must use the original owner MSA for the Seller Dashboard experience.  While the Windows Store portal allows AAD association, the Seller Dashboard is a separate system that only works with your original MSA.

---
>Not seeing your question?  Submit your own - we listen to the developer community across a [several channels](feedback.md).