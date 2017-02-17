# Submit Team Apps for the Tab and Bots Gallery (Preview)

Microsoft Teams provides a Tab and Bot gallery to showcase experiences you deliver to end users. Here's how to submit your Teams app for consideration for inclusion in the appropriate gallery.

>**Note** At this time, Teams Apps submission is a closed program, by invitation only.

## Create a Microsoft Seller Dashboard account (optional)

As Microsoft Teams migrates to the Office Store for Teams Apps distribution later this year, all submissions will go through the [Microsoft Seller Dashboard](http://go.microsoft.com/fwlink/?LinkId=248605).  If you have not already done so, you should create a developer account, which will allow you to submit apps and add-ins to Microsoft marketplaces, including Windows Store, Office Store, Azure Marketplace, and more to come.  This process ensures you establish your identity in the Microsoft Marketplace ecosystem, and triggers the appropriate validation checks by our Marketplace team to ensure you are who you say you are.

Identity in the Microsoft Store ecosystem relies on your [Microsoft Account](https://account.microsoft.com/account).  You'll need to create a new Microsoft account or use an existing one.  Note that this identity will be the main administrator of the Store experience.  For more information, please review the [Developer program FAQ](https://developer.microsoft.com/en-us/store/register/faq).

For details, see: [Submit Office and SharePoint Add-ins and Office 365 web apps to the Office Store](https://msdn.microsoft.com/en-us/library/office/jj220037.aspx)

## Teams app submission package

### 1. Create your manifest

The current Manifest schema is [here](schema.md) but amended per the below.  
* Your manifest file must be named `manifest.json`
* Your manifest must be at the top level of the Submission Package.

Your manifest for Preview must contain the following amended information:
| Field  | Required   | Description   | 
|-------------------------------|---|----------------------------------------------------------------------------|
|	id	|	*	|	Unique identifier for the experience.  Note: Must be a guid for Preview.  [GUID generator](https://guidgenerator.com/)	|	
|	tabs	|		|	This section is required if you are providing an interactive Teams tab as part of your solution.	|	
|	bots	|		|	This section, previously marked _reserved for future use_ will be used for your optional bot submission.	|
|	bots->mri	|	*	|	This must be the Bot Framework ID for your registered bot solution.	|
|	bots->pinnedTabs	|	  | (Coming soon) Your bot may optionally provide a static tab, shown in direct chat.	|
|	bots->pinnedTabs->id	|	*	|	User defined ID for the tab	|
|	bots->pinnedTabs->definitionId	|	*	|	Like an entity ID for a Teams tab, this can be used by you to identify the specific content on display.	|
|	bots->pinnedTabs->displayName	|	*	|	Name to show on the Tab UX	|
|	bots->pinnedTabs->url	|	*	|	The url for the content to show in the tab	|
|	bots->pinnedTabs->websiteUrl | * 	|	A fallback URL for the user to view in browser 	|


Sample manifest:

```json
{
  "$schema": "https://teamspacewusprodms.blob.core.windows.net/tabframework/0.4/tab-manifest-schema.json", 
  "manifestVersion": "0.4",
  "version": "0.4",
  "developer": {
    "name": "Yuri Dogandjiev",
    "websiteUrl": "https://teams-todolist.azurewebsites.net",
    "privacyUrl": "https://teams-todolist.azurewebsites.net/privacy",
    "termsOfUseUrl": "https://teams-todolist.azurewebsites.net/termsofuse"
  },
  *Note: the following section is for solutions that have a tab scenario:*
  "tabs": [
    {
      "id": "56E1A16C-DDB4-46C0-B4B1-FC634ED86DDD",  *Note: this must be a GUID, not reverse domain name*
      "name": "ToDoList (0.4)",
      "description": {
        "short": "Create todo lists.",
        "full": "Create todo lists. Sign in with your favorite service, start creating todo lists."
      },
      "icons": {
        "44": "https://bot-framework.azureedge.net/bot-icons-v1/bot-framework-default-3.png",  *Note: may be URL to icon, or filename of icon embedded in Zip*
        "88": "https://bot-framework.azureedge.net/bot-icons-v1/bot-framework-default-3.png"
      },
      "accentColor": "#ff6a00",
      "configUrl": "https://teams-todolist.azurewebsites.net/config?version=0.4",
      "canUpdateConfig": true
    }
  ],

  *Note: the following section is for solutions that have a bot*
  "bots": [
    {
      "mri": "9a346a73-fe93-484c-8236-6bc7203830e1",  *Note: this is the Bot Framework id*

      *Note: the following section is used if you wish to include a static tab as part of your bot.  Static tabs will not allow any customization like Team tabs, but can be used to surface information about your experience, like FAQ or other content that may be useful for your users.*
      "pinnedTabs": [
        {
          "id": "personal_todolist",  *Note: this is user defined*
          "definitionId": "com.skype.teams.samples.todolist.0.4",
          "displayName": "Personal todo list",
          "url": "https://teams-todolist.azurewebsites.net?version=0.4",  *Note: this is the content to surface in the bot's tab*
          "websiteUrl": "https://teams-todolist.azurewebsites.net" 
        }
      ]
    }
  ]
}
```




### 2. Fill out the [Test Submission form]("TeamsAppSubmission.docx")

This simple document is used to inform our validation team what your app does and how best to test it.  We will look at the experience from the perspective of a new user - does the app work, what value does it provide, is it clear how to create any necessary accounts? We would also like to experience your app as an experienced user. If applicable, please create a test user account, ideally with appropriate data to highlight your key scenarios, and submit the account information and simple test plan in this form.  

### 3. Provide supplemental marketing materials

Per the terms of our agreement, we may user your Publisher Marks and offering information for marketing and promotional purposes.  In order to best facilitate this, please provide us the following:
* Hi-res logo of your product
* Hi-res logo of your company logo
* Marketing / promotional copy of your app experience (1-3 sentences, please use text box in the Test Submission form)

### 4. Build the Submission Package

Create a zip file containing: the Manifest json file, the Test Submission form, and all supplemental marketing material.  You must use the following naming convention: _date_-_pubname_-_appname_-_versionnum_.zip  

### 5. Send it to us

Email the Submission Package to: TeamsSubSupport@microsoft.com


## Teams app approval process

In order for your Teams app to be approved:
* It must not contain inadmissible or offensive material.
* It must be stable and functional.
* Any material that you associate with your Teams app, such as descriptions and support documentation, must be accurate. Use correct spelling, capitalization, punctuation, and grammar in your descriptions and materials.
* It must pass all [Validation policies for Teams apps](TeamsAppValidation.pdf), which is based off of the Office Store validation policies found [here](https://msdn.microsoft.com/en-us/library/office/jj220035.aspx).  Please note, like the Office Store validation policies, these are subject to change.

When the validation process is complete, you will receive a message to let you know that either your Teams app is approved, or you need to make changes and resubmit it.  For resubmission, please follow the above process.  Do not change version number, but do change the date in the file name.

>**Note** If you make changes to an approved Teams app, specifically as it relates to the Manifest, it must go through the approval process again.