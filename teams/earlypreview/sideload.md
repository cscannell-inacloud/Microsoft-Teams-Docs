# Side loading your bot or tab in a team (new change rolling out)

We are rolling out a new process for side-loading a bot or tab.

To determine if you need to use this, navigate to your test team (you must be an owner of it).  Click on the overflow (“…”), and select View team.

!["View team"](../images/viewteam.jpg)

If you see a 'Developer (Preview)' tab, you should use the current process for side-loading a [bot](../botsadd.md) or a [tab](../createpackage.md)

If you see a 'Bots' tab at the top of the page, you should use the following process - regardless of whether you are building a bot or a tab.

1.  Create an updated manifest according to these [updated instructions](manifest.md).  **Note that you now need a manifest to side-load a bot as well as for a tab.**

2.	Click on "Sideload a bot or tab" on the lower right.

   !["Sideload entry point"](../images/sideloadentrypoint.png)

3.	Browse to and select your zip package from your PC.

4.	You will see your side loaded bot or tab in the list.

   !["Example of bot in list of side-loaded bots"](../images/botinlist.jpg)

If you're adding a tab, it should now appear in the tab gallery as normal.

If you're adding a bot:
 
1.	You will see a post in the General channel indicating the bot has been added to the team.

2.	You can start by invoking your bot by @mentioning the name of the bot, which should autocomplete.

3.  To test direct chats with your bot, you should first @mention it in a channel.  You can then start a 1:1 chat with it – e.g. via ‘New Chat’ or Search. 

## Troubleshooting

If the manifest doesn't load, please double-check you've followed all the instructions [here](manifest.md).

In particular, for tabs, note the size limitations on icons included in your side-load package.
