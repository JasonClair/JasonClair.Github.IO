---
title: "Dynamics 365 Reports – Open Link Error"
last_modified_at: 2019-02-07T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Developer Toolkit
header:
  teaser: /assets/images/posts/ssrs-open-in-crm-link-error/SSRSError.jpg
classes: wide
comments: true
---

I have used SSRS to create Dynamics 365 reports for a while. This report shows the users some Opportunity information and lets them click on the Opportunity name to open the record (see [this post](/dynamics/Open-Record-Dynamics-CRM-From-SSRS-Report) for a guide on setting this up).

The report has worked well and the users have been happy with it. They have been exporting it to Excel to work with some of the numbers. The links within Excel have worked for them until recently when an error started to appear. The error was a generic “An error has occurred” message.

{%
  include figure
  image_path="/assets/images/posts/ssrs-open-in-crm-link-error/SSRSError.jpg"
  alt="Error displayed to the user"
  caption="Error displayed to the user"
%}

The error didn’t occur if the user copied the URL and pasted into a browser. It also didn’t occur when clicking the link within SSRS. I was also receiving the error, but a colleague didn’t – this made me think that it had to be machine related.

After some head scratching, my [colleague](http://www.antony-butcher.co.uk){:target="_blank"} was able to find something from Microsoft Support which resolved the issue. It was as simple as downloading the MSI from [Microsoft Support](https://support.microsoft.com/en-us/help/218153/error-message-when-clicking-hyperlink-in-office-cannot-locate-the-inte){:target="_blank"}. I was able to navigate to Dynamics through the Excel link as soon as this was installed.
