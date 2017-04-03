---
layout: post
title: Web Deployment in TFS Build
authorId: simon_timms
date: 2010-12-21
---

I have been working the last couple of days on doing web deployment as part of one of my TFS builds. There are [quite](http://vishaljoshi.blogspot.com/2010/11/team-build-web-deployment-web-deploy-vs.html) [a](http://stackoverflow.com/questions/4041836/team-build-publish-locally-using-msdeploy) [few](http://weblogs.asp.net/jdanforth/archive/2010/04/24/package-and-publish-web-sites-with-tfs-2010-build-server.aspx) posts out there on how to do it and so far every last one of them is wrong. Technically they are fine, doing what they say will for sure deploy a package to your webserver*. However they fail to grasp the deploy only on successful build model. Why would I want to deploy something to a webserver which I had tests proving it didn't work? All that is gonna get you is trouble, and I have enough trouble around these parts. In order to get the behaviour I wanted I needed to edit the build workflow.

First step is get your build to produce the zip file and other files associated with deployment. To do that you need only add

<span style="font-style:italic;">/p:CreatePackageOnPublish=true /p:DeployOnB</span><span style="font-style:italic;">uild=true </span>

to the MSBuildArgumetns in your build definition. Here is a picture because it makes this post look longer and easier to follow:[![](http://stimms.files.wordpress.com/2010/12/msbuildflags1.png?w=300)](http://stimms.files.wordpress.com/2010/12/msbuildflags1.png)  
Woo, picture!

[![](http://stimms.files.wordpress.com/2010/12/branch.png?w=300)](http://stimms.files.wordpress.com/2010/12/branch.png)Next I branched an existing build template. These templates are located, by default, in BuildProcessTemplates folder at the root of your TFS project. You can put them anywhere you want, however, and you might want to put them in the same directory as your solution files, just to keep them spatially proximal. You want to branch rather than creating a new one because the default template is amazingly complex and recreating it would be painful. The default workflow is actually pretty reasonable and I have only minor quibbles with how it is put together.

Next we're going to to add some new parameters to the build so that people can easily configure the deployment [![](http://stimms.files.wordpress.com/2010/12/parameters.png?w=300)](http://stimms.files.wordpress.com/2010/12/parameters.png)from the build definition. To do this open up your newly branched template and click on arguments. I have yet to figure out how you can put these arguments into categories from the UI but from the XML it is [pretty easy](http://blogs.msdn.com/b/jpricket/archive/2009/12/23/tfs-2010-custom-process-parameters-part-2-metadata.aspx). I don't care enough to put things into categories at this time, although it probably took me longer to write this sentence about not caring than it would have taken to just do it.

As you can see I added 6 new properties

- PerformDeploy "“ A boolean which denotes if we should attempt a deploy at all. This keeps our project generic so it can be used on anything.
- DeploymentURL "“ The URL to which the deployment should run. I'm stuck on IIS6 so this is the MSDeployAgentService URL
- DeploymentUserName "“ The username to use during the deploy
- DeploymentPassword "“ The password to use during the deploy. You might want to be more creative about how you get your password to comply with whatever rules exist at your site.
- DeploymentApplicationPath "“ The path in IIS to which deployment should be run
- DeploymentCommand "“ The name of the *deploy.cmd file to run. This should

Now for the fun part: adding the new workflow item. If you have a slow computer, say anything slower than the computer from [Voyager](http://thecia.com.au/star-trek/voyager/406a.shtml#specs) then you're going to be sorry you ever opened this workflow. Scroll to the very bottom where there is a task for dealing with gated checkins. Our new task is going to go after this. Drag and If from the toolbox. In the conditional we're going to check that we had no build failures and that we should run a deploy:

<span style="font-style:italic;font-weight:bold;">BuildDetail.TestStatus = Microsoft.TeamFoundation.Build.Client.BuildPhaseStatus.Succeeded And PerformDeploy</span>

Now drop in an invoke process to the Then. In the arguments put

<span style="font-style:italic;font-weight:bold;">String.Format("/M:{0} /U:{1} /P:{2} ""-setParam:name='IIS Web Application Name',value='{3}'"" /Y ", DeploymentURL, DeploymentUserName, DeploymentPassword, DeploymentApplicationPath)</span>

File name:

<span style="font-style:italic;font-weight:bold;">Path.Combine(BuildDetail.DropLocation, DeploymentCommand)</span>

[![](http://stimms.files.wordpress.com/2010/12/task1.png?w=297)](http://stimms.files.wordpress.com/2010/12/task1.png)I also dropped a couple of WriteBuildMessage actions on there because that is how I roll. Remember this: the longer the build log the more important your job. Let's see that junior developer guy who only gets paid half what you do figure this 29Meg log file out.

That is pretty much it, save it, commit it and create a new build definition which uses it. You can either run it on commit or manually. I am running mine manually because I don't trust our unit tests to prove correctness yet.

*Assuming you have everything configured properly and you're not relying on black magic or even mauve magic.



