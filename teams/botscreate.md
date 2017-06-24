# Creating and Testing a bot for Microsoft Teams

All bots created using the Microsoft Bot Framework are configured and ready to work in Microsoft Teams.

See the [Microsoft Bot Framework Overview](https://docs.botframework.com/en-us/) to learn how to:

1. [Register the bot with the Microsoft Bot Framework](https://dev.botframework.com/), making sure that you add Microsoft Teams as a channel and that you re-use any Microsoft App ID you generated if you've already created your app package/manifest.

![Bot Framework Registration page](images/Bot/BFRegister.PNG)

2. Build a bot using the [new Microsoft Teams .NET SDK](https://www.nuget.org/packages/Microsoft.Bot.Connector.Teams), [the new Microsoft Teams Node.js SDK](https://www.npmjs.com/package/botbuilder-teams) or [the Microsoft Bot Connector API](https://docs.botframework.com/en-us/restapi/connector/#navtitle).

3. Test it using the [Bot Framework Emulator](https://docs.botframework.com/en-us/tools/bot-framework-emulator/)
4. Deploy the bot to a cloud service, such as [Microsoft Azure](https://azure.microsoft.com/)

To make your bot experience Teams ready:

5. [Create a sideloadable bot package](createpackage.md) and [sideload it to a team](sideload.md) to test it in action.
6. [Add tabs](tabs.md) or [other extensibility options](index.md) to make your experience shine in Teams.
7. [Submit your final app package](submission.md) for publication in the Office Store.
