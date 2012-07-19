---
layout: post
title: Simpler WIF integration for ASP.NET web application using cloud friendly appSettings
author: Matias Woloski
tags : [auth10, identity, adfs, acs, sharepoint, claims, federation, jabbr]
keywords: identity, windows azure active directory, wif, aspnet
description: Use a simpler mechanism to integrate ASP.NET applications using appSettings
---
{% include JB/setup %}

Last week we've spent some time contributing to [Jabbr](https://github.com/davidfowl/JabbR) (the open source chat app based on SignalR). [David Fowler](https://twitter.com/davidfowl), the main dev behind the project, expressed on twitter that it would be great to have enterprise sign-on support on [Jabbr](https://github.com/davidfowl/JabbR) so that it could be used as a chat system on the enterprise. 

Being a [single page application](http://en.wikipedia.org/wiki/Single-page_application), we thought it would be a good idea to integrate it with [Windows Identity Foundation (WIF)](http://msdn.microsoft.com/en-us/security/aa570351.aspx), learn a couple of things to make this scenario much simpler and bring that back to [Auth10](http://auth10.com) while also contributing to [Jabbr](https://github.com/davidfowl/JabbR). We went ahead and forked the [Jabbr repo](https://github.com/davidfowl/JabbR) and within a couple of hours we had it working and a [pull request](https://github.com/davidfowl/JabbR/pull/525).

We extracted what we learnt from this experience and packaged it into a couple of NuGets: [Auth10.AspNet.SimpleConfig](http://nuget.org/packages/Auth10.AspNet.SimpleConfig) and [Auth10.AspNet.SimpleConfig.WindowsAzureAD.IdentitySelector](http://nuget.org/packages/Auth10.AspNet.SimpleConfig.WindowsAzureAD.IdentitySelector). 

<!-- end preview -->

### Screencast

<a href="http://www.youtube.com/watch?v=Ev9aUmxQTCc" target="_blank" title="Screencast: Configuring an ASP.NET application to accept Google and ADFS identities"><img alt="Screencast: Configuring an ASP.NET application to accept Google and ADFS identities" src="http://puu.sh/JRHy" /></a>

### What we've learnt from Jabbr

* There is a single page with the page structure and the rest is in JavaScript.
* Jabbr stores configuration items on appSettings to be cloud-friendly. Clouds like Windows Azure Web Sites or AppHarbor allows you to override config from appSetting but you can't change complex config sections.
* Jabbr has its own mechanism to track a user logged in, they don't use IPrincipal.
* Jabbr has two authentication mechanism: user and password or social identity providers (via JanRain)
* Once the user is logged in it will use SignalR in a trusted subsystem (i.e. trusting the cookie set on login)
* The Jabbr code is very clean and well structured!

In this scenario we need non-intrusive, easy to integrate and minimum footprint code so that we don't break things and adapt to whatever structure the application already have.

### Less complexity, less footprint, less intrusive

Sometimes frameworks hide complexity away and leave us developers in hard to debug and extend systems. So we thought of doing this WIF integration using the least common denominator approach that everyone could understand.

We've spent some time packaging that in a seamless experience using NuGet.

	Install-Package Auth10.AspNet.SimpleConfig

The NuGet package will add the following settings to `<appSettings>`

	<add key="fedauth.identityProviderUrl" value="https://auth10-preview.accesscontrol.windows.net/v2/wsfederation" />
    <add key="fedauth.realm" value="urn.....ample-app" />
    <add key="fedauth.replyUrl" value="" />
    <add key="fedauth.certThumbprint" value="B538E6F6....B529F716" />
    <add key="fedauth.requireSsl" value="true" />
    <add key="fedauth.enableManualRedirect" value="false" />

WIF SDK provides the **Add STS reference** wizard, we provide an equivalent to that in a form of a NuGet CmdLet. From the NuGet Package Manager console run the following:

	Set-FederationParametersFromWindowsAzureActiveDirectory 
			-realm urn...ample-app 
			-serviceNamespace auth10-preview

That will write the values of the above `<appSettings>`. We also provide a more generic CmdLet: `Set-FederationParametersFromFederationMetadataUrl` and `Set-FederationParametersFromFederationMetadataFile`

The NuGet will also inject a slightly customized version of the WIF modules using App_Start WebActivator (or if it's NET 3.5 the NuGet will add them under `<httpModules>`).

	public static void PreAppStart()
    {
        DynamicModuleUtility.RegisterModule(typeof(CustomWSFederationAuthenticationModule));
        DynamicModuleUtility.RegisterModule(typeof(CustomSessionAuthenticationModule));
    }

It will add the request validator that will allow tokens to be posted to the application.

	<httpRuntime requestValidationMode="2.0" 
				 requestValidationType="$rootnamespace$.FederatedIdentity.Infrastructure.AllowTokenPostRequestValidator" />

It will set the authentication mode to none and deny access to anonymous users. This is protecting the whole site but can be changed to use `[Authorize]` attribute on MVC or another authorization mechanism.

    <authentication mode="None">
	<authorization>
      <deny users="?" />
    </authorization>

It will add a static helper class with a few methods to allow triggering the login process programmatically instead of relying on the modules. It also provides the logoff methods that makes more explicit the logoff implementation.

	FederatedIdentityHelper.LogOn([issuer], [realm], [homeRealm])

	FederatedIdentityHelper.LogOff();

	FederatedIdentityHelper.FederatedLogOff(idpSignoutUrl, [replyUrl]);

### Adding an identity provider selector

Another thing we've extracted from this experience is the concept of the identity selector. If you run the following NuGet, it will add to your application a small JavaScript component that will query Windows Azure Active Directory and build a list of identity providers that are configured for your application.

	Install-Package Auth10.AspNet.SimpleConfig.WindowsAzureAD.IdentitySelector

This NuGet provides a small snippet that you can add wherever you want in your app to show the selector.

	<script src="/Scripts/waad.selector.js" type="text/javascript"></script>
    <script type="text/javascript">
        $("#logon").click(function () {
            window.waadSelector('auth10-preview',
                                'urn.....ample-app',
                                $("#identityProviderSelector"));

            // use jQuery UI to show the modal dialog (or any other javascript library)
            $("#identityProviderSelector").dialog({ modal: true });
            return false;
        });
    </script>

The markup generated by the selector looks like this. We are generating CSS class with the pattern "selector-identityProvider" so that you could customize with a logo using CSS background-url for instance.

	<div id="identityProviderSelector">
		<ul>
			<li>
				<a href="https://www.google.com/...." 
					alt="Login with Google" 
					class="selector-Google">Google</a>
			</li>
			<li>
				<a href="https://..." 
					alt="Login with Contoso AD" 
					class="selector-Contoso-AD">Contoso AD</a>
			</li>
		</ul>
	</div>


This is a screenshot from the [screencast](http://www.youtube.com/watch?v=Ev9aUmxQTCc) showing the selector. This is using jQuery UI and a simple ul/li but you could customize it the way you want (like we did on <https://auth10.com/account/logon>).

!["Windows Azure Active Directory identity provider selector"](http://puu.sh/JSit)

### Conclusion

Auth10 mission is to democratize federated identity, making it simpler and easier. We know the only way to do that is by helping you, developers, providing the less intrusive, transparent and clean integrations. Go ahead and try the NuGet packages and the [Auth10 dashboard](http://auth10.com). Plaese [let us know what you think and how can we help you](http://auth10.uservoice.com).

