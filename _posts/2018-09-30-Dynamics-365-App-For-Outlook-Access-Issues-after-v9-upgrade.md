---
title: "Dynamics 365 App for Outlook Access Issues after V9 Upgrade"
last_modified_at: 2018-09-30T18:00:00-00:00
categories:
  - Dynamics
tags:
  - App For Outlook
  - Error
header:
  teaser: /assets/images/posts/app-for-outlook-error/1_errormessage.png
classes: wide
comments: true
---
After I upgraded from Dynamics 365 v8.2 to v9 I lost access to the Dynamics App for Outlook on non-admin accounts. The error message below would appear whenever I clicked opened the app in Outlook.

{%
  include figure
  image_path="/assets/images/posts/app-for-outlook-error/1_errormessage.png"
  alt="Error: Don't have permissions to retrieve AppForOutlookModule"
  caption="Error: Don't have permissions to retrieve AppForOutlookModule"
%}

The error message shows that the permissions related to the AppForOutlookModule. I tested with various permissions and found the minimum was Create, Read & Write.

{%
  include figure
  image_path="/assets/images/posts/app-for-outlook-error/2_permissions.png"
  alt="Permissions required"
  caption="Permissions required"
%}
