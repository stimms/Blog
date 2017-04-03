---
layout: post
title: Setting Version Number in TFS
authorId: simon_timms
date: 2010-12-23
---

Yet another TFS post! Is it because I believe that TFS is the cat's meow and that you should all run out an buy it right now? Not so much but it is starting to grow on me and I can admit that building .net projects building it is okay. There is really a need for a collection of workflow tasks and some better generic templates. I'm going to see if I can convince my boss to let me open source some of the stuff I'm building for running NUnit tests and the such.

One thing which is missing from TFS is the ability to embed build information in the binaries which are built. I always like this because it is easy to get clients to right click on a file and tell you the version number which will give you an idea as to where to start looking at their problem. The version numbers in .net builds are set using attributes which are commonly defined in Properties/AssemblyInfo.cs. The quickest solution is to update the .cs files before running the compilation step of the build. To do this I created a new task to plug into the windows workflow process that is run during a build in TFS2010. Funnily enough Jim Lamb used setting the build numbers as his example for [creating a custom TFS task](http://blogs.msdn.com/b/jimlamb/archive/2010/02/12/how-to-create-a-custom-workflow-activity-for-tfs-build-2010.aspx).

I started with the project he provided and made changes to it because my build numbers are slightly different from his. Jim uses the date as the build number, that's reasonable but I want to know the exact changeset. I also changed what was passed into and out of the task. It made sense to me to pass in the IBuildDetails object rather than trying to couple task together to get out of the object what I wanted.

First change the in argument to BuildDetail

  
[RequiredArgument]  
 public InArgument BuildDetail  
 {  
 get;  
 set;  
 }

Then the execute to set up all the fields I want

  
protected override void Execute(CodeActivityContext context)  
 {  
 String filePath = FilePath.Get(context);  
  
 if (String.IsNullOrEmpty(filePath))  
 {  
 throw new ArgumentException( "Specify a path to replace text in", "FilePath");  
 }  
  
 if (!File.Exists(filePath))  
 {  
 throw new FileNotFoundException("File not found", filePath);  
 }  
  
 IBuildDetail buildDetail = BuildDetail.Get(context);  
 if (buildDetail == null)  
 {  
 throw new ArgumentException("Specify the build detail", "BuildDetail");  
 }  
  
 // ensure that the file is writeable  
 FileAttributes fileAttributes = File.GetAttributes(filePath);  
 File.SetAttributes(filePath, fileAttributes & ~FileAttributes.ReadOnly);  
  
 string contents = File.ReadAllText(filePath);  
  
 contents = SetAssemblyConfigurationString(contents, buildDetail.BuildNumber);  
 contents = SetVersionNumberString(contents, buildDetail.SourceGetVersion);  
 contents = SetVersionFileNumberString(contents, buildDetail.SourceGetVersion);  
 contents = SetAssemblyCopyrightString(contents);  
  
 File.WriteAllText(filePath, contents);  
  
 // restore the file's original attributes  
 File.SetAttributes(filePath, fileAttributes);  
 }

Slap it into the workflow, set the properties and you're good to go. I put my task after Get Workspace in a branched version of the default workflow.

[![](http://stimms.files.wordpress.com/2010/12/hatt1.png?w=300)](http://stimms.files.wordpress.com/2010/12/hatt1.png)

One additional thing to remember is that you need to check a compiled version of your task into TFS somewhere and point the build controller at the location. Thanks to this all the binaries which come out of my build are stamped properly.



