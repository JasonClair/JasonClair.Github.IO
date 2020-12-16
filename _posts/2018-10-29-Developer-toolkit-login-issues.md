---
title: "Dynamics 365 Developer Toolkit Login Issues"
last_modified_at: 2018-10-29T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Developer Toolkit
header:
  teaser: /assets/images/posts/developer-toolkit-login-issues/1_hiddenerror.png
classes: wide
comments: true
---
The [Dynamics 365 Developer Toolkit](https://marketplace.visualstudio.com/items?itemName=DynamicsCRMPG.MicrosoftDynamicsCRMDeveloperToolkit){:target="_blank"} is a helpful extension to Visual Studio that saves time when developing new plugins & workflows for Dynamics.

Recently though, I had an issue with the application where it would not let me login to any instances. The login screen would appear but would have forgotten all previously stored data. When new credentials were entered and the login button clicked nothing would happen. When trying to close Visual Studio an error would appear hinting that a hidden error message had popped up.

![Error message appeared when closing Visual Studio](/assets/images/posts/developer-toolkit-login-issues/1_hiddenerror.png)

After investigation, I realised I needed to delete the appdata directory for the Developer toolkit because it had become corrupted. Deleted the directory allows it to be recreated next time the application is loaded. The directory I deleted was: C:/Users/&lt;USERNAME&gt;/appdata/roaming/microsoft/CRMDeveloperToolkit