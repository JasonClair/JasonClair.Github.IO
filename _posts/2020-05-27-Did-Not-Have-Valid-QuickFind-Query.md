---
title: "Dynamics 365 Solution won’t uninstall"
last_modified_at: 2020-05-27T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Error
header:
  teaser: /assets/images/posts/quick-find-error/Settings.png
classes: wide
comments: true
---

Whilst uninstalling a managed solution I received an error message stating that one of my entities “did not have a valid QuickFind query”. I checked the entity and there definitely was a valid QuickFind view.

It turns out that the solution could not be uninstalled as the entity was being used in the Categorized Search functionality. After removing the entity from the Categorized Search I was able to uninstall the solution correctly.

To remove the entities from the Categorized Search, go to Settings -> Administration -> System Settings and then under the General tab scroll down until the you see the Select button for “Select entities for Categorized Search”

{%
  include figure
  image_path="/assets/images/posts/quick-find-error/Settings.png"
  alt="Setting within System Settings"
  caption="Setting within System Settings"
%}
