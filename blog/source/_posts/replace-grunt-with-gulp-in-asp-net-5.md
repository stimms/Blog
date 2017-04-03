---
layout: post
title: Replace Grunt with Gulp in ASP.net 5
authorId: simon_timms
date: 2015-02-20
---
The upcoming version of ASP.net and Visual Studio includes first class support for both Grunt and Gulp. I've been using Gulp a fair bit as of late but when I created a new ASP.net 5 project I found that the template came with a gruntfile instead of a gulpfile. My tiny brain can only hold so many different tools so I figured I'd replace the default Grunt with Gulp.

Confused? Read about [grunt](http://gruntjs.com/) and [gulp](http://gulpjs.com/). In short they are tools for building and working with websites. They are JavaScript equivlents of ant or gnumake - alghough obviously with a lot of specific capabilities for JavaScript. 

The gruntfile.js is pretty basic

```
// This file in the main entry point for defining grunt tasks and using grunt plugins.
// Click here to learn more. http://go.microsoft.com/fwlink/?LinkID=513275&clcid=0x409

module.exports = function (grunt) {
    grunt.initConfig({
        bower: {
            install: {
                options: {
                    targetDir: "wwwroot/lib",
                    layout: "byComponent",
                    cleanTargetDir: false
                }
            }
        }
    });

    // This command registers the default task which will install bower packages into wwwroot/lib
    grunt.registerTask("default", ["bower:install"]);

    // The following line loads the grunt plugins.
    // This line needs to be at the end of this this file.
    grunt.loadNpmTasks("grunt-bower-task");
```

It looks like all that is being done is bower is being run. Bower is a JavaScript package manager and running it here will simply install the packages listed in the bower.json. 

So to start we need to create a new file at the root of our project and call it gulpfile.js

![](http://i.imgur.com/9KiNPBs.png)

Next we can open up the package.json file that controls the packages installed via npm and add in a couple of new packages for gulp.

```
    "version": "0.0.0",
    "name": "IdentityTest",
    "devDependencies": {
        "grunt": "^0.4.5",
        "grunt-bower-task": "^0.4.0",
        "gulp": "3.8.11",
        "gulp-bower": "0.0.10"
    }
}
```

We have gulp here as well as a task that will install bower. The packages basically mirror those found in the file already for grunt. Once we're satisfied we've replicated grunt properly we can come back and take out the two grunt entries. Once that's done you can run 

```
npm install
```
   
From the command line to add these two packages. 

In the gulp file we'll pull in the two required packages, gulp and gulp-bower. Then we'll set up a default task and also one for running bower

```
var gulp = require('gulp');
var bower = require('gulp-bower');

gulp.task('default', ['bower:install'], function () {
    return;
});

gulp.task('bower:install', function () {
    return bower({ directory: "wwwroot/lib" });
});
```

We can test if it works by deleting the contents of wwwroot/lib and running bower from the command line. (If you don't already use gulp then you'll need to install it globally using ```npm install -g gulp```) The contents of the directory are restored and we can be confident that gulp is working. 

We can now set this up as the default by editing the project.json file. Right at the bottom is 
```
 "scripts": {
        "postrestore": [ "npm install" ],
        "prepare": [ "grunt bower:install" ]
    }
```

We'll change this from grunt to gulp

```
 "scripts": {
        "postrestore": [ "npm install" ],
        "prepare": [ "gulp bower:install" ]
    }
```

As a final step you may want to update the bindings between visual studio actions and the gulp build script. This can normally be done through the task runner explorer, however at the time of writing this functionality is broken in the Visual Studio CTP. I'm assured that it will be fixed in the next release. In the meantime you can read more about gulp on David Paquette's [excelent blog](http://www.davepaquette.com/archive/2014/10/08/how-to-use-gulp-in-visual-studio.aspx).
