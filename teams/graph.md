# Using the Microsoft Graph in your Microsoft Teams pages

You may want to use the [Microsoft Graph](https://developer.microsoft.com/en-us/graph/) in your [configuration](createconfigpage.md) and [content](createcontentpage.md) pages.  For example, you might want to access the user's profile information, their calendar, their files, etc.

To use the Graph APIs, you must [get an access token](https://developer.microsoft.com/en-us/graph/docs/concepts/auth_overview).  When running inside Microsoft Teams, the only difference is that you must drive the authentication flow as described [here](auth.md).

Care must be taken if using the [Microsoft Graph APIs for team resources](https://developer.microsoft.com/en-us/graph/docs/api-reference/beta/resources/group), rather than just those for the current user.  This is because of differing consent models.  Typically, users can directly consent to your Microsoft Teams app within a specific team.  However, currently [an admin must also consent to your app (as registered in Azure AD) using these group APIs](https://developer.microsoft.com/en-us/graph/docs/concepts/permissions_reference#group-permissions), at which point it then has API access all of the groups/teams for each user. You should therefore ensure your Microsoft Teams app copes with not having the permissions it needs, and that it respects the user's intention about which teams it should operate in.

