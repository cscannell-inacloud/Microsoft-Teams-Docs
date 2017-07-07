# Running and Debugging Microsoft Teams apps

Microsoft Teams apps can contain one or more capabilities, and the ways to run or even host them may be different.  When it comes to debugging, in general, we have the following ways to run your Microsoft Teams app:
* Purely local – For bots, you can test your experience in the Bot Emulator.  For other content, you can run locally and address content through `http://localhost`
* Locally hosted, in Teams – This involves running locally with tunneling software, and creating a package to sideload into Teams.  This allows you to easily run and step-debug your app within the Teams experience.
* Cloud hosted, in Teams – This truly simulates (or is) product level support for a Teams app experience.  It involves uploading your solution to your cloud provider of choice (we recommend Azure), and [creating a package](createpackage.md) to [sideload](sideload.md) into Teams.

Note that none of these testing solutions fully replicates the end user experience for a Store-distributed app, as the app installation process does some of the capability checks, like scope, at install time.

## Running locally

For purely local or local Teams testing, you'll run the experience off of your own machine.  This allows you to actually compile and run within your IDE, and take full advantage of such techniques as break points and step debugging.

### Purely local - running in the Bot Emulator

Our bot samples are designed to run out of the box within the Bot Emulator.  This allows you to test some of the core 1:1 logic of the bot, see rough layout of messages and perform simple tests.  Here are the steps:
* Run the code by selecting the pre-built "Launch - Emulator" debug configuration for Node.js, or use the default "web.config" values for .NET/C#.
* Launch the Bot Emulator and set the URL to:
    * `http://localhost:3978/api/messages` for Node.js, or :3979 for .NET/C#.
* Leave the Microsoft App ID and Microsoft App Password blank, to match the default environment variables.

### Locally hosted

As Microsoft Teams is an entirely cloud-based product, it requires all services it accesses to be available from the cloud.  Therefore, to enable our samples to work within Teams, you need to either publish the code to the cloud of your choice, or make our local running instance externally accessible.  We can do this with tunneling software.

While you can use any tool of choice, we use and recommend Ngrok and will use this in our examples below.

Setting up Ngrok
Ngrok basically creates an externally addressable URL for a port you open up locally on your machine.  As mentioned, we typically use port 3978 for Node.js and 3979 for .NET.  Thus for our sample app, you’ll want to perform the following steps: 
o	In a terminal, go the directory where you have ngrok.exe installed.
o	Run "./ngrok http 3978 --host-header=localhost:3978"
o	This will launch Ngrok to listen on the port you specify. In return, it will give you an externally addressable URL, valid for as long as Ngrok is running.
o	Note: if you stop and restart Ngrok, you will get a new address.
o	Copy the secure address (https) to your clipboard.
 
 
This is the process you’ll go through if you want to test our samples on your machine, within Teams.  By running the code locally, you get to take advantage of such things as step debugging and rapid code iteration.  To see this in action, though, you need to have something to run against, so let’s register a bot.

Registering a Bot in the Microsoft Bot Framework
Bots in Teams must be built upon the Microsoft Bot Framework.  For our samples, as part of the package download process, you’ll get the Bot Framework and in most cases, the Bot Framework Teams extension packages.
In addition, every bot must be registered in the Bot Framework, so it is accessible by the services it uses like Microsoft Teams.  Our samples are designed for you to run yourself, so you’ll need to create your own Bot identity, which consists of a Microsoft App ID and password.  Here’s how: 
o	Using your work or school account, sign in to the Microsoft Bot Framework site https://dev.botframework.com/ and click on the "My bots" tab to register a new bot.
 
 
 
o	Display name - Give your app a name.  This does not have to be unique.  This will be the name displayed in Teams. (In the screenshots below, the name "Dorothy Bot" was used.) 
o	Note: If you decide to change the Display Name or icon after your bot is registered, it may take 2-4 weeks before your new name or icon will show up in Teams. 
o	Bot handle – Create a unique identifier for your bot.
o	Note: This is unchangeable and may be visible to the public. If you change the Display name of your bot, your Bot handle will still remain the same.  
o	Long description - Enter a long description which may appear in channels or directories.
o	Note: In Microsoft Teams, the Store information will come from the Seller Dashboard.

