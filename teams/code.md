# Code your Microsoft Teams app

As Microsoft Teams apps are composed web services, you can use any web programming technology.  For bots and compose extensions, we recommend you use either C# or Typescript to take advantage of the .NET and Node.js SDKs.

## Coding your tab

Tabs are simply iframe'd web content.  You can leverage your existing web service, written in any language and hosted on any cloud platform, and simply include the Microsoft Teams Javscript library in pages you display in Teams.

## Coding your bot and compose extension

As your Teams bot and compose extension are built upon the Bot Framework, we recommend you leverage the existing Bot Framework SDKs provided in either .NET/C# or Node.js. You may choose to develop in any other web programming technology and call the Bot Framework REST APIs directly as well, but must perform all token handling yourself.

### Microsoft Teams Bot Extension SDK

We also provide an extension to the Bot Builder SDK.  This SDK provides helper functions and models for Microsoft Teams-specific functionality like fetching team and channel information, and models for Team channel data.