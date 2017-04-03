---
layout: post
title: ASP.net Identity Default Cookie Expiry
authorId: simon_timms
date: 2014-03-03
---

I couldn't find how long the cookie expiry for a cookie based identity token is for ASP.net Identity anywhere in any documentation. I ended up decompiling Microsoft.Owin.Security.Cookies in which that property is defined. The default expiry is 14 days with a sliding expiration window.

The full set of defaults looks like:

<script src='https://gist.github.com/stimms/9329518.js'></script>

**UPDATE: **[Pranav Rastogi](https://twitter.com/rustd)**[](https://twitter.com/rustd)**was kind enough to point out that the source code for this module is part of the Katana Project and is available on [codeplex](https://katanaproject.codeplex.com/SourceControl/latest#src/Microsoft.Owin.Security.Cookies/CookieAuthenticationOptions.cs)



