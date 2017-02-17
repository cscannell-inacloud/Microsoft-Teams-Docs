# Submit Team Apps for the Tab and Bots Gallery

Microsoft Teams provides a Tab and Bot gallery to showcase experiences you deliver to end users. Here's how to submit your Teams app for consideration for inclusion in the appropriate gallery.

>**Note** At this time, Teams Apps submission is a closed program, by invitation only.

## Create a Microsoft Seller Dashboard account (optional)

As Microsoft Teams migrates to the Office Store for Teams Apps distribution later this year, all submissions will go through the [Microsoft Seller Dashboard](http://go.microsoft.com/fwlink/?LinkId=248605).  If you have not already done so, you should create a developer account, which will allow you to submit apps and add-ins to Microsoft marketplaces, including Windows Store, Office Store, Azure Marketplace, and more to come.  This process ensures you establish your identity in the Microsoft Marketplace ecosystem, and triggers the appropriate validation checks by our Marketplace team to ensure you are who you say you are.

Identity in the Microsoft Store ecosystem relies on your [Microsoft Account](https://account.microsoft.com/account).  You'll need to create a new Microsoft account or use an existing one.  Note that this identity will be the main administrator of the Store experience.  For more information, please review the [Developer program FAQ](https://developer.microsoft.com/en-us/store/register/faq).

For general information on Office Add-ins on the Office Store, see: [Submit Office and SharePoint Add-ins and Office 365 web apps to the Office Store](https://msdn.microsoft.com/en-us/library/office/jj220037.aspx)

## Review and sign our [Teams App Developer Agreement](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/developeragreement.pdf)

This specific agreement defines and covers expectations for both parties for the curated Teams Tab and Bot Gallery.

## Send us your Teams app submission 

### 1. Create your manifest

The current Manifest schema is [here](schema.md) but amended per the below.  
* Your manifest file must be named `manifest.json`
* Your manifest must be at the top level of the Submission Package.

Your manifest for Preview must contain the following amended information:
| Field  | Required   | Description   | 
|-------------------------------|---|----------------------------------------------------------------------------|
|	id	|	*	|	Unique identifier for the experience.  Note: Must be a guid for Preview.  [GUID generator](https://guidgenerator.com/)	|	
|	tabs	|		|	This section is required if you are providing an interactive Teams tab as part of your solution.	|	
| tabs->description |  | Both the short and full descriptions must accurately and adequately explain your experience. |
|	bots	|		|	This section, previously marked _reserved for future use_ will be used for your optional bot submission.	|
|	bots->mri	|	*	|	This must be the Bot Framework ID for your registered bot solution.	|

Sample manifest - note this example shows a Tab and a Bot in a single package.  Include only the section relevant to your solution:

```json
{
  "$schema": "https://teamspacewusprodms.blob.core.windows.net/tabframework/0.4/tab-manifest-schema.json", 
  "manifestVersion": "0.4",
  "version": "1.0",
  "developer": {
    "name": "Yuri Dogandjiev",
    "websiteUrl": "https://todolistsample.azurewebsites.net",
    "privacyUrl": "https://todolistsample.azurewebsites.net/privacy",
    "termsOfUseUrl": "https://todolistsample.azurewebsites.net/termsofuse"
  },
  "tabs": [
    {
      "id": "56E1A16C-DDB4-46C0-B4B1-FC634ED86DDD",  
      "name": "ToDo Sample App",
      "description": {
        "short": "Create Simple Personal ToDo lists",
        "full": "Create Simple Personal ToDo lists in Microsoft Teams.  Sign in with your favorite service, and start creating your own personal ToDo lists."
      },
      "icons": {
        "44": "icon44x44.png", 
        "88": "icon88x88.png"
      },
      "accentColor": "#ff6a00",
      "configUrl": "https://todolistsample.azurewebsites.net/config?version=0.4",
      "canUpdateConfig": true
    }
  ],
  "bots": [
    {
      "mri": "9a346a73-fe93-484c-8236-6bc7203830e1", 
    }
  ]
}
```

### 2. Fill out the [Test Submission form](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/TeamsAppSubmission.docx)

This simple document is used to inform our validation team what your app does and how best to test it.  We will look at the experience from the perspective of a new user - does the app work, what value does it provide, is it clear how to create any necessary accounts? We would also like to experience your app as an experienced user. If applicable, please create a test user account, ideally with appropriate data to highlight your key scenarios.  Should the test account require two-factor authentication, please include instructions for how the tester may fully validate.  Please do not assume any prior familiarity with your experience.  

### 3. Provide supplemental marketing materials

Per the terms of our agreement, we may use your Publisher Marks and offer information for marketing and promotional purposes.  In order to best facilitate this, please provide us the following:
* Hi-res logo of your product
* Hi-res logo of your company logo
* Marketing / promotional copy of your app experience (1-3 sentences, please use text box in the Test Submission form)

### 4. Build the Submission Package

Create a zip file containing: 
* manifest.json
* Your 44x44 and 88x88 icons
* The Test Submission form
* All supplemental marketing material.  

You must use the following naming convention: _date_-_pubname_-_appname_-_versionnum_.zip  

### 5. Send it to us

Email the Submission Package to: TeamsSubSupport@microsoft.com


## Teams app approval process

In order for your Teams app to be approved:
* It must not contain inadmissible or offensive material.
* It must be stable and functional.
* Any material that you associate with your Teams app, such as descriptions and support documentation, must be accurate. Use correct spelling, capitalization, punctuation, and grammar in your descriptions and materials.
* It must pass all [Validation policies for Teams apps](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/TeamsAppValidation.pdf), which is based off of the Office Store validation policies found [here](https://msdn.microsoft.com/en-us/library/office/jj220035.aspx).  Please note, like the Office Store validation policies, these are subject to change.

When the validation process is complete, you will receive a message to let you know that either your Teams app is approved, or it fails one of the stated policies.  

For approved apps, no more action is needed on your side; your solution will be considered for Gallery placement.  Note that as your solution is a service that you host, you are free to continue to work and improve your product, to the extent that it remains in compliance with the Validation policies.  We reserve the right to spot-check and/or respond to customer complaints on issues that arise after approval and per our Developer Agreement, may de-list your product until any issues are resolved.

Failures will be explained and have appropriate policy violation references. All failures must be addressed before resubmission.  For resubmission, please follow the above process.  Do not change version number, but do change the date in the file name.

>**Note** If you make changes to an approved Teams app, specifically as it relates to core functionality or the Manifest, it must go through the approval process again.  For all other changes to your service, for example addressing issues or adding new features, resubmission is not required.