Next you need to configure your Bot service endpoint where services like Microsoft Teams will find your running bot instance:
o	Messaging endpoint - Paste the Ngrok url that you copied to your clipboard and append to it the appropriate endpoint.  For our samples, again, that’s "/api/messages": e.g. https://2d1224fb.ngrok.io/api/messages
o	Create Microsoft App ID and password – This button will take you to the Application Registration Portal, where you can create a unique Microsoft App ID and password.  You’ll need to log in again with your same identification.
o	App name – This should prepopulate with the Display name from the registration
o	App ID – This is a unique GUID created for your app, e.g. 93fed3d5-6782-462e-8a58-6a3e83ca6eab
o	Generate an app password to continue – Click this button to generate an app secret aka password.  E.g. qgSctpqT89ZdfAymt66Ukgf
o	Note:  You’ll need to copy and save this in a secure location as you will need this, and the app ID later.

When done with this stage, you’ll return to the Registration page, with the app ID filled in, that matches the one created above.  Check the box at the bottom, and click “Register” to create your new accessible Bot Framework bot.

Enable Microsoft Teams as a channel
By default, new bots are registered to be accessible by Skype and Web Chat.  After registering, you’ll be taken to your bot Channels page.  From here, you can select “Microsoft Teams” from the Add a channel list.  Simply slide the “Disabled” to “Enabled” and hit Done.  This enables your bot to work in Microsoft Teams, once we do all of the following of course.  
Configure the Sample code to use your new information
Now that you’ve registered your own bot, you’ll need to make the sample app assume that bot’s identity.  This means you’ll need the sample code to use your Microsoft App ID and password when it’s running, so that Teams can validate that it’s talking to the right bot.
By default, our Teams app samples expose environment variables you can configure, in one place, to set important information like this.  For our projects with a .vscode/launch.json file, you can modify the environment variables stored there.  Typical environment variables are:
•	MICROSOFT_APP_ID – This is the bot’s app ID you registered above
•	MICROSOFT_APP_PASSWORD – The secret from your registration
•	BASE_URI – Since our sample logic includes tabs and other non-bot features, we’ve made routing simple by allowing you to set the BASE_URI of your instance.  In this case, you should use the full endpoint from Ngrok, e.g. https://2d1224fb.ngrok.io
For this sample, open up the launch.json file and select the “Launch – Teams Debug” block.  Replace the above environment variables with your values from your bot registration.  Now when you run that profile, your code will validate that it is the registered bot. 
Note: if you choose to run the app directly, via “node app.js” from the command line, you’ll need to set the environment variables in your app.js code directly.  For ease of use, though, we recommend you use the launch configuration file provide.

Updating the Manifest
The manifest provides Teams all the information it needs to work with your app.  Find the manifest by going to your subdirectory \microsoft-teams-sample-get-started\Node\TeamsAppsPackages and opening manifest.json.
 
App and Bot IDs
You can keep most of the information in this file, the metadata that describes the app, as is, with a few important exceptions:
•	For the “id”, replace with app id that you got when registering the bot.
 

•	For the “bots:botId”, use this app id again
 

•	For this sample, we have a compose extension, which at its core is just a bot, and we’ll use the same id here as well.
 
 
Tabs 
Since we’re already here, let’s update the tab URLs to point to our running Ngrok instance.  Note that in non-sample real life, you may very well have separate endpoints and services handling your tabs and bots, which is totally fine.  For our sample purposes, though, we’re erring on simplicity and providing all app support in one running service, served up to Teams via our Ngrok instance.

Update the contentUrl and websiteUrl for both the static tab and the configurable tab to use your Ngrok instance.  Make sure you keep the pathing, e.g. /tabs/index/
 
 
 
 
Valid Domains
Add your Ngrok URL to the list of valid domains.  This is a safety check to ensure a Teams tab doesn’t allow venturing into unknown territory.
  
 
Save your new version of the manifest.json, making sure to keep the name as is.   We’re on to the next step:

