---
layout: post
title: "Breathe Life into Ageing Applications"
date: 2017-12-11
menu: review
categories: Architecture
published: true
---
As development consultants we often encounter very mature software applications that are now in need of modernisation.

Many of these are considered to be critical applications that deliver inherent business value and as such, are commonly large complex solutions.

## Does this sound familiar?
These applications are characterised by the following:
- Develpoed over a number of years by many different people
- Have incomplete, limited or out-of-date system documentation
- Complex, difficult and time consuming to enhance
- Difficult, time consuming and risky to deploy
- Tricky to support when issues arise in production.

## Ideals
In a perfect world we'd prefer these applications to:
- Be easy to work on when enhancements and / or fixes are required
- Be quick and straightforward to deploy to production in a safe, risk-free and repeatable way
- Be easy to support, troubleshoot and monitor once in production.

## So, what are the options?
The options for modernisation usually depend on factors such as the size and complexity of the application. Appetite for change and budget available will also play a part. Some options include:
- Total rewrite - useful for smaller applications or where the desire to change is high
- Gradual migration to something more manageable - where identifiable system components could be moved to a new architecture, bit-by-bit
- Break off functionality from the existing application to new services where suitable.

## Where to Start?
Deciding which option to proceed with can be a daunting prospect in itself. However, development consultants can help with this by conducting the following activities:
- Thorough analysis of the current system architecture, functionality and technology in use
- Establishing the functionality, availability and scalability that are expected of the application not just for today but also for tomorrow
- Observing and gaining an understanding of the current application deployment process
- Engaging with someone who has to support the application.

## Problem in Context
As mentioned above, we come across complex monolithic applications that are now so hard to manage that it's time to start thinking about their future and how they could be transformed.
    
Imagine that we have the following hypothetical application that:
- Is a monolithic web application 
- Is hosted on IIS
- Is business critical
- Has regular requests for new features and functionality
- Can only be deployed in it's entirety
- Runs (long-running, process intensive) background processes all day in the IIS process.
  
Embarking on a full rewrite at this stage would be too expensive, too risky and would take too long.

### Suggested Approach
Identify functionality that could be broken off into a lightweight, decoupled component (that could even be presented as a container image and deployed / scaled on demand). This can then be deployed independently and run as a service in a self-hosted process.
  
For example:
  
![](/assets/Component-Host.PNG)
  
Note that:
- The .NET Console Application leverages OWIN (via the Microsoft.AspNet.WebApi.OwinSelfHost NuGet package)
- The component is self-contained and independent
- Components can be deployed to a single host OR scaled out to multiple hosts (potentially leveraging cloud technologies).

#### Benefits
- A controlled low-risk method of transitioning to a new and more modern architecture
- Has many of the advantages of a microservices architecture and has a flexible hosting model e.g. can scale if required
- ASP.NET Web API provides an HTTP end point (API)
- Easy for developers to work on and add new functionality
- Easy and quick to deploy and can be done independently
- Runs outside of the IIS process, which is good for long running background tasks.

## Summary
This is just one way to modernise a complex monolithic application. However, there are many other approaches that could be taken and should be decided on a case by case basis.