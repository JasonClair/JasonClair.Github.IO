---
title: "Getting FetchXml and LayoutXml from Advanced Find"
last_modified_at: 2020-06-02T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
header:
  teaser: /assets/images/posts/getting-fetch-xml/3_GettingLayoutXml.png
classes: wide
comments: true
---

When working with custom views inside Dynamics 365 you may need to create fetchXml and layoutXml strings for new temporary custom views.

The easiest way to do this, in my opinion, is to create a new Advanced Find with the information & filters you require.

![New Advanced Find](/assets/images/posts/getting-fetch-xml/1_CreatingAdvancedFind.png)

Then execute the Advanced Find and get the results.

![Showing the results in Advanced Find](/assets/images/posts/getting-fetch-xml/2_RunningAdvancedFind.png)

Once you’ve got the results page, open the Developer Tools (F12 in Chrome & Edge) and use the search functionality to look for fetchXml or layoutXml. You should find a div containing your strings.

![layoutXml and fetchXml showing in the DOM](/assets/images/posts/getting-fetch-xml/3_GettingLayoutXml.png)
