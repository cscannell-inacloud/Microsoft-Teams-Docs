# Get context for use in your Microsoft Team (preview) tab

Your tab may require contextual information in order to display the necessary content.

* Your tab may need basic information about the user, team, or company.
* Your tab may need locale and theme information.
* Your tab may need to read back the `entityId` or `subEntityId` that identifies what is in this tab.

## User context

Context about the user, team or company can be especially useful when:

* You need to create or associate resources in your app with the specified user or team.
* You want to initiate an authentication flow against Azure Active Directory or other identity provider, and you don't want to require the user to enter their username again. (For more information on authenticating within your Microsoft Teams tab, see [Authenticating in your Microsoft Teams tab pages](auth.md).)

>**Important:** While this user information can help provide a smooth user experience, you should **not** use it as proof of identity. For example, an attacker could you load your page in a 'bad browser' and provide it with any information they want.

To obtain this identifying information about the user, team or company, you must set the `needsIdentity` element in your tab manifest to `true`. Doing so will prompt the user for their consent when they add your tab.  

## Accessing context

There are two ways to access context information:

* Inserting URL placeholder values
* Using the [Microsoft Teams Tab library](jslibrary.md)

### Getting context through inserting URL placeholder values

Use placeholders in your configuration or content URLs. Microsoft Teams replaces the placeholders with the relevant values when determining the actual configuration or content URL to navigate to. The available placeholders include all fields on the [Context](jslibrary.md#Context) object. Common placeholders include:

* {entityId}: The ID you supplied for the item in this tab when first [configuring the tab](createconfigpage.md)
* {subEntityId}: The ID you supplied when generating a [deeplink](deeplinks.md) for a specific item _within_ this tab.  This should be used to restore to a specific state within an entity, for example scrolling to or activating a specific piece of content.
* {upn}: The user name
* {groupId}: The ID of an Office 365 group
* {tid}: The company ID, i.e., tenant ID
* {locale}: The user locale, such as 'en-us'

For example, suppose in your tab manifest you set the `configURL` attribute to:

`"https://www.contoso.com/config?name={upn}&tenant={tid}&group={groupId}"`

And the signed-in user has the following attributes:

* Their username is 'user@example.com'
* Their company tenancy ID is 'e2653c-etc'
* They are a member of the Office 365 group named 'test' 

When they select your tab, they will be navigated to:

`https://www.contoso.com/config?name=user@example.com&tenant=e2653c-etc&group=test`


### Getting context through using the Microsoft Teams Tab library

You can also retrieve the information listed above using the [Microsoft Teams Tab library](jslibrary.md), by calling `microsoftTeams.getContext(function(context) { /* ... */ })`.

In addition, you can also register your app to be told if the theme changes by calling `microsoftTeams.registerOnThemeChangeHandler(function(theme) { /* ... */ })`.
