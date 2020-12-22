---
title: "Dynamics 365 & Azure DevOps – Unit Testing"
last_modified_at: 2019-03-28T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Unit Tests
  - Azure DevOps
header:
  teaser: /assets/images/posts/dynamics-and-azure-devops/VisualStudioTestSettings.png
classes: wide
comments: true
---
In a previous [post](/dynamics/Dynamics-365-Visual-Studio-Team-Services-Build-And-Release-Automated-Solution-Deployment) I have shown how to use Azure DevOps (was Visual Studio Team Services) builds with [Dynamics 365 Build Tools](https://marketplace.visualstudio.com/items?itemName=WaelHamze.xrm-ci-framework-build-tasks){:target="_blank"} to export an un-managed solution as a managed solution and then install into into a different instance.

I have also then provided some guidance on how to use [FakeXrmEasy](https://dynamicsvalue.com/home){:target="_blank"} to create unit tests for plugins & coded activities in another [post](/dynamics/Dynamics-365-Unit-Testing-Plugins).

The next step is to prevent solutions being installed if the unit tests fail. This a simple but powerful addition to the build profile as it helps to further reduce the amount of bugs deployed to production. It can also be used to identify issues before development code is moved to test instances.

I will assume that the previous [post](/dynamics/Dynamics-365-Visual-Studio-Team-Services-Build-And-Release-Automated-Solution-Deployment) about using Azure DevOps has been read and understood.

The first step is to ensure that the unit test assembly (.dll) files are in the source control. Without these, the test runner will be unable to run the tests.

Adding the unit test step is as easy as editing the build & adding the [Visual Studio Test](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/test/vstest?view=azure-devops){:target="_blank"} step at the beginning of the agent and then configuring the Test files field to include the search pattern that would find the test assembly.

{%
  include figure
  image_path="/assets/images/posts/dynamics-and-azure-devops/VisualStudioTestSettings.png"
  alt="Example configuration fo the Visual Studio Test step - in my example the unit test project compiles to UnitTests.dll"
  caption="Example configuration fo the Visual Studio Test step - in my example the unit test project compiles to UnitTests.dll"
%}

After the step has been added, any future runs of the build will first check to make sure the unit tests pass. If they do not, then the build fails and subsequently the release is not triggered, so the solutions are not installed on the target instance.

I hope you will agree that this is a quick & easy addition to the build profile which will allow even more confidence that plugins & coded activities inside solutions are bug free.
