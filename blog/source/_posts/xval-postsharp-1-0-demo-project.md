---
layout: post
title: xVal PostSharp 1.0 Demo Project
authorId: simon_timms
date: 2009-10-07
---

After my last post I thought I would look at the demo project for xVal 1.0 and see if I could get it working with PostSharp. It was a little bit differnt in how it way set up from my projects but I figured it could still be improved with PostSharp. My first issue was that the method I was intercepting was in the entity itself rather than in a repository. This meant that there were methods in the entity which I didn't wish to intercept. There were two classes of those

1. Accessor methods â€“ we don't need to intercept getters
3. Internal methods â€“ ASP.net MVC uses reflection to examine the internals of the data classes in order to bind form results to them. We can avoid intercepting these by ignoring methods which start with â€˜.'

Next because the entity already contained all of the data it needed to persist the persistance method didn't have any arguments. In the previous post I assumed that this would always be the case. You know what they say about assuming: if you assume you make a jerk out of everybody in Venice. Pretty sure that is the saying.

This required and expansion of the current validator

  
public override void OnEntry(MethodExecutionEventArgs eventArgs)  
 {  
 if (IsAccessor(eventArgs) || IsInternal(eventArgs))  
 return;  
 if (HasArguments(eventArgs))  
 {  
 CheckPassedInformation(eventArgs);  
 }  
 else  
 {  
 CheckSelfContainedEntity(eventArgs);  
 }  
 base.OnEntry(eventArgs);  
 }  
  
 private static bool HasArguments(MethodExecutionEventArgs eventArgs)  
 {  
 return eventArgs.GetReadOnlyArgumentArray() != null && eventArgs.GetReadOnlyArgumentArray().Count() &rt; 0;  
 }  
  
 private static void CheckPassedInformation(MethodExecutionEventArgs eventArgs)  
 {  
 var toValidate = eventArgs.GetReadOnlyArgumentArray()[0];  
 var errors = DataAnnotationsValidationRunner.GetErrors(toValidate);  
 if (errors.Any())  
 throw new RulesException(errors);  
 }  
  
 private static void CheckSelfContainedEntity(MethodExecutionEventArgs eventArgs)  
 {  
 var toValidate = eventArgs.Instance;  
 var errors = DataAnnotationsValidationRunner.GetErrors(toValidate);  
 if (errors.Any())  
 throw new RulesException(errors);  
 }  
  
 public bool IsAccessor(MethodExecutionEventArgs eventArgs)  
 {  
 if (eventArgs.Method.Name.StartsWith("get_"))  
 return true;  
 return false;  
  
 }  
  
 public bool IsInternal(MethodExecutionEventArgs eventArgs)  
 {  
 if (eventArgs.Method.Name.StartsWith("."))  
 return true;  
 return false;  
  
 }

You can see I did a little bit of clean code refactoring in there to extract some methods. Now two different methods of saving information are checked.

The lesson here seems to be that the way in which you construct your data persistance layer has an effect on the construction of the validator such that there is no generic aspect which you can download and use.

You can download my modified xVal project [here](http://stimms.googlepages.com/PostSharp-xValDemo.zip). In order to get it running you'll need to have [PostSharp](http://www.postsharp.org/) installed but you're going to want it for lots of other stuff so get going on installing it.

Oh just one more note, when you're trying it out be sure to disable javascript so that the page actually posts back and doesn't validate using the javascript validation.



