# Submission and Manifest Metadata Checklist

The follow metadata is required in your manifest.json file and/or for Seller Dashboard submission:

|Data|Type|Size|Manifest|Seller Dashboard|Description|
|---|---|---|---|---|---|
|App Package|Zip|||✔|This is the actually app package used for sideloading or Store submission in the App package field.|
|App Logo|.png|96x96px|`icon.color`|✔|The icon to display in the product page listing in the Office Store / Teams gallery. This is your full color product icon.|
|App Logo Outline|.png|20x20px|`icon.outline`||The icon to display in Teams, in the Teams chat channel and other locations.  This is your logo rendered a white outline with transparent background.|
|Support Link|URL|||✔|A link to support material for end users.  May be http or https.|
|Privacy Link|URL||`developer.privacyUrl`|✔|A link to your privacy policy (https).|
|Video Link|URL|||Opt|A link to a video about your app.|
|EULA|.doc/.pdf/etc|||Opt|The Office Store requires an end user licensing agreement, which you may provide as an attachment.  If you choose not to submit, one will be provided on your behalf.| 
|Terms of Service|URL||`developer.termsOfServiceUrl`||A link to your terms of service (https).|

## Localized content

Please Note:  In the future, Teams will support localized content for the following metadata.  At this point, only English is supported for Teams apps.
|Data|Type|Size|Manifest|Seller Dashboard|Description|
|---|---|---|---|---|---|
|App Name|String|30|`name.short`|✔|The name for your application as it should appear in the storefront and in product.|
|Long App Name|String|30|`name.full`|✔|The name for your application as it should appear in the storefront and in product.|
|Short description|String|80|`description.short`|✔|Short description of your app.|
|Long description|String|4000|`description.full`|✔|A more detailed description of your app.  In the manifest file, an accurate summary is adequate.  In the Seller Dashboard, you may use a richer and formatted description for the Store product page.|
|Screenshots (1..5)|.png,.jpg,.gif|1366w x 768h, less than 1024KB||✔|At least one screenshot that shows your app experience.  For use on the app details page.|

## Submission extras for Bots

Bots in Microsoft Teams must be built and registered in the Bot Framework.  Teams pulls some information from the Bot Framework to display to end users, so you must ensure you provide the following information on the Bot Framework Dev Portal as well as in the Teams Manifest.

On your bot portal page, click the Publish button.  In this dialog there are several required fields that you must enter:
* Icon - you should use your color 96x96 icon
* Privacy statent - must match the Privacy Link URL you provide in your manifest and Seller Dashboard
* Terms of Use - must match the Terms of service URL you provide in your manifest

Fill in the above and hit **Save**.  Please Note: there is no requirement to Publish the bot to the Bot Framework Bot Directory.

