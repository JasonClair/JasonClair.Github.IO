---
title: "SharePoint Calendar Colour Coded Entries (using Calendar Overlays)"
last_modified_at: 2017-08-30T18:00:00-00:00
categories:
  - SharePoint
tags:
  - Useful Tips
header:
  teaser: /assets/images/posts/sharepoint_coloured_calendar_overlay/6-coloured_calendar_with_overlay_duplication_fix.png
classes: wide
---
One of the features that I’ve found users like most about the SharePoint calendar is the ability to colour code entries. In some cases users have coloured the calendar dependent on who the appointment is for, in others, they have coloured based on the type of appointment, this blog post will show the latter scenario.
For this example, I will be using the out of the box calendar functionality and choosing the colour based on the category using calendar overlays.

{%
  include figure
  image_path="/assets/images/posts/sharepoint_coloured_calendar_overlay/1-coloured_calendar_before.png"
  alt="Calendar with no colouring added"
  caption="Calendar with no colouring added"
%}

## Views

A view must be selected when creating the overlays. The views are normal calendar views with the filters set however is required, in this example, any entries that are categorized as ‘Holiday’ will be coloured red, this means a view showing just Holidays is required.

{%
  include figure
  image_path="/assets/images/posts/sharepoint_coloured_calendar_overlay/2-coloured_calendar_creating_filter.png"
  alt="Creating the filter"
  caption="Creating the filter"
%}

## Overlay

From the calendar that you would like to add an overlay too, click on the Calendar ribbon and select ‘Calendar Overlays’. The next step is to click ‘New Calendar’, a page like below will be created. There is a limited choice of colours and a limitation of 10 overlays per calendar.

The Web URL should be filled in automatically but if not this should be the URL where the calendar is located. Clicking on resolve will then fill in the List and List View.

{%
  include figure
  image_path="/assets/images/posts/sharepoint_coloured_calendar_overlay/3-coloured_calendar_creating_overlay.png"
  alt="Creating the overlay"
  caption="Creating the overlay"
%}

{%
  include figure
  image_path="/assets/images/posts/sharepoint_coloured_calendar_overlay/4-coloured_calendar_with_overlay_preduplication_fix.png"
  alt="Calendar with overlay"
  caption="Calendar with overlay"
%}

## Final Steps

The calendar will now show red items for Holidays, but they will also be shown by default in the normal view. This means that you may want to change the default view to ignore holidays (and any other overlays that have been added).

{%
  include figure
  image_path="/assets/images/posts/sharepoint_coloured_calendar_overlay/5-coloured_calendar_creating_filter_deduplication.png"
  alt="Filtering to remove holidays from the default value"
  caption="Filtering to remove holidays from the default value"
%}

With the default view updated the calendar now shows one entry for a holiday.

{%
  include figure
  image_path="/assets/images/posts/sharepoint_coloured_calendar_overlay/6-coloured_calendar_with_overlay_duplication_fix.png"
  alt="Calendar with overlay and default view modified"
  caption="Calendar with overlay and default view modified"
%}
