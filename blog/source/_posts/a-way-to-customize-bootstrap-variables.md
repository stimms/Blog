---
layout: post
title: A way to customize bootstrap variables
authorId: simon_timms
date: 2015-05-05
---
I have been learning a bunch about building responsive websites this last week. I had originally been using a handful of media-queries but I was quickly warned off this. Apparently the "correct" way of building responsive websites is to lean heavily on the pre-defined classes in bootstrap. 

This approach worked great right up until I got to the navbar, that thing that sits at the top of the screen on large screens and collapses on small screens. My issue was that my navbar had a hierarchy to it so was a little wider than the normal version. As a result the navbar looked cramped on medium screens. I wanted to change the point at which the break between the collapsed and full navbar fired. 

Unfortunatly the suggested approach for this is to customize the boostrap source code and rebuild it. 

![Bootstrap documentation](http://imgur.com/4UnshDq.png)

I really didn't want to do this. The issue is that I was pulling in a nice clean bootstrap from bower. If I started to modify it then anybody who wanted to upgrade in the future would be trapped having to figure out what I did and apply the same changes to the updated bootstrap. 

The solution was to go to the web brain trust that is [James Chambers](http://jameschambers.com) and [David Paquette](http://www.davepaquette.com/). After some discussion we came up with just patching the variables.less file in bootstrap. 

#How does that look?

My project already used gulp but was built on top of sass for css so I had to start by adding a few new packages to my project

    npm install --save-dev gulp-less
    npm install --save-dev gulp-minify-css
    npm install --save-dev gulp-concat
    
Then I dropped into my gulpfile. As it turns out I already had a target that moved about some vendor css files. All the settings for this task were defined in my config object. I added 4 lines to that object to list the new bootstrap variables I would need. 

```
vendorcss: {
      input: ["bower_components/leaflet/dist/*.css", "bower_components/bootstrap/dist/css/bootstrap.min.css"],
      output: "wwwroot/css",
      bootstrapvariables: "bower_components/bootstrap/less/variables.less",
      bootstrapvariablesoverrides: "style/bootstrap-overrides.less",
      bootstrapinput: "bower_components/bootstrap/less/bootstrap.less",
      bootstrapoutput: "bower_components/bootstrap/dist/css/"
    },
```

The ```bootstrapvariables``` gives the location of the variables.less file within bootstrap. This is what we'll be patching. The ```bootstrapvariablesoverrides``` gives the file in my scripts directory that houses the overrides. The ```bootstrapinput``` is the name of the master file that is passed to less to do the compilation. Finally the ```bootstrapoutput``` is the place where I'd like my files put.

To the vendorcss target I added 

```
gulp.src([config.vendorcss.bootstrapvariables,config.vendorcss.bootstrapvariablesoverrides])
    .pipe(concat(config.vendorcss.bootstrapvariables))
    .pipe(gulp.dest('.'));
```
This takes an override file that I keep in my style directory and appends it to the end of the bootstrap variables. In it I can redefine any of the variables for bootstrap. 

```
  gulp.src(config.vendorcss.bootstrapinput)
   .pipe(less())
   .pipe(minifyCSS())
   .pipe(rename(function (path) {
       path.basename = "bootstrap.min";
   }))
   .pipe(gulp.dest(config.vendorcss.bootstrapoutput));
```
This bit simply runs the bootstrap build and produces and output file. Our patched variables.less is included in the newly rebuild code. The output is passed along to the rest of the task which I left unmodified. 

The result of this is that I now have a modified bootstrap without having to actually change bootstrap. If another developer, or me, comes along to upgrade boostrap it should be apparent what was changed as it is all isolated in a single file. 
