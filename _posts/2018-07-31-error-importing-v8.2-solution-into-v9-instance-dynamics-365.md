---
title: "Dynamics 365 CRM – ‘Products in Parent Price List updated’ saved query missing"
last_modified_at: 2018-07-31T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Errors
header:
  teaser: /assets/images/posts/error-importing-8.2-solution/0_teaser.png
classes: wide
---

Whilst importing a Dynamics CRM v8.2 managed solution into Dynamics CRM v9 I encountered the following error. Searching the internet suggests that the error is mainly thrown when trying to install ADX Studio, I didn’t see any examples that mentioned solution imports from v8.2 to v9. The error message thrown is:

>“The evaluation of the current component(name=Entity, id=efd3a52d-04ca-4d36-a54c-2a26a64f5571) in the current operation (Update) failed during managed property evaluation of condition: Managed Property Name: canmodifymobileclientreadonly; Component Name: Entity; Attribute Name: canmodifymobileclientreadonly;”.

The import log pointed out that this was against the Marketing List entity. I had a look around the solutions but couldn’t find anything that would have modified this value. The easiest way to resolve this issue is to:

* Extract the Managed Solution
* Open Customizations.xml
* Search for the entity name
* Find the values for the fields below and change from ‘0’ to ‘1’
* * IsVisibleInMobile
* * IsVisibleInMobileClient
* * IsReadOnlyInMobileClient
* Rezip the solution and import as normal

![XML to Modify](/assets/images/posts/error-importing-8.2-solution/0_teaser.png)

I was curious to see if the v8.2 solution could be updated so that the values were set to ‘1’ so that this step didn’t need to be done every time 

I imported into the v9 solution, however when attempting to do this, the same error message is thrown as above. Based on this, I believe the value must change from the v8.2 to v9 version.