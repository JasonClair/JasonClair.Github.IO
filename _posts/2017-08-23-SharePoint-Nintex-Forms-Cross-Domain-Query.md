---
title: "SharePoint / Nintex Forms Cross Domain Query"
last_modified_at: 2017-08-23T18:00:00-00:00
categories:
  - SharePoint
tags:
  - JavaScript
  - Nintex
header:
classes: wide
---
A requirement came in from a customer asking if it would be possible to use information from 'List A' and use it as validation on another form. At first I tried to directly access the list using JavaScript, but this is prevented due to Cross Domain Scripting issues, instead, I needed to the SP.RequestExecutor functionality. This post will go through a simple of example of querying a list for an item and displaying it in an alert.

Firstly, the list to query has to be set up - for this example I created a simple custom list and used the out of the box title column. Image 1 shows the list that was created and used in this example.

![List being accessed in query](/assets/images/posts/nintex_cross_domain_query/1-query-list.png)

One of the first things that need to be done is to gather the SPHostUrl & SPAppWebUrl parameters from the query string, these will be used later when the executor is called.

In this example, the query is done as soon as the page has finished loading, whilst it may have been possible to directly use jQuery ready this seemed to cause intermittent issues where the query would run before Nintex had finished initializing so instead the NWF RegisterAfterReady functionality was used. Once Nintex has finished loading there is another query to get the SP.RequestExecutor script.

```javascript
var hostedWebUrl = decodeURIComponent(getQueryStringParameter("SPHostUrl"));
var appWebUrl = decodeURIComponent(getQueryStringParameter("SPAppWebUrl"));
var scriptBase = hostedWebUrl + "/_layouts/15/";

NWF.FormFiller.Events.RegisterAfterReady(function () {
        NWF$.getScript(scriptBase + "SP.RequestExecutor.js", queryList);
});

/* Utility function to get host and app URL from the query string */
function getQueryStringParameter(paramToRetrieve) {
    var params = document.URL.split("?")[1].split("&");
    var strParams = "";
    for (var i = 0; i < params.length; i = i + 1) {
        var singleParam = params[i].split("=");
        if (singleParam[0] == paramToRetrieve)
            return singleParam[1];
    }
}
```

The list query is built up using the standard REST APIs. In this example, the query is selecting the Title field for items where the ID is 2. This is obviously a simple example but can be used as a base. The $expand functionality is supported as well.

```javascript
function queryList() {
    var executor = new SP.RequestExecutor(appWebUrl);
    var fields = "&$select=Title";
    var filter = "&$filter=ID eq 2" ;
    var url = appWebUrl + "/_api/SP.AppContextSite(@target)/web/lists/getByTitle('Query List')/items?@target='" + hostedWebUrl + "'" + filter;
    
    executor.executeAsync({
        url: url,
        method: "GET",
        headers: { "accept": "application/json; odata=verbose" },
        success: querySuccessHandler,
        error: queryErrorHandler
    });
}
```

The results are returned in JSON format which allows easy navigation through the objects. The example below only expects a single result, but .each was used as an example.

```javascript
function querySuccessHandler(data) {
    var jsonObject = JSON.parse(data.body).d;
    
    NWF$(jsonObject.results).each(function () {
        alert(this.Title);
    });
}

function queryErrorHandler(data, errorCode, errorMessage) {
    // Handle error
}
```

Below shows the output from the page. Now that the information has been retrieved it can be used in a custom validation event.

![Resulting alert from the page](/assets/images/posts/nintex_cross_domain_query/2-test-message.png)