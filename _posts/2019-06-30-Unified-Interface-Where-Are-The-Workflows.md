---
title: "Unified Interface – Where are the On-Demand Workflows?"
last_modified_at: 2019-06-30T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Unified Interface
  - Workflows
header:
  teaser: /assets/images/posts/unified-interface-workflows/FlowMenu.png
classes: wide
comments: true
---

My customers have got a lot of on-demand workflows that have been created over the years. They are used to being able to go “Run Workflows” to start a new on-demand workflow.

Some of these customers had decided that they wanted to do more training on Microsoft Flow before releasing it to the users, therefore, they asked me to remove the Flow options from Dynamics. This was done by going to Administration -> System Settings -> Customization and disabling ‘Enable Microsoft Flow’ and by removing the ‘Run Flows’ permissions from the customization tab of security roles.

Whilst looking at upgrading some of the environments to Unified Interface one of the first things that got asked was “How do I start an on-demand workflow?”. The answer? Under the “Flow” tab that had been hidden by the settings previously mentioned! Enabling Microsoft Flow & re-adding the ‘Run Flows’ permissions allows the Flow option to appear and under there the on-demand workflows appear too.

![Run Workflow now appears under the Flow menu](/assets/images/posts/unified-interface-workflows/FlowMenu.png)
