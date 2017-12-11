---
layout: post
title: "Application Modernisation"
date: 2017-12-11
menu: review
categories: Architecture
published: true
---
In our role as development consultants something that we come across quite frequently, while out in the field, are very mature software applications that are now in need of modernisation.

Many of these are considered to be critical applications that deliver inherent value to the business and as such, are commonly large complex solutions.

## Does this sound familiar?
Typically, these applications are characterised by the following:
- Develpoed over a number of years by many different people (who may not be there now)
- Limited, incomplete or out-of-date supporting system documentation
- Complex, difficult and time consuming to enhance or add new features
- Difficult, time consuming and risky to deploy (often has to be done out-of-hours)
- Tricky to support when issues arise in production.

## Ideals
In a perfect world we'd prefer these systems to:
- Be easy to work on when enhancements (or fixes) are required
- Be quick and straightforward to deploy to production in a safe, risk-free and repeatable way
- Be easy to support, troubleshoot and monitor once in production.

## So, what are the options?
Typically, the options for modernisation will largely depend on factors such as the size and complexity of the application. Appetite for change and budget available will also play a part. Some options include:
- Total rewrite - useful for smaller applications or where desire / need to change is high
- Gradual migration to something more manageable - where identifiable system components could be moved to a new architecture, bit-by-bit
- Break off functionality from the existing application to new services where suitable.

## Where to Start?
Of course, trying to decide which option to proceed with can be a daunting prospect in itslef for businesses. However, development consultants can help with this by conducting the following activities:
- Thorough analysis of the current system architecture, functionality and technology used
- Establishing the functionality, availibilty and scalbility that are expected of the application not just for today but also for tomorrow
- Observing the current application deployment process
- Engaging with someone who has to support the application.

## Problem in Context
As mentioned above, we've come across complex monolithic applications that are now so hard to manage that it's time to start thinking about tomorrow and how they could be transformed.
    
Imagine that we have the following hypothetical application that:
- Is a monolithic web application 
- Is hosted on IIS
- Is business critical
- New features / functionality have to be added to on a regular basis
- Can only be deployed in it's entirety (which is difficult)
- Runs background processes (day & night)
  
Embarking on a full rewrite at this stage would be too expensive, risky and may not be the right thing to do.

### Potential Approach
Identify functionality that could be broken off into a lightweight, decoupled component (that could even be presented as a container image and deployed / scaled on demand). This could then be deployed (independently) and run as it's own service on a host.
  
For example:
  
![](/assets/Component-Host.PNG)
  
Note that:
- This is a regular .NET Console Application (with the Microsoft.AspNet.WebApi.OwinSelfHost NuGet package installed)
- ASP.NET Web API provides an HTTP end point (that could be consumed by other clients)
- The component is self-contained and independent
- Components can be deployed to a single host OR scaled out to multiple hosts (potentially leveraging cloud technologies)

#### Benefits
- TODO