---
layout: post
title: a sneak peak of what we're building for Auth10...
tags : [auth10, identity, adfs, acs, sharepoint, claims, federation]
---
{% include JB/setup %}

####The Problem
Your company has 20+ apps (web, desktop, web services), 100+ of users (some are employees others are external people that work for you), different platforms (.NET, SharePoint, PHP, Java, node.js, etc.), different authentication and authorization implementations (AD, LDAP)...managing __who__ can access __what__ and __how__ is complicated and getting worse with the addition of *cloud* based apps and *mobile* growth.

####One Step Ahead
[Claims based identity](http://msdn.microsoft.com/en-us/library/ff423674.aspx) and identity federation are mature and proven solutions for these challenges. Standards like SAML, OAuth and WS-Federation are being adopted more widely, but many have just given up: too many levers, too many options, too intrincate and obscure technologies. It still requires a very deep understanding of what's really going on to successfully deploy a solution based on this architecture.

####Auth10
[__Auth10__](http://auth10.com) hides all this complexity and provides a higher abstraction layer on top of the core building blocks of claims based identity. It allows you quickly define and __configure__ your __applications__, the __users__ of those applications regardless of who they are and how they prove their identity; and finally connect both with a __policy__ that defines what they can do. And because __Auth10__ has an end to end knowledge of all components involved, it can easily pinpoint issues, helping you __troubleshoot__ in minutes what before took long hours of trial and errors.


###Dashboard
The Dashboard gives you a consolidated view of all your applications and users within your organization and your partners. In one view you can easily identify who can access what and if there are any problems:

![](http://markdownr.blob.core.windows.net/images/6956447565.png)

###Applications
Every time you create an app, choose from the catalog of application types (SharePoint 2010 and ASP.NET initially) to tailor and accelerate the federated identity configuration time. Claims enabling an app (like SharePoint) can take days for those new to the subject. Auth10 automates this and will claims enable SharePoint in minutes.

![](http://markdownr.blob.core.windows.net/images/6179843389.png)

###User groups
Every time you identify a logical group of users (your employees or vendors, managed identities or not), choose from the catalog of authentication types (ADFS, Facebook and Google initially) to tailor and accelerate the federated identity setup time for those identity providers.

![](http://markdownr.blob.core.windows.net/images/5011326206.png)

###Rules
Create and connect applications with group of users and define rules to satisfy application access control requirements

![](http://markdownr.blob.core.windows.net/images/6749064028.png)

###Rule templates
Quickly connect applications with user groups with different access control strategies by clicking on one of the templates (passthrough and custom initially)

![](http://markdownr.blob.core.windows.net/images/3939488281.png)

###Analytics
Browse detailed statistics for all your applications. Analyze who, when and what applications have been accessed.

![](http://markdownr.blob.core.windows.net/images/6729874989.png)

###Troubleshooting
Quickly identify problems with the __Auth10 Probing Tools__, troubleshoot interactions between the applications and identity providers by looking at all interactions between components. Use __Auth10 Probing Tools__ to extract meaningful error messages.

![](http://markdownr.blob.core.windows.net/images/837964497.png)
