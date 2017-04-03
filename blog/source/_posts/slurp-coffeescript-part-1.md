---
layout: post
title: Slurp -  CoffeeScript Part 1
authorId: simon_timms
date: 2013-04-19
---

I'm doing a couple of talk at [Prairie DevCon](http://prairiedevcon.com/)in Winnipeg in early May. One of them is about data visualizations and the other is about next generation JavaScript. This is the first time I've talked about JavaScript in front of an audience which actually knows about JavaScript. Scary.

CoffeeScriptis a language which compiles down to JavaScript instead of some binary language. In that way it is not unlike TypeScript about which I [have ](http://blog.simontimms.com/2013/03/05/typescript-compiling-your-first-program/ "Typescript "“ Compiling your firstprogram") [written ](http://blog.simontimms.com/2013/03/05/typescript-creating-large-programs/ "Typescript "“ Creating largeprograms") [a](http://blog.simontimms.com/2013/03/11/adding-typescript-to-an-existing-project/ "Adding TypeScript to an ExistingProject") [few ](http://blog.simontimms.com/2013/03/15/typescript-cleaning-up-warnings/ "Typescript "“ Cleaning upWarnings") [things](http://blog.simontimms.com/2013/01/28/this-vs-_this-in-typescript/ "this vs. _this inTypeScript").To get started the easiest thing to do is download and install node.js from [their site](http://nodejs.org/). Node provides an implementation of the V8 JavaScript engine which can be run headless and it includes the npm package manager. Once you have node installed it is as simple as running

npm install -g coffee-script

that will even add CoffeeScriptto your path. What a friendly fellow that node package manager is! If you're running this on OSX or Linux and you want to install it globally (that's what the -g flag does) then you'll need to have sudo.

We can now get started with a simple CoffeeScriptprogram. By convention CoffeeScript files end in .coffee but for maximum confusion you should end them with .java or .c (please don't). The lengthening of file extensions is kind of funny, don't you think?

.c -> .cpp-> .java -> .coffee -> .heythisisajavascriptfile

So let's start with a really simpleCoffeeScript program.

I've spoken before about how to arrange large code bases using namespaces and classes. CoffeeScript doesn't have a built in concept of namespace or module but it does have a concept of classes. You can simulate modules but that's a lesson for another day, classes will do just fine for now.

<script src='https://gist.github.com/stimms/5423493.js'></script>

The first thing you'll notice is that the syntax is different from JavaScript. CoffeeScript uses a python style syntax which means that whitespace is important. The constructor keyword creates a constructor which takes a single parameter name. You'll notice that name is prefaced by an @ symbol. This makes name an automatically assigned property.

<script src='https://gist.github.com/stimms/5423703.js'></script>

Here you can see the syntax for setting up ifs and elses. Again notice the lack of braces, code blocks are denoted by indentation.

Over the next week I'll be delving more into coffeescript in preparation for my talk.



