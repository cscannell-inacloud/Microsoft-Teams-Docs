# Requirements for Microsoft Teams tabs app pages

> Please note: use of the Microsoft Teams tab SDK is subject to the [Terms of Use](https://aka.ms/bf-terms), [Privacy Statement](https://aka.ms/bf-privacy), and [Code of Conduct](https://aka.ms/bf-conduct) for the Microsoft Bot Framework (Preview).

All tab content, including configuration, content and tab removal pages must meet the following requirements:

* Pages must be hosted on a secure https:// endpoint.  Microsoft Teams will not display insecure http:// content.

* Your content must work in an iframe. By default web pages can be iframed by anyone. You may optionally set these headers if you wish to only allow your page to be iframed by Microsoft Teams for extra security:
	
	* Set header `Content-Security-Policy: frame-ancestors teams.microsoft.com *.teams.microsoft.com *.skype.com`. Most modern browsers support this.

		* For Internet Explorer 11 compatability, Set X-Content-Security-Policy as well.

	* Alternately, set header `X-Frame-Options: ALLOW-FROM https://teams.microsoft.com/`. This header is deprecated but still respected by most browsers.

* Include the [Microsoft Teams Tab library](jslibrary.md) in your page as a script source.

	`<script src="https://statics.teams.microsoft.com/sdk/v1.0/js/MicrosoftTeams.min.js" />`

* Once your page has successfully loaded, call `microsoftTeams.initialize()` to display your page. Microsoft Teams will not display your page unless you do so.

* All domains for pages you display in your tab(s) must be listed in the manifest's `validDomains` list.  See [here](schema.md#validDomains) for more information.

> Hitting problems?  See the [troubleshooting guide](troubleshooting.md).

>**Tip:** For developers using TypeScript, Microsoft Teams provides a [definition file](https://statics.teams.microsoft.com/sdk/v0.4/types/MicrosoftTeams.d.ts) to enable IntelliSense or similar support from your code editor as well as compile-type type checking as part of your build.
