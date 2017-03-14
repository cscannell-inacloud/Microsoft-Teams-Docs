# Sideloading your bot or tab in a team (New)

In order to test your bot or tab within teams, you'll need to sideload your app using the instructions below.  This adds the bot or tab to the team you've selected, and you and other team users can interact with it like an end user.

> **Note** For bots designed only for 1:1 contexts, see [here](botsadd.md) for an alternate way to add for testing.

## Create your sideload package

For development, you must create a sideloadable package which contains the information to describe your experience.  The package, a zip file, contains the application manifest and optional icons (for tabs) that uniquely describes your experience.  This process should only be used for testing / development as there are limitions to sideloaded packages during preview.

To create your sideload package, please review the full documentation [here](createpackage.md).

## Load your package into a team

With your package created, you can now load it into a team of your choosing.  This adds the experience (tab and/or bot) as an available integration for all users in the selected team.

> **Note** In order for sideloading to work, your tenant admin must first permit sideloading ([more info](setup.md))

1.  Create a new team for testing, if necessary.  Click **Create team** at the bottom of the left-hand panel.

2.  On the team you wish to load it, click on the overflow (“…”), and select View team. 

   !["View team"](images/tab_view_team.png)

> **Note** You must be the team owner, or the owner must allow users to add tabs for this functionality to appear.

3.	Select the Bot tab, then click on "Sideload a bot or tab" on the lower right.

   !["Sideload entry point"](images/sideloadentrypoint.png)

4.	Browse to and select your zip package from your PC.

5.	You will see your sideloaded bot or tab in the list.

   !["Example of bot in list of side-loaded bots"](images/botinlist.jpg)



## Accessing your sideloaded tab

With the tab loaded in the team, users may pin it to any channel on the team using the standard tab gallery flow:

1. Go to a channel in the team.  Click **+** to the right of the existing tabs.

2. Select your tab from the gallery that appears.

3. Accept the consent prompt.

4. Configure your tab via its [configuration page](createconfigpage.md) and click **Save**. 

![The Add a tab dialog box, featuring a gallery of available tabs.](images/tab_gallery.png)


## Accessing your sideloaded bot
 
When you add a bot to the team, it should be usable by anyone on that team, inside and outside the team channels.  You and other team members will see a post in the General channel indicating that the bot has been added to the team.

You can start by invoking your bot by @mentioning the name of the bot, which should autocomplete.

To test direct chats with your bot, you should first @mention it in a channel.  You can then start a 1:1 chat with it – e.g. via ‘New Chat’ or Search. 


## Removing or updating your app

Should you wish to remove your tab or bot, select the trash can icon next to the app name in the View Teams bots list.  

If you change manifest information, you must first remove the solution and readd the new package per above.  Note that in general, code changes on your service do not require you to re-sideload your manifest, unless those changes require manifest updates (e.g. URL or bot id changes). 

## Troubleshooting Notes

> * If you've sideloaded tabs via the old preview method, they may have disappeared recently.  If so, you'll need to sideload them again using the above steps.

> * If the manifest doesn't load, please double-check you've followed all the instructions [here](createpackage.md).  In particular, for tabs, note the size limitations on icons included in your sideload package (each under 1.5k)

> * Encountering other problems?  See the [troubleshooting guide](troubleshooting.md).