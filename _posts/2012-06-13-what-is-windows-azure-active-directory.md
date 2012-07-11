---
layout: post
title: What is Windows Azure Active Directory?
tags : [identity, adfs, acs, claims, federation, waad, azure]
author: Matias Woloski
---
{% include JB/setup %}

On Monday, June 11, Stuart Kwan presented on Teched North America about [Windows Azure Active Directory](https://www.windowsazure.com/en-us/home/features/identity/). If you want to get an idea of where Microsoft is heading with regards to Identity and Access Management, watch the session.
<http://channel9.msdn.com/Events/TechEd/NorthAmerica/2012/SIA209>

You can see the slides [online](http://view.officeapps.live.com/op/view.aspx?src=http%3a%2f%2fvideo.ch9.ms%2fteched%2f2012%2fna%2fSIA209.pptx)

## TL;DR

Windows Azure Active Directory is or will be: 

* A projection of your AD in the cloud (and for those who don't have AD, like mom & pop shops, it will be their single AD).
* It has a REST HTTP interface to query its data (called the Graph API).
* It is a federation provider Security Token Service (what today is Windows Azure Access Control Service) and will be an identity provider in the future.

Your investments in federation, claims based identity and standard-based protocols will pay off :)

<!-- end preview -->

## Notes

### General direction

Sync + federation

> comment: sync to avoid latency querying AD information and federation to authenticate users within their corporate boundaries.

Windows Azure AD is a projection of your on premise AD. It will have a federation server (currently ACS but in the future they will merge), the multi tenant directory and the graph API (think LDAP with a nice HTTP REST + JSON protocol).

Protocols that Microsoft will keep investing on: WS-Fed, SAMLP, OAuth2, OpenID Connect (future).

### What scenarios they are targeting for the enterprise (big and small)?

For the big companies: AD is a projection of your on-premise AD (without the password hashes). 

> comment: I can imagine a people picker for SharePoint wired to use Windows Azure AD Graph API.

For the small companies: used as your main directory supporting built-in federation with Office365 suite and in the future with other (custom or third party) cloud apps.

> comment: this makes sense, but I would argue that currently Google Apps is better positioned to be the identity provider for small companies (don't have adoption numbers, just perception). The good news is that if they both support the protocols (WS-Fed, SAMLP, OpenID connect who knows what), then it doesn't really matter which one ends up being.

### What scenarios they are targeting for developers?

Use Windows Azure AD to increase adoption of your web app by accepting identities from Windows Azure AD.

Use Windows Azure AD (together with social idps) as the identity system/hub.

> comment: similar to what ACS provides today but adding the capability of authenticating also with your or others cloud AD. Your website with "login with Facebook", "login with Windows Live"... "login with Windows Azure AD". For apps that live within Microsoft boundaries (Marketplace, etc) this will make sense.

### What is in the preview?

* Access to the REST graph API (directory.windows.net is the base url).
* No production SLA.
* It's a completely separate namespace from ACS (although if you notice the URLs in the demo, it's a special ACS instance accounts.accesscontrol.windows.net).
* Integrating with Windows Azure Active Directory for Web Authentication will be done through the usual WS-Fed for now and there will be samples in different platforms (PHP, Java, NET).

### How do you add trust relationships to Windows Azure Active Directory?

Today it is only supported through some PowerShell CmdLets. You create a ServicePrincipal pointing to the app you want to issue tokens.

### How do you consume the Graph API?

First you need probably a JWT token obtained through OAuth. Grants might be given through the Windows Azure AD portal UI.
Then you make plain HTTP calls that will return JSON.

	GET https://directory.windows.net/contoso.com/Users('ed@contoso.com')
	Authorization: Bearer ..sometoken..

	{ “d”:	
		{
		  "Manager": { "uri": "https://directory.windows.net/contoso.com/Users('User...')/Manager" },
		  "MemberOf": { "uri": "https://directory.windows.net/contoso.com/Users('User...')/MemberOf" },
		  "ObjectId": "90ef7131-9d01-4177-b5c6-fa2eb873ef19",
		  "ObjectReference": "User_90ef7131-9d01-4177-b5c6-fa2eb873ef19",
		  "ObjectType": "User", "AccountEnabled": true,
		  "DisplayName": "Ed Blanton",
		  "GivenName": "Ed", "Surname": "Blanton",
		  "UserPrincipalName": "Ed@contoso.com",
		  "Mail": "Ed@contoso.com",
		  "JobTitle": "Vice President", "Department": "Operations",
		  "TelephoneNumber": "4258828080", "Mobile": "2069417891",
		  "StreetAddress": "One Main Street", "PhysicalDeliveryOfficeName": "Building 2", 
		  "City": "Redmond", "State": "WA", "Country": "US", "PostalCode": "98007"
		}	
	}


### How is this all related to [Auth10](http://auth10.com)?

[Auth10](http://auth10.com)’s mission is to help companies easily and securely connect applications in the cloud, on-premises and mobile with its employees, customers, partners and users. The industry has changed and is trending towards a federated, standards-based identity as the most elegant, robust and secure way to solve this challenge. Windows Azure Active Directory is a testament to that. However, there is still a need for simplification and best practices, and that’s where [Auth10](http://auth10.com) helps.
