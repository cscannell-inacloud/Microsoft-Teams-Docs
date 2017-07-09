# Running and Debugging Microsoft Teams apps

Microsoft Teams apps can contain one or more capabilities, and the ways to run or even host them may be different.  When it comes to debugging, in general, we have the following ways to run your Microsoft Teams app:
* Purely local – For bots, you can test your experience in the Bot Emulator.  For other content, you can run locally in your browser and address content through `http://localhost`
* Locally hosted, in Teams – This involves running locally with tunneling software, and [creating a package](createpackage.md) to [sideload](sideload.md) into Teams.  This allows you to easily run and step-debug your app within the Teams experience.
* Cloud hosted, in Teams – This truly simulates (or is) production level support for a Teams app experience.  It involves uploading your solution to your externally accessible server or cloud provider of choice (we recommend Azure), and [creating a package](createpackage.md) to [sideload](sideload.md) into Teams.

For purely local or local Teams testing, you'll run the experience off of your own machine.  This allows you to actually compile and run within your IDE, and take full advantage of such techniques as break points and step debugging.  For production-scale debugging and testing, we recommend you follow your own company guidelines to ensure you are able to support testing, staging, and deployment through your own processes.

In general, too, we recommend you utilize multiple manifests and packages to allow you to maintain separation between production and development services.  For example, you might choose to register separate development and production bots and create appropriate packages to sideload them in your testing environment.  We also recommend you sideload and test your production package before submitting to the Office Store or distributing to customers.

> Note: none of these testing solutions fully replicates the end user experience for a Store-distributed app, as the app installation process does some of the capability checks, like scope, at install time.

## Purely local - running in the Bot Emulator

Our bot samples are designed to run out of the box within the Bot Emulator.  This allows you to test some of the core 1:1 logic of the bot, see rough layout of messages and perform simple tests.  Here are the steps:
* Run the code by selecting the pre-built "Launch - Emulator" debug configuration for Node.js, or use the default "web.config" values for .NET/C#.
* Launch the Bot Emulator and set the URL to:
    * `http://localhost:3978/api/messages` for Node.js, or :3979 for .NET/C#.
* Leave the Microsoft App ID and Microsoft App Password blank, to match the default environment variables.

>Note: running this way does not give you access to Teams app functionality or Teams-specific bot functions like roster calls and other channel-specific functionality.  In addition, some capabilities may be allowed in by the Bot Framework in the Bot Emulator that may not function when running in Microsoft Teams. 

## Locally hosted

As Microsoft Teams is an entirely cloud-based product, it requires all services it accesses to be available from the cloud using HTTPS endpoints.  Therefore, to enable our samples to work within Teams, you need to either publish the code to the cloud of your choice, or make our local running instance externally accessible.  We can do the latter with tunneling software.

While you can use any tool of choice, we use and recommend Ngrok.  Ngrok  creates an externally addressable URL for a port you open up locally on your machine.  To set up Ngrok in preparation for running your Microsoft Teams app locally: 
* In a terminal, go the directory where you have ngrok.exe installed.
* Run, for example, `./ngrok http 3978 --host-header=localhost:3978`, or replace the port number as needed.
This will launch Ngrok to listen on the port you specify. In return, it will give you an externally addressable URL, valid for as long as Ngrok is running.

> Note: if you stop and restart Ngrok, you will get a new address.

To utilize Ngrok in your project, and depending on the capabilities you are using, you must replace all URL references in your code, configuration, and/or manifest.json file to use this URL endpoint.  

For example, for bots registered in the Microsoft Bot Framework, update the bot's endpoint to use this new Ngrok endpoint, e.g. https://2d1224fb.ngrok.io/api/messages.  You can validate that Ngrok is working, too, by testing bot response in the Bot Framework portal's Test chat window - again, like the emulator, this test will not allow you to access Teams-specific functionality. 

Please note that any time you change values in the manifest.json, you'll need to repackage and re-sideload into Teams.

## Cloud-hosted

You may use any externally addressable service to host your development and production code and their HTTPS endpoints.  There is no expectation that your capabilities reside on the same service - it's your project so it's up to you.  We do require that all domains being accessed from your Microsoft Teams apps do get added to the `validDomain` object in the manifest.json file.

>Note: To ensure a secure environment, we urge you to be explicit on the exact domain and subdomains you reference, and those domains must be in your control.  E.g. *.azurewebsites.net would not be allowed, but contoso.azurewebsites.net would.

## Loading and running

In general, to load and run your experience within Microsoft Teams, you'll need to create a package and sideload it into Teams via the following guidance:
* [Create the package for your Microsoft Teams app](createpackage.md)
* [Sideloading your app in a team](sideload.md)
