---
layout: post
title: Quick post on getting node information from Umbraco with IronPython
authorId: simon_timms
date: 2009-10-28
---

I was just working with the IronPython page type in umbraco and needed to get a property from the page I was on. This can be done by accessing the Node API found in umbraco.presentation.nodeFactory. In order to be able to pull a value you will need pull in that part of the API

  
import umbraco.presentation.nodeFactory  
from umbraco.presentation.nodeFactory import *

Now you can get the current node and query its properties

  
print Node.GetCurrent().GetProperty("Address").Value



