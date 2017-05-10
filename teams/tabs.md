# Getting started with tabs for Microsoft Teams

Tabs in Microsoft Teams allow you to display rich interactive web content.  You can build a Microsoft Teams tab from scratch or by adapting your existing web app experience.

Microsoft Teams supports tabs in two different contexts:
* Configurable Tabs, for use in channels, allow teams to configure the content of your tab experience when the tab is first added to a channel.
* Static Tabs, for use in individual contexts, allow users to interact with your experience privately, outside the context of a team.

## Overview of building a Microsoft Teams tab

Follow these steps to build a tab:

*  [Design a great tab:](design.md#building-a-great-tab) While it's easy to adapt a web app to become a Microsoft Teams tab, it's worth considering which of your experiences and functionality will work most effectively. 
*  [Create the package:](createpackage.md) This package contains the manifest, which specifies attributes of your tab, as well as other app components you may provide as part of your Teams app experience.

### For Configurable Tabs
*  [Create the configuration page:](createconfigpage.md) For configurable tabs, you must provide a configuration page to present options and gather information so users can customize the content and experience with your tab.  This iframed HTML page will be displayed when a user first pins the tab to a channel via the **Add a Tab** dialog.
	*  You can also [enable users to update a tab](updateremove.md#updating-an-existing-tab-instance) after they add it.
*  [Create the content page:](createcontentpage.md) Microsoft Teams displays this content page when the user visits your tab. This is also an HTML page which you host and Microsoft Teams displays within an iframe.
	* You can also provide a page for users to specify [what happens to content when they remove a tab](updateremove.md#removing-a-tab).
	* You can enable users to create and share [deep links to items within your tab](deeplinks.md) - such as links an individual task within a tab that contains a task list.

### For Static Tabs
*  [Create your static tab app identity](statictab.md): Static tabs host web content you already provide, and is great for a personal user-focused view of your app experience.  These tabs will show up in user's personal context.
*  [Create the content page:](createcontentpage.md) Microsoft Teams displays this content page when the user visits your tab. Content in a Static tab is subject to the same constraints as a Configurable tab.