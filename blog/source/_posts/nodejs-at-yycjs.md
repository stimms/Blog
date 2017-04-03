---
layout: post
title: NodeJS at YYCJS
authorId: simon_timms
date: 2013-06-03
---

I took a trip down to the YYCJS meetup group the other night. This is one of those rambling, incoherent posts which serves as the notes I took while there.

I hadn't been before so that was pretty exciting. I felt like a bit of a fraud because

a) I only pretend to know JavaScript

b) I work for The Man

Maybe I'm assuming too much about the thick framed fellows I met at YYCJS but it seemed like a good place to pretend I knew about made up bands and worked for a small socially aware company instead of a huge people-hatingconglomerate. Everybody had a macbook and an android phone. I half fit in.

I guess this was the second half of a series of NodeJS talks "“ I missed the first half so I could be in big trouble.

- There is a constant debate between native and web applications
- Web is still much slower but is catching up at an alarming rate. (Good! -ed.)
- There is apparently an LLVM to JavaScript compiler which means that you can write your JavaScript in pretty much any language.
- There was a demo of an amazing idea to use a smart phone as a front end to remote control a web browser using web sockets. You associated the device and the browser by having a QR code shown on the browser which contains a url. That associates the phone with the browser.[QRControl](https://github.com/bredele/qrcontrol)
- Google [RollIt](http://chrome.com/campaigns/rollit) extends the idea into a full game using the accelerometer interface for chrome. I didn't even know that there were hooks for accelerometers in chrome.
- I should hang out here more, Jesus my job is boring.
- I'm so appreciative that the people demoing make use of the command line.
- Using the same QR code linking with websockets Oliver showed using a device to draw on a canvas from the phone. Some great applications for that.
- Oh god, they're talking about nodejs "“ node your head knowingly. They'll beat you to death with designer beer bottles if you let them know you're hosting on IIS
- Oliver has created a framework with another guy called Oliver. The framework is called Olives.
- Olives is a DOM agnostic MVVM framework. That means that it can be used in Node which has no DOM.
- Oliver isn't use to a QWERTY keyboard. He is pretty cool so I bet he has his own keyboard mapping. Hardcore
- It seems like require.js is pretty much a given in the JS community, I really need to start pulling that in to my JavaScript stuff. Require is a dependency resolving library which live loads libraries as they become needed.
- Socket.io also seems to be pretty much a given. Looking at the socket.io site it looks super easy, far easier than SignalR but I don't know if it has a good scalability story.
- Live binding in MVVM frameworks rerender only segments of the page instead of the whole thing. That makes it much faster. So I guess the morale is keep your templates small and bind at the lowest possible level.
- Apparently AngularJS is slow. Oh. Damn, I was going to make use of that framework.
- I have to say live binding is pretty impressive. I guess it is just implemented as an object wrapper which keeps track of what it binds and updates it.
- Olives support live binding but it is provided through a plugin so you don't need to use it or you could swap out the implementation.
- The binding for Olives is in the HTML through data-* attributes. Acutally it makes use of data-model and then you pass descriptive strings into that. I'm not crazy about that, seems like tooling around richer data attributes would be easier.
- Olives has various other plugins including a nifty socketIO transport one which can live update from changes in a database such as Couch
- JavaScript can be used at every level, on the browser, on the server with NodeJS and in the database with CouchDB
- The demo for CouchDB is very interesting. It looks like what happens is that the NodeJS server subscribes to a document collection on the CouchDB server. When the document collection changes CouchDB publishes a change notification and sends the entire collection back down to the NodeJS server. This then passes that updated collection onto the browser.
- There are some serious scalability concerns in my mind there

[![Gimp is hard to use, FYI](http://stimms.files.wordpress.com/2013/06/nodesubscribe.jpg)](http://stimms.files.wordpress.com/2013/06/nodesubscribe.jpg)


## Authentication

- Express seems to be THE framework to put on top of NodeJS
- There are some authentication libraries for Node. [Passport.js](http://passportjs.org/) seems to be a very serious contender
- Defining modules in Node is easy. You basically just put the functionality inside a file and require it. Inside that file it seems sensible to add your functions to an object and then set module.export to that object.
- By defining an index.js and shoving it in your module's root it will automatically be loaded by just requiring the directory. Inside that file you can require additional things which will bring in all the rest of your files.



