---
title: "Dynamics 365 – Show SharePoint Documents"
last_modified_at: 2019-02-17T18:00:00-00:00
categories:
  - Dynamics
tags:
  - SharePoint
header:
  teaser: /assets/images/posts/show-sharepoint-documents/DocumentsShown.png
classes: wide
comments: true
---

In a previous [post](/dynamics/Dynamics-365-Enable-SharePoint-Document-Management), I showed how to enable Dynamics 365 SharePoint integration. This allows you to store attachments in SharePoint instead of Dynamics – which is a lot cheaper and allows for better document management.

One of the issues with using SharePoint to store the attachments is that they cannot be shown on Dynamics forms out of the box. This means that the user has to use the navigation menu to see them. In my experience this can cause frustration for the users.

This is an easy fix by using JavaScript and an IFrame on the form. The content of the IFrame is set by JavaScript. In my case, I have also used a separate tab to have the IFrame in. I do this so that I can hide the entire section if there is no associated Document Location.

This idea was originally found on [Jason Lattimer’s blog](https://jlattimer.blogspot.com/2017/01/show-sharepoint-documents-on-main-form.html){:target="_blank"}, but I have modified it for my own needs by adding the tab functionality. I have also updated the code to use formContext instead of Xrm.Page to future proof it.

## Form Setup

I use the components below on my form. I recommend using the settings in the IFrame to give the cleanest look, but these changeable based on your requirements.

* Tab
* IFrame
  * Restrict cross-frame scripting has to be disabled
  * Number of Rows set to 34
  * Scrolling set to Never
  * Display Border disabled

## Script Setup

The following script then needs to be added to the on-load event of the Form with the formContext passed as the initial variable & then the tab name and IFrame name following it.

``` javascript
/// Show Documents sub-grid on the main form
function setDocumentsIFrame(executionContext, tabName, iframeName) {
    var formContext = executionContext.getFormContext();

    // Hide the tab until we're sure there's something to show
    var tab = formContext.ui.tabs.get(tabName);
    if (tab === null) {
        console.log("Could not find tab: " + tabName);
        return;
    }
    tab.setVisible(false);

    // Only want to run the code if the record is just being created, otherwise
    // there will no documents to show anyway
    if (formContext.ui.getFormType() !== 1) {
        var iframe = formContext.getControl(iframeName);
        if (iframe === null) {
            console.log("Could not find iframe: " + iframeName);
            return;
        }

        var id = formContext.data.entity.getId().replace("{", "").replace("}", "");

        // Query SharePoint Documents Locations to find any records which are related to the 
        // record we are currently viewing
        var req = new XMLHttpRequest();
        req.open("GET",
            formContext.context.getClientUrl() +
            "/api/data/v8.0/sharepointdocumentlocations?$select=_regardingobjectid_value&$filter=_regardingobjectid_value eq " +
            id, true);
        req.setRequestHeader("OData-MaxVersion", "4.0");
        req.setRequestHeader("OData-Version", "4.0");
        req.setRequestHeader("Accept", "application/json");
        req.setRequestHeader("Content-Type", "application/json; charset=utf-8");
        req.setRequestHeader("Prefer", "odata.include-annotations=\"OData.Community.Display.V1.FormattedValue\"");
        req.onreadystatechange = function () {
            if (this.readyState === 4) {
                req.onreadystatechange = null;
                if (this.status === 200) {
                    var results = JSON.parse(this.response);
                    if (results.value.length > 0) {
                        // If we have any related items then show the tab and set the IFrame URL accordingly
                        tab.setVisible(true);
                        var url = formContext.context.getClientUrl() +
                            "/userdefined/areas.aspx?formid=ab44efca-df12-432e-a74a-83de61c3f3e9&inlineEdit=1&navItemName=Documents&oId=%7b" +
                            formContext.data.entity.getId().replace("{", "").replace("}", "") +
                            "%7d&oType=" +
                            formContext.context.getQueryStringParameters().etc +
                            "&pagemode=iframe&rof=true&security=852023&tabSet=areaSPDocuments&theme=Outlook15White";
                        iframe.setSrc(url);
                    } else {
                        tab.setVisible(false);
                    }

                } else {
                    alert(this.statusText);
                }
            }
        };
        req.send();
    }
}
```

## Finished Product

After the form & script have been updated, the documents sub- grid should appear on the form as below.

{%
  include figure
  image_path="/assets/images/posts/show-sharepoint-documents/DocumentsShown.png"
  alt="Documents showing in sub-grid on the Accounts Main Form"
  caption="Documents showing in sub-grid on the Accounts Main Form"
%}
