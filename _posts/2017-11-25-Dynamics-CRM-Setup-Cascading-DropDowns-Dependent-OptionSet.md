---
title: "Dynamics CRM Cascading Drop Downs / Dependent Option Set"
last_modified_at: 2017-11-25T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
  - JavaScript
header:
  teaser: /assets/images/posts/cascading_drop_down/CDD_FirstImage.png
---
A user asked if it would be possible to recreate cascading drop down lists like they had in SalesForce. I thought this would be an out of the box configuration and agreed that they could have it. Surprisingly, this doesn’t seem to be something that is out of the box – but handily Microsoft have published an excellent tutorial on MSDN about how to do this using JavaScript.

The tutorial by Microsoft is very useful, but I wanted to create one in my own words with a few more images to hopefully help newer members of the Dynamics community. I’ve not included the code in this blog in case Microsoft update the [MSDN](https://msdn.microsoft.com/en-us/library/gg594433.aspx){:target="_blank"} article with newer versions of the code.

In this example, I will be making a cascading drop down between the out of the box businesstypecode field and a new custom field called *new_businessspecialisation*.

## JSON
The code Microsoft provided uses JSON to manage the parent child relationships, this works well & is easy to create, but might become time consuming if the field values change regularly. It may be worth investigating whether there is another way to do this that provides the user with a front end to manage the relationships, but that was beyond the scope of this work.

## Setup
The JSON file will need to be uploaded as a web resource (of type JScript). The code provided by Microsoft will also need to be uploaded as a web resource. 

![Adding the code as web resources](/assets/images/posts/cascading_drop_down/1_CDD_WebResource.png)
Adding the code as web resources

Once the web resources have been created you will need to define the Form On Load functionality, to this simply add the library, enter the function name and pass in the name of your JSON file into the parameters. The script provided by Microsoft this will then concatenate this into a URL to load the source from.

![Setting the onLoad event](/assets/images/posts/cascading_drop_down/1_CDD_WebResource.png)
Setting the Form OnLoad Event

Once the Form On Load event has been created you will then need to create the On Change event for the parent field. This time you will need to pass in the name of the parent field & the name of the child field. In the Microsoft example they do this another as they have an extra level.

![Setting the field OnChange event](/assets/images/posts/cascading_drop_down/3_CDD_OnChangeEvent.png)
Setting the field OnChange event

Once this has been done, everything should be good to go. The next couple of images show the functionality in action.

![Field locked because no parent Business Type has been chosen](/assets/images/posts/cascading_drop_down/4_CDD_NoValue.png)
Field locked because no parent Business Type has been chosen

![Metal & Plastics showing for Manufacturing](/assets/images/posts/cascading_drop_down/5_CDD_Manufacturing.png)
Metal & Plastics showing for Manufacturing

![Dynamics & SharePOint showing for Application Development](/assets/images/posts/cascading_drop_down/6_CDD_AppDev.png)

## Summary
Overall, thanks to the Microsoft sample code this proves to be a relatively simple piece of functionality to add that potentially provides a lot of value to the end user. I’m hopeful that this will be added into the system at some point.