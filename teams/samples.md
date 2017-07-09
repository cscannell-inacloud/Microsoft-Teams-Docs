# Sample applications for the Microsoft Teams Developer Platform

These samples that show you the various capabilities and different ways to implement Microsoft Teams apps:
* [Getting started sample](https://github.com/OfficeDev/microsoft-teams-sample-get-started).  This sample shows all the capabilities available in Microsoft Teams app, including bots, tabs, compose extensions, and connectors.  Source code provided in both C# and Node.js.
* [Microsoft Teams Graph API sample](https://github.com/OfficeDev/microsoft-teams-sample-graph).  This sample shows an example of using the new Microsoft Graph API calls to perform such tasks as querying teams and channels from a webservice running outside of Microsoft Teams.
* [Simple List tab sample](https://github.com/OfficeDev/microsoft-teams-sample-todo).  This Node.js sample shows how easy it is to convert an existing web app into a tab.

## Common prerequisites
Depending on your development platform of choice, we recommend the following common prequisites for running our sample experiences:
* [An Office 365 account with access to Microsoft Teams, with sideloading enabled](setup.md).
* For Node.js:
    * [Visual Studio Code](https://code.visualstudio.com/)
    * [Node.js](https://nodejs.org/en/download/)
* For .NET/C#:
    * Visual Studio. You can download the [Community](http://www.visualstudio.com/) version for free.
* For samples with bots:
    * [Bot Framework Emulator](https://docs.botframework.com/en-us/downloads/#navtitle)
* [Git command line tool](https://git-scm.com/) or [Git for Windows](https://git-for-windows.github.io/)
* Tunneling software like [Ngrok](https://ngrok.com/download).


## Getting samples
Microsoft hosts much of its sample code in GitHub, a web-based Git or version control repository.  If you’re not familiar with Git or GitHub, we recommend you review the [Git documentation](https://git-scm.com/doc) and follow the [GitHub Hello World guide](https://guides.github.com/activities/hello-world/).

To download our samples from GitHub:
* Open the project in GitHub
* Select the "Clone or download" button and copy the URL
* Open a command prompt in the parent directory into which you want to install the sample project
* Run: `git clone <pasted url>`

This process will copy down the entire project.  Most samples containing bots will provide code in both Node.js or .NET/C#; others will depending on the example or capability being shown.  You can find the appropriate project in the appropriate subdirectory.

### For Node.js samples
We provide a packages.json file that lists all required packages for a sample.  Simply run `npm install` from the command line in your Node project directory to install the required packages.  You're now ready to open up the project in Visual Studio Code and start experimenting.

### For .NET/C# samples
Our .NET samples provide a Visual Studio solution which contain all required libraries.

### For other samples
As always, the project's ReadMe file should have more information on specific needs for specific samples

## Tips for running Microsoft Teams samples

Our sample apps attempt to standardize much of the configuration and structure to simplify your experience.  While each project may have specific additional instructions within their ReadMe, a few commonalities are discussed here.

### Project structure

For Microsoft Teams App projects, we provide a representative app package (.zip) along with the granular components (icons and manifest.json) in a subfolder named "TeamsAppPackage".  In general, these packages are for your convenience in building and sideloading the sample app, and will not run as is.  Please review the manifest file for guidance on what values should be replaced in order repackage and sideload your sample for running within Microsoft Teams.

For Node.js projects, we provide a Visual Studio Code "launch.json" configuration file, which contains profiles for running in the Bot Emulator (for projects with Bots) and for running within Teams.  These profiles set default environment variables for you to edit with your own values, such as:
* `MICROSOFT_APP_ID` - set to your registered Bot Framework bot id (or blank for the Emulator configuration)
* `MICROSOFT_APP_PASSWORD` - set to your registered Bot Framework bot id (or blank for the Emulator configuration)
* `TEAMS_APP_ID` - set to your registered Bot Framework bot id, or different as needed
* `BASE_URI` - base URL for your local hosted instance, for use by for example Tab sample projects.  This will most likely be set to your tunnel URL from Ngrok or other tunneling software for local debugging, or your production endpoint if cloud-hosted.

For .NET/C# projects, we leverage the "web.config" file to host environment variables for our project, which you should modify to run as needed for your testing.  Similar to above, we use the following environment variables, used as above:
* `MicrosoftAppId`
* `MicrosoftAppPassword`
* `TeamsAppId`
* `BaseUri` 

### Running and debugging
For more information on running and debugging our samples, see our general [Running and Debugging Microsoft Teams apps](debugging.md) guidance.