---
title: "SharePoint Setting Ribbon Context using JavaScript"
last_modified_at: 2017-10-08T18:00:00-00:00
categories:
  - SharePoint
tags:
  - Useful Tips
  - JavaScript
header:
  teaser: /assets/images/posts/sharepoint_ribbon_context/1-Ribbon_NotFocusedOnDocumentLibrary.png
classes: wide
---
I recently had a user ask me to create a web part page which had a document library & content editor web part which described the purpose of the library & where to get more information should they need it. By doing this, the ribbon context was lost which meant that the users no longer received Files & Library tabs – they could get the tabs back by clicking in the library but not all users were aware of this.

![Ribbon without context set](/assets/images/posts/sharepoint_ribbon_context/1-Ribbon_NotFocusedOnDocumentLibrary.png)
Ribbon without the context set

![Ribbon with context set](/assets/images/posts/sharepoint_ribbon_context/2-Ribbon_FocusedOnDocumentLibrary.png)
Ribbon with the context set

Luckily, there’s a simple fix for this. Adding a script editor and using the script below will add an event that sends a dummy click to the element which will return the tabs as they should be. Please note, the ID of the element will need to be changed to the document library you want to add the ribbon context too.

```javascript
<script type='text/javascript'>
_spBodyOnLoadFunctionNames.push("loadRibbon");

function loadRibbon() {
    setTimeout(function () {
        var elem = document.getElementById("---DIV ID OF WEB PART--- (ie: MSOZoneCell_WebPartWPQ3)");
        if (elem != null) {
            var fakeEvent = new Array();
            fakeEvent["target"] = elem;
            fakeEvent["srcElement"] = elem;
            WpClick(fakeEvent);
            _ribbonStartInit("Ribbon.Browse", true)
        }
    }, 2000);
}
</script>
```