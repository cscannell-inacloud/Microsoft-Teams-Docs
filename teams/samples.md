# Sample applications for the Microsoft Teams Developer Platform

There are Microsoft Teams samples in our GitHub repositories for you to clone or download:

* [Getting started sample](https://github.com/OfficeDev/microsoft-teams-sample-get-started).  This sample shows all the capabilities available in Microsoft Teams app, including bots, tabs, compose extensions, and notification feeds.  Source code provided in both C# and Node.js.
* [Simple List tab sample](https://github.com/OfficeDev/microsoft-teams-sample-todo).  This Node.js sample shows how easy it is to convert an existing web app into a tab.


## Working with Microsoft Teams samples

While each project may have specific instructions for how to test and deploy, in general the following guidelines should be applicable to most samples.  Please note that in order to fully test within Microsoft Teams, you'll need to follow the Packaging and Sideloading guidelines to make the app available.  All samples should have a TeamsAppPackages directory which contains a sample manifest.json file and icons, to facilitate your app creation.

### Samples with Bots

By default, all samples with bots should work locally, by simply running the [Bot Framework Emulator](https://docs.microsoft.com/en-us/bot-framework/debug-bots-emulator) using the default ports (3978 for Node.js samples, 3979 for .NET/C# samples) and blank values for the Microsoft App ID and Password.  Note that the Emulator will only allow you to test basic 1:1 messages.  

To run the bot inside Microsoft Teams, to flex their full capabilities, you must sideload your test package.  Please review [Creating and Testing a bot for Microsoft Teams](botscreate.md) for a general overview.  For our sample in particular, follow these steps:

1. [Register the bot in the Bot Framework](https://docs.microsoft.com/en-us/bot-framework/portal-register-bot) - Make sure you record your Bot ID and Password

2. Update the code to use your registered bot ID and password
    * If you're using Node.js, set the following environment variables:<br>
            * `MICROSOFT_APP_ID`<br>
            * `MICROSOFT_APP_PASSWORD`<br>
    * If you're using the Bot Builder SDK for .NET, set the following key values in the web.config file:<br>
            * `MicrosoftAppId`<br>
            * `MicrosoftAppPassword`<br>

3. Update the example manifest with your BotID and update the sideloadable package

    * Manifest.json and sample icons are located in the project's TeamsAppPackages folder.  Replace the `bots.id` value with your Bot ID from the Bot Framework.

4. [Sideload the package](sideload.md) into a team 
5. Run your bot locally with Ngrok or [deploy your bot to the cloud](https://docs.microsoft.com/en-us/bot-framework/deploy-bot-overview)

>For more information on testing your bot, see [Testing your bot](botsadd.md).

### Samples with Tabs

Samples which contain tabs will either use temporarily hosted sites, or illustrative public sites.  In all cases, should you wish to deploy and modify yourself, follow these steps:

1. Deploy the tab content to your desired host (localhost:PORT or your host domain)
2. It's a good practice to create a new AppID for your modified app as well.  Generate a unique Teams App Id / GUID with your app of choice (e.g. https://guidgenerator.com/)
3. Update the code to use your new AppID.
    * If you're using Node.js, replacer the environment variables: `TEAMS_APP_ID`.<br>
    * If you're using .NET, replace the key value `TEAMS_APP_ID` in the web.config file.<br>
4. Update the code to use your desired host (localhost:PORT or your host domain)
    * If you're using Node.js, look for environment variables with `TAB_URL` suffixes.<br>
    * If you're using the Bot Builder SDK for .NET, look for key values ending in `TAB_URL`.<br>
5. Update the example manifest 
    * `id` - your AppID
    * `configurableTabs.configurationUrl` - your config html, if applicable
    * `staticTabs.contentUrl` and `.websiteUrl` - your static html host(s), if applicable
    * `validDomains` array - the list of hosts you're using
6. Update the package with the modified manifest and [Sideload the package](sideload.md) into a team


