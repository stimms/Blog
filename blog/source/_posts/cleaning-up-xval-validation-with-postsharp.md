---
layout: post
title: Cleaning Up xVal Validation With PostSharp
authorId: simon_timms
date: 2009-09-16
---

Even though the new ASP.net MVC 2.0 framework comes with built in [validation](http://haacked.com/archive/2009/10/01/asp.net-mvc-preview-2-released.aspx) it is useful to look at some alternatives. Afterall you don't want Microsoft telling you how to do everything, do you? One of the better validation frameworks is the xVal framework which just [went 1.0](http://blog.codeville.net/2009/09/17/xval-v10-now-available/). In this release there has been added support for a number of new features, probably the coolest of which is AJAX based validation for complex input.

However xVal does have one drawback, it is quite verbose to implement. In every method which alters data you will have to put something like

  ```csharp  
 var errors = DataAnnotationsValidationRunner.GetErrors(this).ToList();  
  
 if(errors.Any())  
 throw new RulesException(errors);
```

This is a bit repedative and kind of tiresome. Sure we could extract a method from this and just call that each and every time we edit data but that doesn't really solve the underlying problem of having code which is repeated.

Enter [PostSharp.](http://www.postsharp.org/)

PostSharp is an aspect oriented addon for .net languages. It acutally does most of its weaving in a post build step modifying the MSIL the compiler generates. We can extract the validation into an aspect.

  ```csharp
 [Serializable]  
 public class ValidateAttribute : OnMethodBoundaryAspect  
 {  
 public ValidateAttribute(){}  
  
 public override void OnEntry(MethodExecutionEventArgs eventArgs)  
 {  
 if (eventArgs.GetReadOnlyArgumentArray() != null && eventArgs.GetReadOnlyArgumentArray().Count() > 0)  
 {  
 var toValidate = eventArgs.GetReadOnlyArgumentArray()[0];  
 var errors = DataAnnotationsValidationRunner.GetErrors(toValidate);  
 if (errors.Any())  
 throw new RulesException(errors);  
 }  
 }  
 }
 ```

Here we create an OnEntry method which is called before the method we are intercepting. We skip any method with no arguments since it isn't likely to be updating data. Then we extract the argumetns and pass them into the validator for it to do its business.

Finaly we give the PostSharp framework a bit of information about where to use this aspect

AssemblyInfo.cs

```csharp
[assembly: CertificateSearch.Aspects.Validate(AttributeTargetTypes = "CertificateSearch.Models.*Repository")]
```
I have applied it to all the methods in files which end with Repository the Models namespace. That covers all the data modification methods in the project. I can now add new methods without the cost of ensuring that I validate each one.



