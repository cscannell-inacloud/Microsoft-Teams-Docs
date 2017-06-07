# Sample applications for the Microsoft Teams Developer Platform

There are Microsoft Teams samples in our GitHub repositories for you to clone or download:

* [Getting started sample](https://github.com/OfficeDev/microsoft-teams-sample-get-started).  This sample shows all the capabilities available in Microsoft Teams app, including bots, tabs, compose extensions, and notification feeds.  Source code provided in both C# and Node.js.
* [Simple List tab sample](https://github.com/OfficeDev/microsoft-teams-sample-todo).  This Node.js sample shows how easy it is to convert an existing web app into a tab.


## Working with Microsoft Teams samples

While each project may have specific instructions for how to test and deploy, in general the following guidelines should be applicable to most samples.  Please note that in order to fully test within Microsoft Teams, you'll need to follow the Packaging and Sideloading guidelines to make the app available.  All samples should have a TeamsAppPackages directory which contains a sample manifest.json file and icons, to facilitate your app creation.

### Samples with Bots

#### Running Locally

By default, all samples with bots should work locally, by simply running the [Bot Framework Emulator](https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator) using the default ports (3978 for Node.js samples, 3979 for .NET/C# samples) and blank values for the Microsoft App ID and Password.  Note that the Emulator will only allow you to test basic 1:1 messages.  




### Samples with Tabs

Clone or download the desired sample from Github. Each sample code repository has a subfolder called TeamsAppsPackages, which contains one or more zips. Each zip contains a manifest.json file. Review the manifest.json file contents against the manifest schema so you understand how the app was built. Refer to the [manifest schema](https://msdn.microsoft.com/en-us/microsoft-teams/schema) for reference.

To use a bot in Teams, you need to deploy it to the cloud (or emulate it being in the cloud). You also need to register it in the Bot Framework, so that it is available in the cloud, and then add it to a team, so it is usable in the Teams channel by all team members. 

## Deploy your bot

To test your bot locally, use the [Bot Framework Emulator](https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator). If you are using ngrok, make note of the HTTPS link displayed. 

To deploy your bot in the cloud, follow these instructions [to deploy your bot in the cloud](https://docs.microsoft.com/en-us/bot-framework/deploy-bot-overview). If using Azure, you must have a Microsoft Azure subscription. If you don’t already have one, you can register for a free trial. Additionally, the process described by this article requires Git. 

### Register your bot
To register your bot, sign in to the Bot Framework Portal, click Register, and complete the form. Full instructions on the [BotFramework site](https://docs.microsoft.com/en-us/bot-framework/portal-register-bot). Populate the fields as follows: 
* Display name: sampleName 
* BotHandle: sampleName 
* Description: sampleName  app 
* Messaging Endpoint: https://(endpoint)/api/messages (if using ngrok, enter the ngrok endpoint, e.g., https://00bfb7bd.ngrok.io/api/messages)
* AppID (Click Create Microsoft App ID and Password. It will redirect to a new tab and display your new AppID. Copy AppID and click Generate Password. Copy and save these. Click Return to Bot Framework to continue. The new appID should now be auto-populated)
* Analytics if desired (optional)
* Checkbox to indicate that you have read and accepted the the Terms of Use, Privacy Statement, and Code of Conduct.
* Click Register to complete the registration process. 
* Next, the BotFramework registration page displays a list of channels. Besides the default channels, we want our new bot to run on the Teams channel, so click the Teams button and accept the defaults. BotFramework should add a row for the Teams channel. Click Save. 

Your bot is now available on your Teams channel! If you ever need to update your bot’s settings or channels, review My bots.

### Prep and sideload your app into your Teams channel
To connect your bot to Teams, you will need to: 
1. Update your manifest.  with the following: 
    * id : change the value GUID to any guid. For a Bot, you may use the Bot Framework Id. For a tab or other capability type, you can use [this online tool](https://guidgenerator.com/), or use one of your choosing, to generate a GUID. 
    * botid : insert your new botID from registering it on BotFramework. 
    * do a global search and replace YOURDOMAIN with either your domain or your ngrok link. 
2. Create your Teams app package. An app package is a zip file that contains your manifest.json file and your app’s icons. Create a zip with your new manifest.json file, and the two icon files in /apps, and call it TeamsApp.zip or whatever you like. 
3. Be sure your O365 Admin has enabled sideloading. See [Getting Started with Teams](setup.md) for details. 
4. Determine which Team will be using your bot. Inside Teams, navigate to the desired team and click the ellipsis (…), and then choose View team. Click the Apps tab, scroll to the bottom and choose Sideload an app. The popup will ask for your zip file. Choose it and click Open. See [Sideloading your app](sideload.md) for full details. You’ve successfully put your new Tasks app in your team! 
5. Inside your Team, navigate to the desired channel. You can put an app in General or any channel within your team, and the entire team will have access to it. In the desired channel, click the + in the tabs, and choose the Tasks App. To create your new tab, give it a name, and change the other defaults if desired. Click Save.  
6. If running a C# sample, in Visual Studio, edit web.config again. In the appSettings, populate these values so that they match your manifest.json file: BotId, MicrosoftAppId, MicrosoftAppPassword.If running a node.js sample, update the appID in bot.js, app,js, notifications.js, ChatConnector.js, and BotConnectBot.js. Deploy your code (or run the app if testing it locally). 

You’ve successfully deployed your app! Test your app in your Teams channel as described in the sample README.

>For more information on testing your bot, see [Testing your bot](botsadd.md).



>For information on loading your samples, see [Sideloading your app](sideload.md).

Further samples are coming soon.

