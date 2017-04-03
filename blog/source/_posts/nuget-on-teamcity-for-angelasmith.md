---
layout: post
title: Nuget on TeamCity for AngelaSmith
authorId: simon_timms
date: 2013-03-18
---

Well I had quite the adventure setting up builds for [AngelaSmith](https://github.com/MisterJames/AngelaSmith). [CodeBetter](http://codebetter.com/) are kind enough to host a TeamCity instance on which you can build open source projects. I got an account and started to set up the build. The first part went okay, I got builds running and unit tests going as well as code coverage. I mostly put in code coverage as a metric to annoy [Donald Belcham](https://twitter.com/dbelcham)who doesn't believe in metrics.

Then I turned my attention to building nuget packages. I decided that I was going to do things right and build not just release packages but also symbol packages on the off chance that somebody would like to debug into our very simple library. I was also going to have the build upload packages directly to nuget.org.

The first thing I did was create a nuspec file.

<script src='https://gist.github.com/stimms/5174595.js'></script>

I didn't specify any explicit contents for it. Instead I use a convention based directory structure which allows me to add files to the package easily. In the build Icopy the output of the build into a lib directory. I copy both the .dll and the .pdb file which is needed for constructing a source package. I also create a src directory adjacent to my lib directory. Into that directory I copy the source code too as this needs to be in the symbol package. To do this I just use a bat script. Sometimes the simple solution is still the best.

<script src='https://gist.github.com/stimms/5174642.js'></script>

TeamCity has a great set of nuget package tasks. I started with the one for assembling the package.

<div class="wp-caption aligncenter" id="attachment_2463" style="width: 310px">[![TeamCity Nuget Package Settings](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-03-15-at-8-30-39-pm.jpg?w=300)](http://stimms.files.wordpress.com/2013/03/screen-shot-2013-03-15-at-8-30-39-pm.jpg)TeamCity Nuget Package Settings

</div>This is very simple, you just need to set the path to the nuspec file. I added a version number based on the build number and a â€œ-betaâ€. When I took this screenshot I was pretty sure that this followed semantic version numbering. As it turns out it doesn't. As these are builds not for release I should be numbering them as +build${buildNumber}.Unfortunatelynuget does not allow for proper semantic versioning so we have to live with a â€“ instead of a +.

The next step was to publish the packages to nuget.org. To do this I set up a new account on nuget.org just for publishing AngelaSmith packages. The secret key is entered into TeamCity which takes care of uploading both the standard package as well as the symbol packages. I don't know how secure the key is which is one of the reasons I used a standalone account. If it is compromised none of the other nuget packages I maintain will be vulnerable.

With this all set up the AngelaSmith builds now push directly to nuget.org.



