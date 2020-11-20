---
layout: post
title: Updating SMS factor phone number in Okta
authorId: simon_timms
date: 2020-11-20 16:00

---

Hello, and welcome to post number 2 today about the Okta API! In the [previous post](/2020/11/20/2020-11-20-okta-resend-sms-nodejs) we got into resending the SMS to the same phone number already in the factor. However people sometimes change their phone number and you have to update it for them. To do this you'll want to reverify their phone number. Unfortunately, you will get an error from Okta that an existing factor of that type exists if you simply try to enroll a new SMS factor. 

You actually need to follow a 3 step process 

1. Find the existing factors
2. Remove the existing SMS factor
3. Re-enroll the factor being user to specify updatePhone

<!--More-->

In the NodeJS API it is actually relatively simple with the possible exception of the weird `Collection` thing that is used instead of just using an array to hold a list of items. 

First we get the existing factors, this assumes you've already got an instance of the Okta Client

```
let factorsCollection = await oktaClient.listFactors(oktaUserId);
let factors: any[] = [];
await factorsCollection.each((factor) => factors.push(factor));
let existingFactor = factors.find((x) => x.factorType == 'sms');
```

`existingFactor` now hold the existing SMS factor (if there is one). We need to remove it using `deleteFactor`

```
if(existingFactor){
  logger.info(`Removing existing sms factor ${existingFactor.id}`);
  await oktaClient.deleteFactor(oktaUserId, existingFactor.id);
}
```

Now we need to recreate the factor. Pay special attention to the additional parameter to `enrollFactor` which is the `updatedPhone` flag. Without this you'll get an error that an existing verified phone number exists on the account. 

```
const smsFactor = {
  factorType: 'sms',
  provider: 'OKTA',
  profile: {
    phoneNumber: phoneNumber
  }
};
logger.info(`Starting okta enroll factor for ${oktaUserId} and sms factor ${JSON.stringify(smsFactor)}`);
let enrollResult = await oktaClient.enrollFactor(oktaUserId, smsFactor, {updatePhone: existingFactor !== undefined});
```

And that's it! More complex than I'd like but it is actually documented in the Okta docs if you dig [deep enough](https://developer.okta.com/docs/reference/api/factors/#enroll-okta-sms-factor-by-updating-phone-number)