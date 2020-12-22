---
title: "Dynamic Grouping using SQL Report Builder"
last_modified_at: 2017-05-20T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Useful Tips
  - SSRS
header:
  teaser: /assets/images/posts/ssrs_dynamic_grouping/0_teaser.png
classes: wide
---
One of the requirements that came in recently was for some reports to be created, upon investigation the reports were the same but grouped differently. Rather than creating individual reports, I wondered if there was a way to allow the user to select the grouping method within the report itself, which would mean less duplication of reports.

This post will go through a simple report which will allow the user to select whether to group some data on the Make, Model or Year of a car. The post will not cover how to create a group or create a report as it is assumed this is already known.

## Data

The data used for this example is below, this has been stored in SQL and the data set query was SELECT * FROM Cars

{%
  include figure
  image_path="/assets/images/posts/ssrs_dynamic_grouping/1-ssrs-dynamic-grouping-table.png"
  alt="DataSet being used"
  caption="DataSet being used"
%}

## Parameters

The first step to allow the user to select the grouping method is to create a parameter for them to select from. Below shows the parameters used for this report, the label can be anything, but the value should be the column name. In this example, the parameter has been called GroupBy.

{%
  include figure
  image_path="/assets/images/posts/ssrs_dynamic_grouping/2-ssrs-dynamic-grouping-params.png"
  alt="Parameters set in SSRS"
  caption="Parameters set in SSRS"
%}

The selected parameter value can then be used in the grouping & name expression. This works as the value of the parameter will be used to find the field value. The grouping expression below can then be used.

`=Fields(Parameters!GroupBy.Value).Value`

The design of the report looks the same as any other grouping, other than the value being calculated by an expression. The header for the grouping column could be set by using the GroupBy value if needed.

{%
  include figure
  image_path="/assets/images/posts/ssrs_dynamic_grouping/3-ssrs-dynamic-grouping-designer.png"
  alt="Report in design mode"
  caption="Report in design mode"
%}

## Results

The below images shows the report running with two different grouping methods selected. As you can see, this is a simple but effective method of allowing the user to group the information in a way that they want to see.

{%
  include figure
  image_path="/assets/images/posts/ssrs_dynamic_grouping/4-ssrs-dynamic-grouping-grouped-make.png"
  alt="Report grouped by Make"
  caption="Report grouped by Make"
%}

{%
  include figure
  image_path="/assets/images/posts/ssrs_dynamic_grouping/5-ssrs-dynamic-grouping-grouped-year.png"
  alt="Report grouped by Year"
  caption="Report grouped by Year"
%}
