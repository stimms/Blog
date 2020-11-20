---
layout: post
title: Resending an SMS verification code in Okta from nodejs
authorId: simon_timms
date: 2020-11-20 14:00

---

The title of this blog seems like a really long one but I suppose that's fine because it does address a very specific problem I had. I had a need to resend an SMS to somebody running through an Okta account creation in our mobile application. We use a nodejs (I know, I'm not happy about it either) backend on lambda (and the hits keep coming). We don't take people through the external, browser based login because we wanted the operation to be seamless inside the app. 

The verification screen looks a little this (in dark mode)

![okta-resend-sms-nodejs](/images/okta-resend-sms-nodejs/signup.png)

<!--more-->

Problem is that the okta nodejs is weirdly incomplete. Some methods are bound and some aren't. I say it's weird because the whole API is just generated. Why wouldn't you generate the whole API? Perhaps the open API docs on which it is based are incomplete. Regardless there is no easy way to call the [resend endpoint](https://developer.okta.com/docs/reference/api/factors/#resend-sms-as-part-of-enrollment). What we can do, however, it lean on the exposed http property inside of the API. It will add appropriate authentication headers so you can just call the resend endpoint explicitly. 

```typescript
public async resendOTPSMS(oktaUserId: string, smsFactorId: string, phoneNumber: string) {
    const config = (await new ConfigService().getConfig());
    const oktaClient = this.getOktaClient(config);
    let url = `${config.OKTA_URL}/api/v1/users/${oktaUserId}/factors/${smsFactorId}/resend`;
    let body = {
      factorType: 'sms',
      provider: 'OKTA',
      profile: {
        phoneNumber: phoneNumber
      }
    };
    const request = {
      method: 'post',
      headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json',
      }, 
      body: body
    };
    await oktaClient.http.http(url, request);
  }
```

As you can see here we grab the okta client then use http.http explicitly to send the body and request. This can be done for other endpoints which are also shunned by the API. It is a bit of a pain but does work. 

The nodejs API for Okta is really in need of some attention. Their issue tracker is a wasteland of Okta staff promising to do things like Typescript types and not delivering. 