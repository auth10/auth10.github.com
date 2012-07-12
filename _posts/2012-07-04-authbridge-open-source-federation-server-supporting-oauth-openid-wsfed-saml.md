---
layout: post
title: AuthBridge an open source federation server supporting OAuth, OpenID, WS-Fed, SAML
author: Matias Woloski
tags : [authbridge, oss, claims, identity, oauth, openid, saml, federation, dotnetopenauth]
keywords: google, adfs, identity, windows azure active directory, sharepoint 2010, oauth, openid
description: AuthBridge is a server written in ASP.NET/C# using WIF and DotNetOpenAuth, that speaks WS-Federation and SAML tokens on one side and OpenID, OAuth, WS-Federation or any other protocol on the identity provider..
---
{% include JB/setup %}

AuthBridge is a server written in ASP.NET/C# using [WIF](http://msdn.microsoft.com/en-us/security/aa570351.aspx) and [DotNetOpenAuth](http://www.dotnetopenauth.net), that speaks WS-Federation and SAML tokens on one side and OpenID, OAuth, WS-Federation or any other protocol on the identity provider. 

![](http://puu.sh/GzU1)

<!-- end preview -->

###Features
* Support Social Identity Providers: for Facebook (OAuth 2), Google (OpenID), Yahoo (OpenID), Twitter (OAuth 1.0a), Windows Live (OAuth 2). More can be added easily.
* Support for Enterprise Identity Providers like ADFS or IdentityServer using WS-Federation Protocol and SAML 1.1 or 2.0 Tokens. SAML 2.0 protocol could be added easily using the WIF SAML Extensions
* Support for Single Sign On
* Extensibility points to add more protocols
* Attribute transformation rule engine to normalize attributes coming from different identity providers

Read more about it at: **<http://authbridge.auth10.com> **

Online demo at: **<https://authbridge-sample.apphb.com>**

Get the code and contribute at: **<http://github.com/auth10/authbridge>**

