---
layout: post
title: Guide to Office 365 Multi-factor Authentication
description: Understand components of Office 365 multi-factor authentication, and compare to other implementations including Google and LastPass.
imgurl: images/blog-banner.jpg
---


* Intro

* Setup MFA for users
  * Device/app options
  * App Passwords
  * Setting up on mobile
  * Setting up on Office clients

* Issues
  * Use of native mail apps

* Comparison

* Summary/References

This article is a step by step guide to configuring multi-factor authentication for your tenant, and helping end users configure multi-factor authentication on their devices.  This is the first article, and we will go through selecting users and then enabling MFA for those users.

There are many specific scenarios for multi-factor authentication.  I'll walk through the two most common scenarios:

1. Enabling Multi-factor authentication for Office 365 Admins.

*I strongly advise the first scenario is non-negotiable. The potential damage (and cost) of a compromised admin account will be greater than other user accounts. It is also expected that these users are familiar with technical concepts, and adoption of new processes will not be difficult.*


2. Enabling Multi-factor authentication for a subset of users which exist in an Office 365 tenant.

*The second scenario is certainly advisable, however there may be constraints which limits the practicality of MFA in its current form. This could be due to location, organisation type or size. However MFA is certainly becoming more familiar to consumers who use Gmail or LastPass, for example.*

## Enabling Multi-factor authentication for Office 365 Administrators

Log into Office 365 Admin console, to begin configuration. These instructions refer to the new Admin portal layout, which differs to the previous portal for navigation.

Select `Settings > Services & Add-ins`, and click `Azure Multi-factor authentication.`

![Azure MFA window]()

A side panel will open,  click `Manage Multi-factor Authentication`, and a new window will open.

![Azure MFA search screen]()

This search window is the key gateway to managing MFA for users. The two filters at the top are important:

* View

*This will allow you to filter on admin group roles. You can bulk configure MFA for users of these groups. Note `Sign-in allowed users` essentially means all users who can currently sign-in.*

* Multi-factor authentication status

*This allows you to identify users who have currently enabled/forced MFA. This is more useful for managing MFA as an on-going management to enable/disable specific users.*

For this example, we will enable MFA for all Global administrators.

Click the `View` dropdown, and select `Global Administrators.`

This will update the list with users who are global administrator users.

![Global Admin Users]()

You can select all to enable MFA for all users, but I would advise you setup and test one or two users first, to confirm it behaves as you expect it to.
To select a user, check the checkbox associated to the users name.  Then click the `Bulk Update` button at the top of the page (even if it is one user selected).
