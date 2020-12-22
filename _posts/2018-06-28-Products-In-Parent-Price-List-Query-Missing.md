---
title: "Dynamics 365 CRM – ‘Products in Parent Price List updated’ saved query missing"
last_modified_at: 2018-06-28T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Errors
header:
  teaser: /assets/images/posts/product-in-parent/0_teaser.png
classes: wide
---
One of my users recently encountered a strange error. When they tried to change an existing product on the Order Product, Quote Product or Sales Document form where the look up field had been set to show the search box, they received an error message. The details of the error message showed that a saved query was missing. When the search box was disabled the user no longer received the error message.

We did some investigation and found the issue was relating to the ‘Products in Price List updated’ view on the Product entity. I created a brand new trial and could not see the view either, but I could still use the search functionality.

I contacted Dynamics Support who were able to identify the problem and provided the steps below to resolve the issue. It looks like the view had been deleted and these steps recreate the view.

The steps to resolve the issue are:

1. Go to Advanced Find and select ‘Views’
1. Set Name Equals ‘Products in Parent Price List’
1. Open the view
1. Add a column to the view
1. Save the view
1. Publish customizations

Now if you try to change the product the issue should be resolved.
