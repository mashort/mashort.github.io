---
layout: post
title: "Feature Toggling for Pain Relief"
date: 2017-03-04
categories: Design
published: true
---
While working as consultants on an Enterprise level ASP.NET application, my colleagues and I became aware of a recurring pain-point that the in-house development team were suffering from.

The team are committed to a monthly application release schedule.

The developers work on application features by creating source code feature branches off from the mainline source code branch. However, some features take longer to develop than a single release cycle and this becomes problematic e.g. with merging and integrating long-lived feature branches into the mainline.

We thought that this was a good use case for feature toggling to provide some relief to this problem.

## So what exactly is Feature Toggling? ##
Feature toggling is a development design pattern that provides an alternative approach to maintaining multiple source code feature branches with the added benefit of being able to turn application features on or off at will.

In practice, *Work-In-Progress* features are wrapped in a conditional if...then statement in code and deployed to production in a *switched off* state, with the ability to switch the feature on at a later time without requiring re-deployment of the source code for that feature.

## Benefits ##
Feature toggles can provide many benefits including...
* Separation of feature rollout from deployment, supporting Continuous Delivery
* Reduced risk - by isolating it's effect on the system e.g. a feature toggle can be turned on or off at a moments notice, without the need for deployment
* Ability to iterate quicker – enabling more frequent and reliable releases
* Ability to roll out features to certain users or exclude groups of users from seeing a feature
* Useful for A / B testing

The concept of feature toggle-driven development has been around for a long time. In fact, the well respected software design & architecture author / speaker Martin Fowler is one of the people credited with introducing the concept - see his [Feature Toggle article][mf-feature-toggle].

Other good resources I found were [Feature Flags][feature-flags-io] and the excellent [Launch Darkly best practices guide][ld-best-practices].

## Categories and usage ##
There are two main categories of feature toggles:
* Release feature toggles
  * Simple on / off switches for getting *Work-In-Progress* features into the mainlilne code branch / Production in an initial disabled state
* Business feature toggles
  * Access Control Toggles – to control access of features to subsets of users
  * DevOps Toggles - support for debugging / maintenance mode
  * Experiment Toggles

Feature toggles can be used to control:
* User-visible features
* Code-only features

## Implementation Tools ##
There are many 3rd party libraries available to assist in implementing feature toggles in your application. 
I'd recommend using one of these rather than rolling your own.

