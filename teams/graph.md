# Using Microsoft Graph in your Microsoft Teams pages

You might want to use [Microsoft Graph](https://developer.microsoft.com/en-us/graph/) in your [configuration](createconfigpage.md) and [content](createcontentpage.md) pages. For example, you might want to access the user's profile information, calendar, or files.

To use the Microsoft Graph APIs, you must [get an access token](https://developer.microsoft.com/en-us/graph/docs/concepts/auth_overview).  When your app is running in Microsoft Teams, the only difference is that you must drive the authentication flow, as described in [Authenticating a user in your Microsoft Teams pages](auth.md).

Be careful if you use the [Microsoft Graph APIs for team resources](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/group), rather than those for the current user, because the two have different consent models. Typically, users can directly consent to your Microsoft Teams app within a specific team. However, currently [an admin must also consent to your app (as registered in Azure AD) using these group APIs](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference#group-permissions), at which point the app then has API access to all the groups/teams for each user. You should therefore ensure that your Microsoft Teams app handles not having the permissions it needs, and that it respects the user's intention about which teams it should operate in.

