---
layout: post
title: Using Two Factor Identity in ASP.net 5
authorId: simon_timms
date: 2015-02-23
---
First off a disclaimer: 

```
ASP.net 5 is in beta form and I know for a fact that some of the identity related stuff is going to change next release. I know this because the identity code in [git](https://github.com/aspnet/Identity) is different from what's in the latest build of ASP.net that comes with Visual Studio 2015 CTP 5. So this tutorial will stop working pretty quickly. 

Update: yep, 3 hours after I posted this the next release came out and broke everything. Check the bottom of the article for an update.
```

With that disclaimer in place let's get started. This tutorial supposes you have some knowledge of how multi-factor authentication works. If not then lifehacker have a [decent introduction](http://lifehacker.com/5938565/heres-everywhere-you-should-enable-two-factor-authentication-right-now) or for a more exhaustive examination the [Wikipedia page]( http://en.wikipedia.org/wiki/Multi-factor_authentication).

If we start with a new ASP.net 5 project in Visual Studio 2015 and select the starter template then we get some basic authentication functionality built in. 

![Starter project](http://i.imgur.com/Y9dVcDU.png)

Let's start, appropriately, in the Startup.cs file. Here we're going to enable two factor at a global level by adding the default token providers to the identity registration: 

```
services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
```

The default token providers are an SMS token provider to send messages to people's phones and an E-mail token provider to send messages to people's e-mail. If you only want one of these two mechanisms then you can register just one with 

```
.AddTokenProvider(typeof(PhoneNumberTokenProvider<>).MakeGenericType(UserType))
.AddTokenProvider(typeof(EmailTokenProvider<>).MakeGenericType(UserType))
```

Next we need to enable two factor authentication on individual users. If you want this for all users then this can be enabled by setting User.TwoFactoreEnabled during registration in the AccountController. 

```
[HttpPost]
        [AllowAnonymous]
        [ValidateAntiForgeryToken]
        public async Task<IActionResult> Register(RegisterViewModel model)
        {
            if (ModelState.IsValid)
            {
                var user = new ApplicationUser
                {
                    UserName = model.UserName,
                    Email = model.Email,
                    CompanyName = model.CompanyName,
                    TwoFactorEnabled = true,
                    EmailConfirmed = true
                };
                var result = await UserManager.CreateAsync(user, model.Password);
                if (result.Succeeded)
                {
                    await SignInManager.SignInAsync(user, isPersistent: false);
                    return RedirectToAction("Index", "Home");
                }
                else
                {
                    AddErrors(result);
                }
            }

            // If we got this far, something failed, redisplay form
            return View(model);
        }
```

I also set EMailConfirmed here, although I really should make users confirm it via an e-mail. This is required to allow the EMailTokenProvider to generate tokens for a user. There is a similar field called PhoneNumberConfirmed for sending SMS messages. 

Also in the account controller we'll have to update the Login method to handle situations where the signin response is "RequiresVerification"

````
 switch (signInStatus)
 {
	 case SignInStatus.Success:
		 return RedirectToLocal(returnUrl);
	 case SignInStatus.RequiresVerification:
		 return RedirectToAction("SendCode", returnUrl);
	 case SignInStatus.Failure:
	 default:
		 ModelState.AddModelError("", "Invalid username or password.");
		 return View(model);
 }
```

This implies that there are going to be a couple of new actions on our controller. We'll need one to render a form for users to enter the code from their e-mail and another one to accept that back and finish the login process. 

We can start with the SendCode action

```
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> SendCode(string returnUrl = null)
{
    var user = await SignInManager.GetTwoFactorAuthenticationUserAsync();
    if(user == null)
    {
        return RedirectToAction("Login", new { returnUrl = returnUrl });
    }
    var userFactors = await UserManager.GetValidTwoFactorProvidersAsync(user);
    if (userFactors.Contains(TOKEN_PROVIDER_NAME))
    {
        if(await SignInManager.SendTwoFactorCodeAsync(TOKEN_PROVIDER_NAME))
        {
            return RedirectToAction("VerifyCode", new { provider = TOKEN_PROVIDER_NAME, returnUrl = returnUrl });
        }
    }
    return RedirectToAction("Login", new { returnUrl = returnUrl });
}
```

I've taken a super rudimentary approach to dealing with errors here, just sending users back to the login page. A real solution would have to be more robust. I've also hard coded the name of the token provider (it is "Email"). I'm only allowing one token provider but I thought I would show the code to select one. You can render a view that shows a list from which users can select. 

The key observation here is the sending of the two factor code. That is what sends the e-mail to the user.

Next we render the form into which users can enter their code:

```
[HttpGet]
[AllowAnonymous]
public IActionResult VerifyCode(string provider, string returnUrl = null)
{
    return View(new VerifyCodeModel{ Provider = provider, ReturnUrl = returnUrl });
}
```

The view here is a simple form with a text box into which users can paste their code

![Entering a code](http://i.imgur.com/6Latkck.png)

The final action we need to add is the one that receives the post back from this form

```
[HttpPost]
[AllowAnonymous]
public async Task<IActionResult> VerifyCode(VerifyCodeModel model)
{
    if(!ModelState.IsValid)
    {
        return View(model);
    }

    var result = await SignInManager.TwoFactorSignInAsync(model.Provider, model.Code, false, false);
    switch (result)
    {
        case SignInStatus.Success:
            return RedirectToLocal(model.ReturnUrl);
        default:
            ModelState.AddModelError("", "Invalid code");
            return View(model);
    }
}
```

Again you should handle errors better than me, but it gives you an idea.

The final component is to hook up an class to send the e-mail. In my case this was as simple as using SmtpClient.

```
using System;
using System.Net;
using System.Net.Mail;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNet.Identity;
using Microsoft.Framework.ConfigurationModel;

namespace IdentityTest
{
    public class EMailMessageProvider : IIdentityMessageProvider
    {

        private readonly IConfiguration _configuration;
        public EMailMessageProvider(IConfiguration configuration)
        {
            _configuration = configuration;
        }

        public string Name
        {
            get
            {
                return "Email";
            }
        }

        public async Task SendAsync(IdentityMessage identityMessage, CancellationToken cancellationToken = default(CancellationToken))
        {
            var message = new MailMessage
            {
                From = new MailAddress(_configuration.Get("MailSettings:From")),
                Body = identityMessage.Body,
                Subject = "Portal Login Code"
            };
            message.To.Add(identityMessage.Destination);

            var client = new SmtpClient(_configuration.Get("MailSettings:Server"));
            client.Credentials = new NetworkCredential(_configuration.Get("MailSettings:UserName"), _configuration.Get("Password"));
            await client.SendMailAsync(message);
        }
    }
}
```

This provider will need to be registered in the StartUp.cs so the full identity registration looks like:

```
services.AddIdentity<ApplicationUser, IdentityRole>(Configuration)
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders()
                .AddMessageProvider<EMailMessageProvider>();
```

You should now be able to log people in using multifactor authentication just like the big companies. If you're interested in using SMS messages to verify people both [Tropo](https://www.tropo.com/) and [Twilio](https://www.twilio.com/) provide awesome phone system integration options.

<h3>Update</h3>

Sure enough, as I predicted in the disclaimer, 3 hours after I posted this my install of VS2015 CTP 6 finished and all my code was broken. The fixes weren't too bad though: 

* The Authorize attribute moved and is now in Microsoft.AspNet.Security.
* The return type from TwoFactorSignInAsync and PasswordSignInAsync have changed to a SignInResult. This changes the code for the Login and VerifyCode actions

```
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
        var signInResult = await SignInManager.PasswordSignInAsync(model.UserName, model.Password, model.RememberMe, shouldLockout: false);
        if (signInResult.Succeeded)
            return Redirect(returnUrl);
        if (signInResult.RequiresTwoFactor)
            return RedirectToAction("SendCode", returnUrl);
    }
    ModelState.AddModelError("", "Invalid username or password.");
    return View(model);
}
```

```
[HttpPost]
[AllowAnonymous]
public async Task<IActionResult> VerifyCode(VerifyCodeModel model)
{
    if (!ModelState.IsValid)
    {
        return View(model);
    }

    var signInResult = await SignInManager.TwoFactorSignInAsync(model.Provider, model.Code, false, false);
    if (signInResult.Succeeded)
        return RedirectToLocal(model.ReturnUrl);
    ModelState.AddModelError("", "Invalid code");
    return View(model);
}
```

* EF's model builder styntax changed to no longer have Int() and String() extension methods. I think that's a mistake but that's not the point. It can be fixed by deleting and regenerating the migrations using:

    k ef migration add initial
    
You may need to specify the connection string in the Startup.cs as is explained here: http://stackoverflow.com/questions/27677834/a-relational-store-has-been-configured-without-specifying-either-the-dbconnectio
