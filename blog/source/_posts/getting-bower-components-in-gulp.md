---
layout: post
title: Getting Bower Components in Gulp
authorId: simon_timms
date: 2015-01-22
---
I'm embarking on an adventure in which I update the way my work project handles JavaScript. Inspired by [David Paquette's blog](http://www.davepaquette.com/archive/2014/10/08/how-to-use-gulp-in-visual-studio.aspx#comment-4151) I'm moving to using gulp and dropping most of the built in bundling and minimization stuff from ASP.net. This is really just a part of a big yak shaving effort to try out using react on the site. I didn't expect it to turn into this huge effort but it is really going to result in a better solution in the end. It is also another step on the way to aligning how we in the .net community develop JavaScript with the way the greater web development community develops JavaScript. It will be the way that the next version of ASP.net handles these tasks. 

One of the many yaks that need shaving is moving many of my JavaScript dependencies to using bower. Bower is to JavaScript as nuget is to .net or as CPAN is to perl or as gem is to ruby: it is a package manager. There is often some confusion between bower and npm as both are package managers for JavaScript. I like to think of npm as being the package manager for build infrastructure and running on node. Whereas bower handles packages that are sent over the wire to my clients. 

So on a normal project you might have jquery, underscore/lodash and require.js all installed by bower.

We would like to bundle up all of these bower components into a single minified file along with all our other site specific JavaScript. We can use Gulp, a build tool for JavaScript, to do that. Unfortunately bower packages may contain far more than they actually need to and there doesn't seem to be a good standard in place to describe which file should be loaded into your bundle. Some projects include a bower.json file that defines a main file to include. This is an excerpt from the bower.json file from the flux library:

```
   {
  "name": "flux",
  "description": "An application architecture based on a unidirectional data flow",
  "version": "2.0.2",
  "main": "dist/Flux.js",
  ...
```

Notice the main file listed there. If we could read all the bower.json files from all our bower packages then you could figure out what files to include. There is, as with all things gulp, a plugin for that. You can install it by running

    npm install --save-dev main-bower-files
    
Now you can reference this from your gulp file by doing

    var mainBowerFiles = require('main-bower-files');
    
You can plug this task into gulp like so
```
gulp.task('3rdpartybundle', function(){
  gulp.src(mainBowerFiles())
  .pipe(uglify())
  .pipe(concat('all.min.js'))
  .pipe(gulp.dest('./Scripts/'));
});
```

From time to time you might find a package that fails to properly specify a main file. This is a bit annoying and certainly something you should consider fixing and submitting back to the author.  To work around it you can specify an override in your own bower.json file. 

```
"dependencies": {
    "marty": "~0.8.3",
    "react": "~0.12.2"
  },
  "overrides": {
    "superagent":{
      "main": "superagent.js"
    }
  }
```

Great! Okay now what if you have an ordering dependency? Perhaps you need to load requirejs as the last thing in your bundle. This can be done through the ordering plugin. Start with npm and install the plugin:

    npm install --save-dev gulp-order

Again you'll need to specify the package inside the gulp file

    var order = require('gulp-order');
    
Now you can plug the ordering into the build pipeline    

```
gulp.task('3rdpartybundle', function(){
  gulp.src(mainBowerFiles({paths: {bowerJson: 'bower.json', bowerDirectory: 'bower_components'}}))
  .pipe(order(["*react*", "*requirejs*"])
  .pipe(uglify())
  .pipe(concat(config.all3rdPartyFile))
  .pipe(gulp.dest('./Scripts/'));
});
```

The ordering plugin doesn't, at the time of writing, support matching the last rule so making something appear last is harder than making it appear first. There is, however, a [pull request](https://github.com/sirlantis/gulp-order/pull/12) out to fix that.

This is really just scratching the surface of the nifty stuff you can get up to with gulp. I'm also building typescript files, transcompiling es6 to es5 and linting my JavaScript. It's a brave new world!
