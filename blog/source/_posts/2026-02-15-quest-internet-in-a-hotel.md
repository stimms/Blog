---
layout: post
title: Defeating Hotel Wifi Sigin on a Meta Quest
authorId: simon_timms
date: 2026-02-15
---

We went on vaction and for some reason we let the kids bring their Meta Quest VR headsets. I don't know if you've used one of these things but to my mind they're a laundry list of everything that's wrong with Facebook/Meta. Not to mention that I'm old so putting them on makes me feel sick almost immediately. 

Today's problem was that the hotel wifi was open but required that you sign in using a browser: it was a captive wifi. In a delicious catch-22 we don't let the kids use the browser on the Quest and to request access to it you need to have internet access. So we were stuck in a loop of "you need to sign in to get internet access" and "you need internet access to sign in". 

We didn't have any phone internet access at this hotel so we had to figure out a way around the problem. What I ended up doing was pulling the MAC address off the Quest and then, on my laptop, spoofing the address and signing into the wifi. 

I powered off the Quests and then opened a terminal on the laptop and ran the following commands to spoof the MAC address of the Quest:

```
networksetup -setairportpower en0 off
networksetup -setairportpower en0 on
sudo ifconfig en0 ether 2e:81:fd:cb:ff:41 # address off the quest
```

Then open the browser and sign in to the wifi. With that done I switched the MAC address back to normal on the laptop and restarted the Quest. Because the hotel wifi filtering only cares about the MAC address it now figured the quest was connected and let it on the network.

Thanks class I took on networking 20 years ago!