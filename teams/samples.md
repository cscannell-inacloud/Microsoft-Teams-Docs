# Sample applications for the Microsoft Teams Developer Platform

These samples that show you the various capabilities and different ways to implement Microsoft Teams apps:
* [Getting started sample](https://github.com/OfficeDev/microsoft-teams-sample-get-started).  This sample shows all the capabilities available in Microsoft Teams app, including bots, tabs, compose extensions, and connectors.  Source code provided in both C# and Node.js.
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
Microsoft hosts much of its sample code in GitHub, a web-based Git or version control repository.  If you’re not familiar with Git or GitHub, we recommend you review the [Git documentation](https://git-scm.com/doc) and follow the GitHub Hello World guide.

To download our samples from GitHub:
* Open the project in GitHub
* Select the "Clone or download" button and copy the URL
* Open a command prompt in the parent directory into which you want to install the sample project
* Run: `git clone <pasted url>`

This process will copy down the entire project.  Most Bot samples will provide code in both Node.js or .NET/C#.  You can find the appropriate project in the appropriate subdirectory.

### For Node.js samples
We provide a packages.json file that lists all required packages for a sample.  Simply run `npm install` from the command line in your Node project directory to install the required packages.  You're now ready to open up the project in Visual Studio Code and start experimenting.

### For .NET/C# samples
Our .NET samples provide a Visual Studio solution which contain all required libraries.

### For other samples
As always, the project's ReadMe file should have more information on specific needs for specific samples

## Tips for running Microsoft Teams samples

Our sample apps attempt to standardize much of the configuration and structure to simplify your experience.  While each project may have specific additional instructions within their readme, a few commonalities are discussed here.

### Project structure

For Microsoft Teams App projects, we provide a representative app package (.zip) along with the granular components (icons and manifest.json) in a subfolder named "TeamsAppPackage".  In general, these packages are for your convenience in building and sideloading the sample app, and will not run as is.  Please review the manifest file for guidance on what values should be replaced in order repackage and sideload your sample for running within Microsoft Teams.

For Node.js projects, we provide a Visual Studio Code "launch.json" configuration file, which contains profiles for running in the Bot Emulator (for projects with Bots) and for running within Teams.  These profiles set default environment variables for you to edit with your own values, such as:
* `MICROSOFT_APP_ID` - set to your registered Bot Framework bot id (or blank for the Emulator configuration)
* `MICROSOFT_APP_PASSWORD` - set to your registered Bot Framework bot id (or blank for the Emulator configuration)
* `TEAMS_APP_ID` - set to your registered Bot Framework bot id, or different as needed
* `BASE_URI` - base URL for your local hosted instance, for use by for example Tab sample projects.  This will most likely be set to your tunnel URL from Ngrok or other tunneling software.

For .NET/C# projects, we leverage the "web.config" file to host environment variables for our project, which you should modify to run as needed for your testing.  Similar to above, we use the following environment variables, used as above:
* `MicrosoftAppId`
* `MicrosoftAppPassword`
* `TeamsAppId`
* `BaseUri` 

### Running the samples

Microsoft Teams apps can contain one or more capabilities, and the ways to run or even host them may be different.  In general, for our samples, we have the following ways to run:
* Purely local – For bots, you can test your experience in the Bot Emulator.  For other content, you can run locally and address content through `http://localhost`
* Locally hosted, in Teams – This involves running locally with tunneling software, and creating a package to sideload into Teams.  This allows you to easily run and step-debug your app within the Teams experience.
* Cloud hosted, in Teams – This truly simulates (or is) product level support for a Teams app experience.  It involves uploading your solution to your cloud provider of choice (we recommend Azure), and creating a package to sideload into Teams.

Note that none of these testing solutions fully replicates the end user experience for a Store-distributed app, as the app installation process does some of the capability checks, like scope, at install time.

#### Running locally

For purely local or local Teams testing, you'll run the experience off of your own machine.  This allows you to actually compile and run within your IDE, and take full advantage of such techniques as break points and step debugging.

All sample projects will use default port settings: 3978 for Node.js, 3979 for .NET.  Similarly, all sample projects will take advantage of environment variables per above.

#### Purely local - running in the Bot Emulator

Our bot samples are designed to run out of the box within the Bot Emulator.  This allows you to test some of the core 1:1 logic of the bot, see rough layout of messages and perform simple tests.  Here are the steps:
* Run the code by selecting the pre-built "Launch - Emulator" debug configuration for Node.js, or use the default "web.config" values for .NET/C#.
* Launch the Bot Emulator and set the URL to:
    * `http://localhost:3978/api/messages` for Node.js, or :3979 for .NET/C#.
* Leave the Microsoft App ID and Microsoft App Password blank, to match the default environment variables.

#### Locally hosted

As Microsoft Teams is an entirely cloud-based product, it requires all services it accesses to be available from the cloud.  Therefore, to enable our samples to work within Teams, you need to either publish the code to the cloud of your choice, or make our local running instance externally accessible.  We can do this with tunneling software.

While you can use any tool of choice, we use and recommend Ngrok and will use this in our examples below.



>For information on loading your samples, see [Sideloading your app](sideload.md).

Further samples are coming soon.