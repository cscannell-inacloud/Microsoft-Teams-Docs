# Submitting your Microsoft Teams app in the Seller Dashboard

For a general overview of the Microsoft Teams app submission flow, see [here](submission.md).

## Steps to submit a new app

In the Seller Dashboard:

1. Enter the Office dashboard by clicking "Continue" under the Office heading.

![Office Seller Dashboard entry point](images/submission/SellerDashboardOfficeEntry.PNG)

2. "Add a new app" to initiate the submission process.

![Office Seller Dashboard add an app](images/submission/SellerDashboardAddApp.PNG)

3. Select the "Teams App" option.

![Office Seller Dashboard Overview](images/submission/SDAppType.PNG)

4. Enter your product Overview information

![Office Seller Dashboard Overview](images/submission/SDOverviewCrop.PNG)

* **App Package** - upload your properly formatted 1.0 Manifest.  Note that the manifest information will prepopulate many of the fields below.
* **Submission title** - this is your app name
* **Version** - autopopulated from the manifest
* **Release date** - by default, the date here will be today's.  This means that your app would be eligible to be published as soon as it passed validation.  If you wish to delay publication to a future date, you can specify that here.  Please note: validation times vary and there is no guarantee the validation process will be done by the specificed date
* **Category** - You can select up to three categories in which to appear in the Office Store.  Please select ones which best match your experience.
* **Testing notes** - Please ensure you provide enough information for our validation team to successfully load and test your experience.  This includes providing login instructions and test accounts, as well as any other notes that might assist in the review of your product.
* **My app calls, support, contains, or users cryptology or encryption** - This checkbox ensures your product is not using cryptography in a way that would prohibit Store distribution.  See [here](https://docs.microsoft.com/en-us/windows/uwp/security/export-restrictions-on-cryptography) for a detailed explanation.
* **App logo** - use your full color 96x96 icon
* **Support document link** - please supply a URL to support content for your experience
* **Privacy document link** - please use the same URL from your manifest
* **Video link** - this optional field can be used to show a video of your experience.  This video will appear on your product listing page in the Office Store.
* **End User License Agreement** - if you need a custom EULA, please upload a copy here

Select the "Next" button when all required information has been filled in.

4. Enter your product Detail information

![Office Seller Dashboard app details](images/submission/SDDetails.PNG)

* **Language** - Select the language your app supports.  Please note: currently there is no way to provide an app with multiple languages.  You may submit your app in one language only.
* **App Name** - This field would hold your localized name, but for now this should be the same as your app name in the manifest.
* **Short Description** - Please use your short description from the manifest.
* **Long Description** - This description is your opportunity to present your experience to end users, and will be shown in the app discovery flow in Microsoft Teams.  While you may use your long description from the manifest, feel free to add more content and take advantage of all the formatting options available to you.
* **Screenshots** - You must include at least one 1366x768 screenshot.  Please note that your screenshot must accurately reflect your Microsoft Teams app experience.

Click the "Next" button when all required information has been filled in.

5. Opt out of any regions you do not wish to be distributed in.

![Office Seller Dashboard block regions](images/submission/SDBlockRegions.PNG)

If there are reasons why you do not which to distribute in all regions where the Office Store is available, check the box and select from the regions where you do NOT wish to appear.

![Office Seller Dashboard available regions](images/submission/SDRegions.PNG)

6. Lead Management screen - Currently not supported in teams.

Click the "Next" button.

7. Pricing - currently Microsoft Teams only allows Free apps.

![Office Seller Dashboard price and submit](images/submission/SDPricing.PNG)

If you have filled everything in correctly, you can "Submit for approval."  Or you can "Save as draft" to return later.

For more information on the validation process, see [Teams app approval process](submission.md#teams-app-approval-process).

