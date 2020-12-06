---
title: "Dynamics CRM 2016 - Creating a record in a custom entity using JavaScript & REST"
last_modified_at: 2017-11-4T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
  - JavaScript
  - REST
header:
  teaser: /assets/images/posts/CRM_REST_javascript/1_CRMRestBuilder.png
---
A requirement came up where the sales managers wanted to be able to see who had viewed an opportunity in Dynamics CRM. At first, I looked at using the auditing functionality, but I could only see examples of this working with creating, updating & deleting records - please comment if I've missed something. So with this in mind, I looked at other alternatives.

The method I decided to go for was to create a record every time the form was opened using JavaScript and REST APIs.

For this example I created a new entity to store the records. The entity is called 'new_opportunityviewhistory' and I will be using the field 'new_name' to store information.

It is possible to create the REST query yourself, but why re-invent the wheel when an awesome tool such as [CRM REST Builder](https://github.com/jlattimer/CRMRESTBuilder){:target="_blank"} has been created. Please read the official documentation for installation information and how to use the product, I will only be covering how to create the query that will be used in this example.

![CRM REST Builder Window](/assets/images/posts/CRM_REST_javascript/1_CRMRestBuilder.png)

The above setup will give us the query required to create a new record, this can then be placed into a function that can be called on page load.

```javascript
function CreateHistoryRecord(){
    var entity = {};
    entity.new_name = "Viewed at " + new Date().toUTCString();

    var req = new XMLHttpRequest();
    req.open("POST", Xrm.Page.context.getClientUrl() + "/api/data/v8.2/new_opportunityviewhistories", true);
    req.setRequestHeader("OData-MaxVersion", "4.0");
    req.setRequestHeader("OData-Version", "4.0");
    req.setRequestHeader("Accept", "application/json");
    req.setRequestHeader("Content-Type", "application/json; charset=utf-8");
    req.onreadystatechange = function() {
        if (this.readyState === 4) {
            req.onreadystatechange = null;
            if (this.status === 204) {
                var uri = this.getResponseHeader("OData-EntityId");
                var regExp = /\(([^)]+)\)/;
                var matches = regExp.exec(uri);
                var newEntityId = matches[1];
            } else {
                Xrm.Utility.alertDialog(this.statusText);
            }
        }
    };
    req.send(JSON.stringify(entity));
}
```

This script can then be put into a web resource to make it available to the form.

![Web Resource setup](/assets/images/posts/CRM_REST_javascript/2_CRM_WebResource.png)
Dynamics CRM Web Resource

Once the web resource has been created the form can then be updated to use the function on page load. This will then create the record and show that the user has viewed a record.

![Form Editor window](/assets/images/posts/CRM_REST_javascript/3_CRM_FormEditor.png)
Adding JavaScript and Event Handler to the Opportunity form

Now every time the opportunity form is opened a new record will be created in the opportunity view history entity, allowing us to see who has been viewing opportunities. Further improvements would include either a link to the original record or some way of identifying the record.

![Results from Advanced Find](/assets/images/posts/CRM_REST_javascript/4_CRM_AdvancedFind.png)
Advanced Find showing 3 records created
