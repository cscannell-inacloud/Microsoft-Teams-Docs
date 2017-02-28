# Submit Team Experiences for the Tab and Bots Gallery

Microsoft Teams provides a Tab and Bot gallery to showcase experiences you deliver to end users. Here's how to submit your Teams tab or bot for consideration for inclusion in the appropriate gallery.

>**Note** At this time, Teams tab and bot submission is a closed program, by invitation only.  If you have a great idea for a Teams experience, submit your proposal [here](https://aka.ms/microsoftteamsdeveloperpreviewinterestform).

## Create a Microsoft Seller Dashboard account (optional)

As Microsoft Teams migrates to the Office Store for Teams tabs and bots distribution later this year, all submissions will go through the [Microsoft Seller Dashboard](http://go.microsoft.com/fwlink/?LinkId=248605).  If you have not already done so, you should create a developer account, which will allow you to submit apps and add-ins to Microsoft marketplaces, including Windows Store, Office Store, Azure Marketplace, and more to come.  This process ensures you establish your identity in the Microsoft Marketplace ecosystem, and triggers the appropriate validation checks by our Marketplace team to ensure you are who you say you are.

Identity in the Microsoft Store ecosystem relies on your [Microsoft Account](https://account.microsoft.com/account).  You'll need to create a new Microsoft account or use an existing one.  Note that this identity will be the main administrator of the Store experience.  For more information, please review the [Developer program FAQ](https://developer.microsoft.com/en-us/store/register/faq).

For general information on Office Add-ins on the Office Store, see: [Submit Office and SharePoint Add-ins and Office 365 web apps to the Office Store](https://msdn.microsoft.com/en-us/library/office/jj220037.aspx)

## Review our [Teams Tab Developer Agreement](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/developeragreement.pdf)

By submitting your tab or bot for inclusion in the Teams gallery, you accept the Agreement, and you agree to be bound by its terms.

## Send us your Teams submission 

### 1. Create and test your bot or tab

You should make sure you're in our developer ring (ask us if you're not)

General documentation for the platform is [here](index.md).  There are also additional features for bots explained in [these](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/earlypreview/Invoke.pdf) [documents](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/earlypreview/BotsInChannels.pdf).

To test your bot or tab, you need to [create a manifest](earlypreview/manifest.md) and [side-load it into Microsoft Teams](earlypreview/sideload.md).

### 2. Fill out the [Test Submission form](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/TeamsAppSubmission.docx)

This simple document is used to inform our validation team what your experience does and how best to test it.  We will look at the experience from the perspective of a new user - does it work, what value does it provide, is it clear how to create any necessary accounts? We would also like to experience your tab or bot as an experienced user. If applicable, please create a test user account, ideally with appropriate data to highlight your key scenarios.  Should the test account require two-factor authentication, please include instructions for how the tester may fully validate.  Please do not assume any prior familiarity with your experience.  

### 3. Provide supplemental marketing materials

Per the terms of our agreement, we may use your Publisher Marks and offer information for marketing and promotional purposes.  In order to best facilitate this, please provide us the following:
* Hi-res logo of your product
* Hi-res logo of your company logo
* Marketing / promotional copy of your experience (1-3 sentences, please use text box in the Test Submission form)

### 4. Build the Submission Package

Create a zip file containing: 
* manifest.json
* Your 44x44 and 88x88 icons, less than ~1.5KB - if you're building a tab, and not referencing your icons via a publicly accessible URL.
* The Test Submission form
* All supplemental marketing material.  

You must use the following naming convention: _date_-_pubname_-_appname_-_versionnum_.zip  

### 5. Send it to us

Email the Submission Package to: TeamsSubSupport@microsoft.com


## Teams tab and bot approval process

In order for your Teams tab or bot to be approved:
* It must not contain inadmissible or offensive material.
* It must be stable and functional.
* Any material that you associate with your experience, such as descriptions and support documentation, must be accurate. Use correct spelling, capitalization, punctuation, and grammar in your descriptions and materials.
* It must pass all [Validation policies for Teams tabs and bots](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/TeamsAppValidation.pdf), which is based off of the Office Store validation policies found [here](https://msdn.microsoft.com/en-us/library/office/jj220035.aspx).  Please note, like the Office Store validation policies, these are subject to change.

When the validation process is complete, you will receive a message to let you know that either your Teams experience is approved, or it fails one of the stated policies.  

For approved tabs and bots, no more action is needed on your side; your solution will be considered for Gallery placement.  Note that as your solution is a service that you host, you are free to continue to work and improve your product, to the extent that it remains in compliance with the Validation policies.  We reserve the right to spot-check and/or respond to customer complaints on issues that arise after approval and per our Developer Agreement, may de-list your product until any issues are resolved.

Failures will be explained and have appropriate policy violation references. All failures must be addressed before resubmission.  For resubmission, please follow the above process.  Do not change version number, but do change the date in the file name.

>**Note** If you make changes to an approved Teams experience, specifically as it relates to core functionality or the Manifest, it must go through the approval process again.  For all other changes to your service, for example addressing issues or adding new features, resubmission is not required.

## Tips for rapid approval

* Make sure you include detailed testing notes and a valid working test account with appropriate prepopulated data.
* If your product requires an account on your or another service, list that in the description.
* If your product requires additional purchases to properly function, list that in the description.
* For your Tab config, make sure you provide About links and proper guidance - this page will be the first thing the user sees, so make sure a new user understands what to do.
* Check your manifest for completeness and accuracy.  Then check it again.
* Follow the file naming conventions and package contents from above.
* Make sure your Bot provides appropriate responses when @mentioned in a channel as well as in 1:1 convesations.