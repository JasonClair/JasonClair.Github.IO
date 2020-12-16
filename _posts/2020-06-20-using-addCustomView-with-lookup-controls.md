---
title: "Using addCustomView with Lookups"
last_modified_at: 2020-06-20T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
header:
  teaser: /assets/images/posts/addCustomView-with-lookups/3_FilteredLookup.png
classes: wide
comments: true
---
I recently wanted to filter a Lookup within Dynamics 365 to show Contacts where the linked Accounts’ Parent Account was a specific Account. The idea being that the user can select any Contact from the Account Hierarchy but shouldn’t be able to select any Contacts from outside of that structure.

Normally, I would use [addCustomFilter](https://docs.microsoft.com/en-us/powerapps/developer/model-driven-apps/clientapi/reference/controls/addcustomfilter){:target="_blank"} & [addPreSearch](https://docs.microsoft.com/en-us/powerapps/developer/model-driven-apps/clientapi/reference/controls/addpresearch){:target="_blank"} to perform this action but I was receiving an error regarding the linked entity not existing. A bit of research suggests that the preFilter cannot have a linked entity included.

Whilst researching this functionality, I found the [addCustomView](https://docs.microsoft.com/en-us/powerapps/developer/model-driven-apps/clientapi/reference/controls/addcustomview){:target="_blank"} function which allows a custom view to be created using fetchXml and layoutXml (a tip on getting these is [here](/dynamics/getting-fetchxml-and-layoutxml-from-advanced-find)). This can then be assigned to the lookup and used as a filter to only show specific records.

## Account Structure

In this example, I am using the Accounts & Contacts in the image below. I will be viewing the Parent Account or Child Account records and will be expecting only users assigned to either of these show in the lookup.

![View showing the contacts with Dynamics 365](/assets/images/posts/addCustomView-with-lookups/1_Contacts.png)

## Before

As would be expected, without including any custom filters all of the contacts are shown when trying to use the lookup.

![All contacts showing as no filter in place](/assets/images/posts/addCustomView-with-lookups/2_UnfilteredData.png)

## JavaScript

Adding the below JavaScript and running the function on Page Load will apply the custom view. In this example, I have hardcoded the GUIDs the ID of the Parent Account.

```javascript

function filterContacts() {
    // the view ID can be anything unique
    var viewId = "{00000000-0000-0000-0000-000000000003}";
    var entityName = "contact";
    var viewDisplayName = "Contacts In Account Family";
    
    fetchXml ="<fetch version=\"1.0\" output-format=\"xml-platform\" mapping=\"logical\" distinct=\"false\"><entity name=\"contact\"><attribute name=\"fullname\"/><attribute name=\"contactid\"/><attribute name=\"parentcustomerid\"/><order attribute=\"fullname\" descending=\"false\"/><link-entity name=\"account\" from=\"accountid\" to=\"parentcustomerid\" link-type=\"inner\" alias=\"ac\"><filter type=\"and\"><filter type=\"or\"><condition attribute=\"accountid\" operator=\"eq\" uiname=\"Parent Account\" uitype=\"account\" value=\"{6AA4C0E1-679B-EA11-A812-000D3A0BA151}\"/><condition attribute=\"parentaccountid\" operator=\"eq\" uiname=\"Parent Account\" uitype=\"account\" value=\"{6AA4C0E1-679B-EA11-A812-000D3A0BA151}\"/></filter></filter></link-entity></entity></fetch>";

    var layoutXml = "<grid name=\"resultset\" object=\"2\" jump=\"lastname\" select=\"1\" icon=\"1\" preview=\"1\"><row name=\"result\" id=\"contactid\"><cell name=\"fullname\" width=\"300\"/><cell name=\"parentcustomerid\" width=\"100\"/></row></grid>";

    Xrm.Page.getControl("jason_contactid").addCustomView(viewId, entityName, viewDisplayName, fetchXml, layoutXml, false);
    Xrm.Page.getControl("jason_contactid").setDefaultView(viewId);
}

```

## After

After publishing the changes & reloading the page, you can now see that the Contact lookup is filtered to only show the the Contacts linked to the Parent Account.

![Lookup showing only contacts with the parent child account structure](/assets/images/posts/addCustomView-with-lookups/3_FilteredLookup.png)

## Notes

This was an interesting way of getting around the preFilter not accepting a linked entity and was easy to add into the solution. One thing to note is that this will not prevent items showing in the ‘Recents’ view for the lookup, so I would recommend disabling that too, information about how to do that is in this [post](/dynamics/disable-recents-on-lookup).
