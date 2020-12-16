---
title: "Dynamics 365 - Unit Testing Plugins"
last_modified_at: 2019-03-23T18:00:00-00:00
categories:
  - Dynamics
tags:
  - Unit Tests
header:
  teaser: /assets/images/posts/dynamics-unit-test/TestRunner.png
classes: wide
comments: true
---
I am an advocate of using unit testing wherever possible. This is because it allows repeatable tests to run, usually in a fraction of a second. Using unit tests allows me to be confident that later updates will not affect the current functionality.

One of the issues with unit testing I had with unit testing for Dynamics plugins & coded activities is that I did not want the unit test to run directly against the Dynamics instance. The two main reasons for this are:

1. I did not want to be performing actions against an instance because part of the reason the test could fail is if the record the test is expecting to be there has been deleted
1. I did not want to test the connection to the platform. The unit tests should just test the code, not the overall platform.

## Mocks

To prevent connecting to an actual instance, we can utilise Mocks.

> In a unit test, mock objects can simulate the behavior of complex, real objects and are therefore useful when a real object is impractical or impossible to incorporate into a unit test.
>
> - [https://en.wikipedia.org/wiki/Mock_object](https://en.wikipedia.org/wiki/Mock_object){:target="_blank"}

In this example, I will be using a Mocking Framework called FakeXrmEasy. This is a fantastic framework which allows most (if not all) of Dynamics functionality to be replicated. The website provides good examples of how to use it – but I’ll go through another example in this post.

## Test Scenario

The plugin that I will be using for this example is simple. It trigger on PreOperation when an Account is created. It will take the name & address1_city fields and concatenate them into another field called *new_nameandcity*.

The tests that I will run on this plugin are:

1. t should work when name and address1_city are provided
1. It should throw an exception when address1_city is not provided

## Installation

In my example, I have created a new Unit Test project (I am using MS Test, but this framework does work with others, like NUnit). Inside the Unit Test project, open [NuGet](https://www.nuget.org/){:target="_blank"} and search for FakeXrmEasy, then install the version that matches your target instance of Dynamics.

Alternatively, you can use the NuGet console to install directly from the command line – the names follow the pattern ‘FakeXrmEasy.version’, for exampleFakeXrmEasy.365.

## Plugin Modification

If you are using the Dynamics 365 Toolkit to generate the plugin structure, then you should see that the plugin constructor accepts secure & unsecure configuration as parameters.

For the unit test framework to be able to access the plugin, another constructor needs to be added which accepts no parameters. I thought that a parameterless constructor had to be added, but [Betim Beja](https://www.linkedin.com/in/betimbeja/){:target="_blank"} has pointed out that he has updated the framework to allow the plugin instance to be created & then passed it to ExecutePlugin.

In this example, I have also added a property which I use to determine if the plugin is being executed by the unit test runner or by the Dynamics system. I have done this to highlight that sometimes your code may need to be modified to run correctly in both environments. In my example, I am running on Create in PreOperation. If the code is executed within Dynamics, then the Update command cannot be called at the end otherwise an exception is shown regarding record not existing. However, if Update is not ran in the test, then the record is not Updated and the test fails.

## Code

The two unit tests are included below. A sample project has also been uploaded to [GitHub](https://github.com/JasonClair/blog_examples/tree/master/plugin_unit_test_examples){:target="_blank"}.

### Account Created Unit Test – Successful

``` csharp
        /// <summary>
        /// Test that when a new account is created the new_nameandcity field is updated with the values
        /// from the name & address1_city fields
        /// </summary>
        [TestMethod]
        public void Field_Updated_When_Account_Created()
        {
            // Create the fake context. This allows us to setup a mocked version of Dynamics for use later in the test
            XrmFakedContext context = new XrmFakedContext();

            // Generate a GUID which will be assigned to the fake record. It is done here to allow the record to be retrieved
            // later during the test
            Guid accountGuid = Guid.NewGuid();

            // Create the entity record which will be the target. In this case I have set the name & address1_city
            // field to have a value, and assigned the new_nameandcity field to have no value
            Entity account = new Entity("account")
            {
                Id = accountGuid,
                Attributes =
                {
                    ["name"] = "Acme",
                    ["address1_city"] = "America",
                    ["new_nameandcity"] = ""
                }
            };

            // This will setup the context with the entities created. This is the set of entities we will be able to access
            // during the test. 
            context.Initialize(new List<Entity> { account });

            // Execute the plugin. I have added the MessageName & stage as this plugin executes PreOperation
            context.ExecutePluginWithTarget<PreOperationaccountCreate>(account, "Create", 20);

            // Retrieve the update account from the fake context
            Entity updatedAccount = context.GetOrganizationService()
                .Retrieve("account", accountGuid, new ColumnSet(new string[] { "new_nameandcity" }));

            // Assert that the value meets our expected value
            Assert.AreEqual("Acme - America", updatedAccount.GetAttributeValue<string>("new_nameandcity"));
        }
```

### Account Created Unit Test – Exception Thrown

``` csharp
        /// <summary>
        /// The plugin should throw an exception if the city or name fields are not filled in
        /// </summary>
        [TestMethod]
        [ExpectedException(typeof(Exception))]
        public void Error_Thrown_If_City_Is_Null()
        {
            // Create the fake context. This allows us to setup a mocked version of Dynamics for use later in the test
            XrmFakedContext context = new XrmFakedContext();

            // Generate a GUID which will be assigned to the fake record. It is done here to allow the record to be retrieved
            // later during the test
            Guid accountGuid = Guid.NewGuid();

            // Create an account record which has no address1_city filled in. This will be used during the plugin execution
            Entity account = new Entity("account")
            {
                Id = accountGuid,
                Attributes =
                {
                    ["name"] = "Acme",
                    ["address1_city"] = "",
                    ["new_nameandcity"] = ""
                }
            };

            // This will setup the context with the entities created. This is the set of entities we will be able to access
            // during the test. 
            context.Initialize(new List<Entity> { account });

            // Execute the plugin. I have added the MessageName & stage as this plugin executes PreOperation
            context.ExecutePluginWithTarget<PreOperationaccountCreate>(account, "Create", 20);

            // I am not asserting anything here as the plugin only needs to throw an exception. If no exception is thrown
            // then the unit test would fail
        }
```

## Summary

Hopefully you can see how easy unit testing can be when using the FakeXrmEasy framework. Although unit testing can cause a bit more work upfront, it easily pays for itself through reduced bugs being introduced at later dates.

Unit testing will also allow more confidence when refactoring or adding new functionality as you can re-run all of the tests in seconds to make sure none of the previous functionality has changed.

It is important to remember that unit tests need to be maintained overtime. If the functionality is meant to change, then the unit test needs to be updated to include this. You may also forget to test certain scenarios, when this is noticed, the unit tests should be added again. The aim is to have every execution path covered.

## Further Functionality

In a previous [post](/dynamics/Dynamics-365-Visual-Studio-Team-Services-Build-And-Release-Automated-Solution-Deployment), I demonstrated using Azure DevOps (was previously named Visual Studio Team Services) pipelines to automated deployment of managed solutions using [Dynamics 365 Build Tools](https://marketplace.visualstudio.com/items?itemName=WaelHamze.xrm-ci-framework-build-tasks){:target="_blank"}. It is possible to integrate unit tests into the pipeline, to prevent deployment of solutions if the unit tests fail. I will cover how to do this in a later blog.
