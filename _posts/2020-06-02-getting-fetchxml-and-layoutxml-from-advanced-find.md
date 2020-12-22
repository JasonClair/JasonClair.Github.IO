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

{%
  include figure
  image_path="/assets/images/posts/getting-fetch-xml/1_CreatingAdvancedFind.png"
  alt="New Advanced Find"
  caption="New Advanced Find"
%}

Then execute the Advanced Find and get the results.

{%
  include figure
  image_path="/assets/images/posts/getting-fetch-xml/2_RunningAdvancedFind.png"
  alt="Showing the results in Advanced Find"
  caption="Showing the results in Advanced Find"
%}

Once you’ve got the results page, open the Developer Tools (F12 in Chrome & Edge) and use the search functionality to look for fetchXml or layoutXml. You should find a div containing your strings.

{%
  include figure
  image_path="/assets/images/posts/getting-fetch-xml/3_GettingLayoutXml.png"
  alt="layoutXml and fetchXml showing in the DOM"
  caption="layoutXml and fetchXml showing in the DOM"
%}