We decided to use Jason Roberts excellent open source [FeatureToggle][jr-feature-toggle] library for the following reasons:
* Specifically for use with .NET
* No magic strings – strongly-typed objects are used. This makes it easy to remove toggles by just removing the toggle class and seeing where the build breaks - compile-time error discovery!
* Flexible Provider Model – so you can customize where you get the configured value from (if you don't want to use one of the built-in ones)

## Getting started ##
For this example, the following environment was used:
* Windows 10
* Visual Studio Enterpise 2015
* C#
* Microsoft SQL Server (localdb)

### Intstall FeatureToggle library via NuGet ###
In Visual Studio, install the open source FeatureToggle library via NuGet and add it to the projects where you want to use it...

![Installing FeatureToggle in NuGet](/assets/NuGet-Install-FeatureToggle.PNG)

Notice that a package dependency `FeatureToggle.Core` is also installed.

I like to create class library project in my solution for feature toggle related functionality...

![Feature Toggle class library in Visual Studio](/assets/Feature-Toggle-Class-Library-In-VS.PNG)

OK, so now we're ready to create a new feature toggle and use it in code. Lets start by using one of the built in toggles that come bundled with this package.

### Create a Feature Toggle class ###
Create a new class for each feature you want to toggle in your application. I like to create a container class to keep all of my Release feature toggles together, that I can then add my feature toggle classes to...

{% highlight C# linenos %}
namespace FeatureControl
{
    public class ReleaseFeatureToggles
    {
        public class MyNewFeature : SimpleFeatureToggle {}
    }
}
{% endhighlight %}

Note that our new feature toggle class needs to inherit from one of the FeatureToggle base classes (e.g. one of the bundled toggles) - in our example we're going to use the `SimpleFeatureToggle`.

### Configure the Feature Toggle value ###
In our example we're using an ASP.NET web application, so we need to set the on / off state for the new feature toggle in the web.config file...

{% highlight xml %}
<appSettings>
  <add key="FeatureToggle.MyNewFeature" value="true"/>
</appSettings>
{% endhighlight %}

Note that the first part of the key must be `FeatureToggle` and the second part of the key `MyNewFeature` must match the feature toggle class created for it.

### Using the Feature Toggle in your code ###
To use the feature toggle in your code it's just a case of creating an instance of the new feature toggle class and then using it's `FeatureEnabled` property in a conditional statement...

{% highlight C# linenos %}
var myNewFeature = new FeatureControl.ReleaseFeatureToggles.MyNewFeature();

if (myNewFeature.FeatureEnabled)
{
    // My new feature...
}
{% endhighlight %}

### What if i want the Feature Toggle to get it's value from a database, rather than a config file? ###
If you want the flexibility of storing and controlling the feature toggles on / off state from a database rather than from a config file, then fortunately there is a bundled toggle called `SqlFeatureToggle` that does just that and here's how you use it...

#### Create a database table for storing Feature Toggle states ####
We're going to use a Microsoft SQL Server database table to store our feature toggle states...

![Feature Toggles database table](/assets/Feature-Toggles-Database-Table.PNG)

And then add a row to this table for our new feature toggle class...

{% highlight sql %}
INSERT INTO FeatureToggles
	(FeatureToggleId,FeatureToggleName,FeatureToggleEnabled,DateAdded)
VALUES
	(1,'temp-AnotherNewFeature',1,GETDATE())
{% endhighlight %}

#### Create a Feature Toggle class that inherits from SqlFeatureToggle ####
This time we need to create a new class that inherits from the `SqlFeatureToggle` base class (which uses a SQL value provider)...

{% highlight C# linenos %}
namespace FeatureControl
{
    public class ReleaseFeatureToggles
    {
        public class MyNewFeature : SimpleFeatureToggle {}

        public class AnotherNewFeature : SqlFeatureToggle {}
    }
}
{% endhighlight %}

Our new class added on line 7.

#### Configure the SQL Feature Toggle value ####
This time we need to set the on / off state for the new feature toggle in the web.config file, in a slightly different way...

{% highlight xml linenos %}
<appSettings>
  <add key="FeatureToggle.MyNewFeature" value="true"/>
  <add key="FeatureToggle.AnotherNewFeature.ConnectionString" value="Data Source=(localdb)\ProjectsV12;Initial Catalog=MyRnD;Integrated Security=True"/>
  <add key="FeatureToggle.AnotherNewFeature.SqlStatement" value="SELECT FeatureToggleEnabled FROM dbo.FeatureToggles WHERE FeatureToggleName = 'temp-AnotherNewFeature'"/>
</appSettings>
{% endhighlight %}

Important points to note here are:
* A setting has to be added for the SQL Connection String
* A setting has to be added for the SQL query (note on line 4 that the setting value should contain the SQL query to retrieve a true / false value from the Feature Toggles database table)
* The Feature Toggle Name must match the value in the FeatureToggleName field in the database table or a runtime exception will occur (note that this is by design for this bundled toggle, it will never attempt to guess whether a toggle is enabled or not)

### Using this Feature Toggle in your code ###
To use this `SqlFeatureToggle` in your code is the same as with the `SimpleFeatureToggle` described earlier...

{% highlight C# linenos %}
var anotherNewFeature = new FeatureControl.ReleaseFeatureToggles.AnotherNewFeature();

if (anotherNewFeature.FeatureEnabled)
{
    // Another new feature...
}
{% endhighlight %}

Done, the feature toggle on / off value is now coming from a database!

### What other toggles come bundled with this library? ###
There are a number of useful toggles that come bundled with the FeatureToggle library...
* AlwaysOffFeatureToggle
* AlwaysOnFeatureToggle
* EnabledOnOrAfterDateFeatureToggle
* EnabledOnOrBeforeDateFeatureToggle
* EnabledBetweenDatesFeatureToggle
* SimpleFeatureToggle
* RandomFeatureToggle
* EnabledOnDaysOfWeekFeatureToggle
* SqlFeatureToggle
* EnabledOnOrAfterAssemblyVersionWhereToggleIsDefinedToggle

You can learn how to use these from the [FeatureToggle documentation][jr-feature-toggle-docs].

### Customising FeatureToggle ###
As we've seen, the toggles that come bundled with the FeatureToggle library use configuration files as part of the mechanism to get a toggles value.

If you don't want to add anything to your applications config files then the FeatureToggle library allows you to customise how the toggle value is provided - here's how...

#### Create a custom Feature Toggle provider class ####
We're going to create a custom SQL feature toggle provider class...

{% highlight C# linenos %}
public class CustomSqlFeatureToggleProvider : IBooleanToggleValueProvider
{
    public bool EvaluateBooleanToggleValue(IFeatureToggle toggle)
    {
        return GetFeatureToggleValue();
    }

    private bool GetFeatureToggleValue()
    {
        // Call data access method here to retrieve the Feature Toggle Enabled value from the database table
    }
}
{% endhighlight %}

Note that a custom toggle provider class must:
* Implement the `IBooleanToggleValueProvider` interface
* Implement the boolean method `EvaluateBooleanToggleValue(IFeatureToggle toggle)`

### Using a Feature Toggle with a custom toggle provider in your code ###
If we want to use a feature toggle in our code and make it use our custom feature toggle provider, then this can be done by assigning a new instance of our custom feature toggle provider class to the `ToggleValueProvider` property of our feature toggle instance, as seen in line 2 below...

{% highlight C# linenos %}
var anotherNewFeature = new FeatureControl.ReleaseFeatureToggles.AnotherNewFeature();
anotherNewFeature.ToggleValueProvider = new FeatureControl.CustomFeatureToggleProviders.CustomSqlFeatureToggleProvider();

if (anotherNewFeature.FeatureEnabled)
{
    // Another new feature...
}
{% endhighlight %}

## Summary ##
In this article we've had a general look at feature toggles, what they can be used for and how they can be implemented.

There are recommended best practices that should be considered and also Business feature toggles to be considered.

[mf-feature-toggle]: https://martinfowler.com/bliki/FeatureToggle.html
[feature-flags-io]: https://featureflags.io/feature-toggles
[ld-best-practices]: https://github.com/launchdarkly/featureflags
[jr-feature-toggle]: https://github.com/jason-roberts/FeatureToggle
[jr-feature-toggle-docs]: http://jason-roberts.github.io/FeatureToggle.Docs/pages/usage.html