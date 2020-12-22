---
title: "Lookups – How to Disable ‘Recents’"
last_modified_at: 2020-07-08T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
header:
  teaser: /assets/images/posts/disable-recents/DisablingRecents.png
classes: wide
comments: true
---

In my previous [post](/dynamics/using-addCustomView-with-lookup-controls), I showed how to use Custom Views to use linked entities to filter items in a lookup. As mentioned in the notes, one of the issues with this is that the ‘Recents’ still show and are not filtered in the same way, therefore, whenever using this functionality I disable the ‘Recents’ from showing.

To do this, go to the Form Designer & open the field properties and tick the option for ‘Disable most recently used items for this field’ (at the time of writing I could only do this using the Classic Experience)

{%
  include figure
  image_path="/assets/images/posts/disable-recents/DisablingRecents.png"
  alt="Disable most recently used items for this field is checked"
  caption="Field property in Classic Experience"
%}
