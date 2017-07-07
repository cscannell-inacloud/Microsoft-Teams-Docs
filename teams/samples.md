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
* Select the `Clone or download` button and copy the URL
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

Microsoft Teams apps can contain one or more capabilities, and the ways to run or even host them may be different.  In general, for our samples, we have the following ways to run:
* Purely local – For bots, you can test your experience in the Bot Emulator.  For other content, you can run locally and address content through `http://localhost`
* Locally hosted, in Teams – This involves running locally with tunneling software, and creating a package to sideload into Teams.  This allows you to easily run and step-debug your app within the Teams experience.
* Cloud hosted, in Teams – This truly simulates (or is) product level support for a Teams app experience.  It involves uploading your solution to your cloud provider of choice (we recommend Azure), and creating a package to sideload into Teams.


In order to simply running our samples, 

>For information on loading your samples, see [Sideloading your app](sideload.md).

Further samples are coming soon.