Create your package
You can simply replace the existing manifest in the zip file we have provided, or you can create your own new zip file package.  For the latter, make sure you add the icons, and make sure all content is at the top level of the zip file.  

That’s it.  You have a suitable sideloadable package.  Pick your favorite test team and sideload it.

Sideload your app
Since channels depend on the manifest to communicate with your app, you must first sideload your package containing the manifest into Microsoft Teams.  To access sideload functionality, you’ll need to go to your selected team’s dashboard:  click the “…” next to your team name, and select “View team” from the dropdown.  On the team dashboard, click on "Apps", then in the lower right corner, click the "Sideload an app".  It will prompt you to navigate to your zip file. 

Sideloading your app means that all the capabilities defined therein should be available both in the team you loaded into, and in the personal scope.  Let’s verify.
Check it – Sample Tabs
Microsoft Teams has two different types of tabs you can access – teams tabs (currently Configurable only) and personal tabs (currently Static only).  Our sample app has both, so here’s how to check each type.
Test your configurable tab in a channel
In the team in which you sideloaded the package, select a channel and click the "+" icon next to the tabs and select the app called "Tasks App".  (You will change the "Tasks App" name in a later step).
 
 
 
After clicking on the Tasks App icon, it will ask you to name your tab. Then it will create a new tab for you. Once again, it is pre-populated with some "dummy" data or tasks to flesh out the example.
 
 
 
 
Test your static tab
Click the “Apps” icon in the sidebar to enter personal scope.  Your sideloaded static tab should appear as an App in the list, under the Sideloaded section.

Sideloaded apps should also be visible in the 1:1 chat canvas. The static tabs are labeled "My Tasks" and "Info". Click on "My Tasks".  For the sake of providing an example, the sample app has pre-populated the static tab with sample tasks assigned to "Me".
 
 
 
 
 
Check it – Compose Extension
As part of our sample app, we have provided a sample compose extension flow.  This compose extension is powered by the same bot service, and uses the same ID.  In fact, per above you used the same Microsoft App ID for both the botID and the compose extension ID. 

The sample compose extension simulates a user “searching” through the tasks in our SaaS in order to insert a rich card into the chat stream.  Like all things in our sample, all data is randomized and non-persistent – it is designed to show the overall flow (and the code behind it) for illustration purposes only.  Let’s try it.

•	Click on the Conversation tab, in either 1:1 bot conversation or in the channel into which the app was sideloaded. 
•	Click on the ellipses under the message box.  
•	Click on your App.
 
 
 
 
It will give you a Search box. 
 
 
 
Enter fake data, e.g. “coffee,’ and select one of the fake tasks that are returned.  This will insert it into the chat stream, and provide you an opportunity to provide additional information, like @mentioning a colleague so they can see the fake task too.

Please note: Searching for a given task does not actually work. 
One more example – updating the manifest
Any future changes to the manifest will require you to re-sideload the sample app. (It’s not necessary to delete the app as long as the App ID hasn’t changed).

For example, as a final test, in the manifest.json file, change the app name. In the example below, we changed it to "Dorothy App":
 
 
 
Re-package and sideload the app again in Teams, and note the name change:

 
 
Changing Ngrok
While you can leave your Ngrok running for as long as you want, when you do choose to stop and restart it, it will generate a new URL for you.  This will require you to change the following to get you back up and running:
•	Update the Bot Framework Endpoint
•	Update the Manifest to use the new address
•	Update the BASE_URI environment variable
Conclusion  
Now that you are an expert in running our sample app, we encourage you to play around, look at the code, make changes, step through, and in general learn through doing.  

If you want more on how to make a great Microsoft Teams bot, we encourage you to also check out our Microsoft Teams Template Bot on Github, written in Node.js.

As always, we welcome your feedback.  We look forward to seeing what great experiences you can bring to Microsoft Teams.
