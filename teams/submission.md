# Submit your solutions to the Office Store

>Note: While we have enabled the ability for Microsoft Teams apps to be published in the Office Store, the in-app discovery experience will be available at a future date.

Microsoft Teams Apps can now be published in the Office Store.  Teams will soon provide an in-app gallery for users to find or discover great Teams extensibility options powered by the Office store.  To have you solution available in this gallery, you must publish your solution through the Office Store process.  

The Office Store provides a convenient location for you to upload your Microsoft Teams app, as well as other  Office 365 extensibility types such as Office Add-ins and Sharepoint Add-ins.  To include you solution in the Office Store, you submit it to the Seller Dashboard.  You will need to create an individual or company account if you have not already done so for other Office extensibility types.

## Register as an app developer

If you have not already done so, you should create a developer account in [Microsoft Seller Dashboard](http://go.microsoft.com/fwlink/?LinkId=248605).  This account create process ensures you establish your identity in the Microsoft Marketplace ecosystem, and triggers the appropriate checks by our Marketplace team to validate your identity.

Account management in the Microsoft Store ecosystem relies on your [Microsoft Account](https://account.microsoft.com/account).  You'll need to create a new Microsoft account or use an existing one.  Note that this identity will be the main administrator of the Store experience.  For more information, please review the [Developer program FAQ](https://developer.microsoft.com/en-us/store/register/faq).

>Tip: Use an account specifically for your Store experience.  This is the main adminstrator account, and will be used for portal log in and report notification.  While you can add additional users as administrators, ensure you have a single account that can exist outside of an individual's account.  This also means you should not use any O365 Test tenant identities as these are time limited.

## Register in the Seller Dashboard to submit to the Office Store

There is a second approval process that happens after your Dev Center account has been created:  you need to create your identity in the Office Seller Dashboard.  While the content here should be similar to your Dev Center details, this extra step ensures the Office Store has all the required information and your identity in that storefront accurately reflects your business.

To click off the process, click on the "Continue" button under Office.

![Office Seller Dashboard entry point](images/submission/SellerDashboardOfficeEntry.PNG)

## Use the Seller Dashboard to submit to the Office Store
After your account is approved, you can submit your solution to the Seller Dashboard.  Add a new Application of type Team App to initiate the submission process.

![Office Seller Dashboard add an app](images/submission/SellerDashboardAddApp.PNG)

You will need to upload a Submission Package and provide the required metadata for the product listing page, including information such as app logo, description, screenshots and other information.  Please review our [Package and Submission checklist](submissionchecklist.md) for more information.

Note that all information in the package manifest must match the metadata information you enter into the product listing.

For more Teams-specific help, see [here](submissionguidance.md).

## Teams app approval process

Teams app approval is a free service provided by the Office Store that validates that your app works as described, contains all appropriate metadata, and provides content that would be valuable to an end user.

In order for your Teams app to be approved:
* It must not contain inadmissible or offensive material.
* It must be stable and functional.
* Any material that you associate with your experience, such as descriptions and support documentation, must be accurate. Use correct spelling, capitalization, punctuation, and grammar in your descriptions and materials.
* It must pass all [Office Store validation policies for Teams tabs and bots](https://msdn.microsoft.com/en-us/library/office/jj220035.aspx).  Please note that these policies are subject to change.
* For tabs, it must provide value in to users outside of what is possible by simply pinning your website into Teams.  This means at minimum, it must remove extraneous chrome and disallow navigating outside the configured context. See [here](https://aka.ms/microsoftteamsdesignguidelines) for more guidance.

When the validation process is complete, you will receive a message to let you know that either your Teams experience is approved, or it fails one of the stated policies.  You can also follow these steps to check the approval status in the Seller Dashboard:
1. Sign in to the Seller Dashboard.
2. On the manage tab, your submission status appears under the submission name.
* If the status is pending approval, your submission is still being verified. When it is in this state, you can't update or resubmit it.
* If the status is approved, your submission is approved to be listed in the appropriate marketplaces.
 
>Note: After your submission is approved in the Seller Dashboard, there might be a delay before it is published in a store. After approval, a submission typically appears in the store within 24 hours.

* If the status is changes requested, your submission needs changes in order to be approved. Choose your submission, and then on the summary page, choose "View the add-in report" for details about the required changes.

Failures will be explained and have appropriate policy violation references. All failures must be addressed before resubmission.

>**Note** If you make changes to an approved Teams experience, specifically as it relates to core functionality or the manifest, it must go through the approval process again.  For all other changes to your service, for example addressing issues or adding new features, resubmission is not required.

## Tips for rapid approval

* Make sure you include detailed testing notes and a valid working test account with appropriate prepopulated data.
* If your product requires an account on your or another service, list that in the description.
* If your product requires additional purchases to properly function, list that in the description.
* For your Tab config, make sure you provide About links and proper guidance - this page will be the first thing the user sees, so make sure a new user understands what to do.
* Check your manifest for completeness and accuracy.  Then check it again.
* Make sure your Bot provides appropriate responses when @mentioned in a channel as well as in 1:1 conversations as needed.  If your bot does not provide meaningful context within the personal or teams scope, [disable that scope via the manifest](schema.md#bots).
* Provide the requisite Terms and Privacy policy links in the manifest and the Seller Dashboard, that properly resolve to the correct documentation.  For bots, you must provide this same information in the Bot Framework registration page under the Submission section.
* Ensure all metadata in the manifest matches metadata in the Seller Dashboard, and for Bots, the Bot Framework registration.