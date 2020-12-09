---
title: "Bulk Updating records in Dynamics CRM where a field is read-only"
last_modified_at: 2018-05-12T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
header:
  teaser: /assets/images/posts/bulk-update-read-only/0_teaser.png
classes: wide
---
I recently came across a issue where I had to bulk update some records in CRM, but one of the fields I needed to update had been marked as read only on the form. This meant that I was unable to map the information during the update because it was using the same rules as the form.

I had considered making the field editable and republishing the solution, however this felt like it wasn’t the best option as it would mean publishing the solution before and after each bulk update.

I did some research and found a great [tip of the day](https://crmtipoftheday.com/265/make-a-copy-of-forms-with-read-only-fields/){: target="_blank"} which shows a great solution for exactly the same problem.

The solution is:

* Copy/create a form for the entity you’re trying to update and make sure all the fields are editable
* Set the security role so that only System Admins can access the form
* Set the form to be the last in the form order
* Publish your changes

Once you’ve created the form the system admins will need to view a record in it first. This is because the bulk edit form will use the last form used. 

By setting the form security role to System Admins we will also prevent the users seeing the form. Meaning they can still be restricted to certain fields within the form. 