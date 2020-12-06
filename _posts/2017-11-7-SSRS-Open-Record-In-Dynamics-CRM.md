---
title: "Opening a record in Dynamics from SSRS"
last_modified_at: 2017-11-7T18:00:00-00:00
categories:
  - Dynamics
  - SSRS
tags:
  - Useful Tips
header:
  teaser: /assets/images/posts/CRM_REST_javascript/1_CRMRestBuilder.png
---

One of the requests a user had recently was being able to click on the name of a sales document in a report and for it to open the record in Dynamics. I knew SSRS reports supported actions, but I wasn’t sure if Dynamics would allow a URL to be passed in – thankfully Microsoft had accounted for this requirement. This post will look at how to achieve this functionality.

Microsoft actually exposes a few parameters which can be used in the report out of the box, you just need to create the matching parameter in your report. Please go to [Technet](https://technet.microsoft.com/en-us/library/dn531165.aspx){:target="_blank"} to view the full list of parameters that can be used in SSRS reports. The parameter that I will be using in this example is CRM_URL.

The first step is to create a parameter in your report called ‘CRM_URL’, it’s important that you use the exact name otherwise the value will not be passed through. The URL will be concatenated with more information later on.

![Setting the CRM_URL parameters](/assets/images/posts/ssrs_open_in_crm/1_URL_Parameters.png)
Creating the URL Parameter

The next step is to create the action on the text box. This is achieved by going to the properties of the text box, at the bottom there is actions. This should be set to ‘Go to URL’.

![Setting the URL Action](/assets/images/posts/ssrs_open_in_crm/2_URL_Action.png)
Setting the action to 'Go to URL' and creating the expression

To concatenate the URL I have used an expression. The expression is assigning the GUID & the [Logical Name](https://social.technet.microsoft.com/wiki/contents/articles/9214.microsoft-dynamics-crm-2011-entity-logical-name-and-entity-schema-name.aspx){:target="_blank"}. The logical name will need to change if you want to open other entities.

```
=Parameters!CRM_URL.Value & "?ID={" & Fields!ac_salesorderid.Value & "}&LogicalName=salesorder"
```

**Update**: since using this functionality, I have encountered an issue which stops the link from working. For information about this please see this post.