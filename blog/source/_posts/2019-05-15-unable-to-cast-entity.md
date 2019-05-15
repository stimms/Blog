---
layout: post
title: Unable to cast object of type Microsoft.XrmSdk.Entity
authorId: simon_timms
date: 2019-05-15 19:00

---

You'll likely encounter early bound types when developing workflows, plugins or anything else which calls the Dynamics 365 SDK. These are basically generated classes which come from Dynamics. You can generate them using the [CrmSvcUtil.exe](https://docs.microsoft.com/en-us/dynamics365/customer-engagement/developer/org-service/create-early-bound-entity-classes-code-generation-tool). If you've ever done anything with T4 templates you can think of them as that or a really poor substitute for F# type providers.

<!--more-->

One of the nice things about these is that in conjunction with the `CreateQuery` method on the `OrganizationServiceContext` you can write LINQ queries instead of fumbling about with `QueryExpression`. 

```
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Sdk.Query;
...
//context is an OrganizationServiceContext
var logo = context.CreateQuery<WebResource>().Where(x=>x.Name == "a_cool_logo.png").Single();
```

However if you do this you'll likely run into the error 

```
System.InvalidCastException: Unable to cast object of type 'Microsoft.Xrm.Sdk.Entity' to type 'WebResource'
```

The problem here is that the SDK doesn't by default understand these classes which extend Entity. The fix is pretty simple. When creating the `OrganizationServiceProxy` add a `ProxyTypesBehavior` to the endpoint behaviours 

```
using Microsoft.Xrm.Sdk;
using Microsoft.Xrm.Sdk.Query;
...
var organizationService = new OrganzationServiceProxy(new Uri(uri), null, clientCredentials, null);
organizationService.ServiceConfiguration.CurrentServiceEndpoint.EndpointBehaviors.Add(new ProxyTypesBehavior());
```