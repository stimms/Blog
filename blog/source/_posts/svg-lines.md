---
layout: post
title: SVG Lines
authorId: simon_timms
date: 2013-06-13
---

I know what you're thinking: you're thinking that the lines in an SVG image are pretty boring and how did I get a whole post out of this? Actually lines have some interesting properties in SVG which makes them super-cool(you have to say super-cool with a French accent â€“ the oxymoronic nature of being both French and cool is delicious).

Up first is that you can specify line endings for any line. If you've done work with PowerPoint or any sort of graph software then you'll have seen lines with arrows at the end. In SVG you can actually end line with any shape you want. Firs you need to specify a marker then apply it to a line. For a normal arrow ending you just need to specify a quick triangular path.

<script src='https://gist.github.com/stimms/5779358.js'></script>

There are also some fun attributes hanging off the marker itself. The id specifies a name; this is used later to attach the marker to the line. Next a view box is simply a way of defining how big of an object we're creating. This is a small arrow so we've set the offset to (0,0) and it is 10 pixels by 10 pixels. The refX and refY will offset the arrow from the attachment point. We're setting a value here just enough to center it on the line. Next is the actual size of the marker and the final orientation attribute rotates the marker so it follows the same slope as the final segment of the line.

This marker can easily be attached to a line like so:

<script src='https://gist.github.com/stimms/5779396.js'></script>

And it ends up looking like

<div class="wp-caption aligncenter" id="attachment_2844" style="width: 223px">[![Slanty!](http://stimms.files.wordpress.com/2013/06/line1.jpg)](http://stimms.files.wordpress.com/2013/06/line1.jpg)Slanty!

</div>Arrows are pretty boring so let's make something more interesting.

<script src='https://gist.github.com/stimms/5779476.js'></script>

Will get you

[![startstop](http://stimms.files.wordpress.com/2013/06/startstop.jpg)](http://stimms.files.wordpress.com/2013/06/startstop.jpg)That stop sign is a little sickly but it gives you a good idea of what you can do.

The next cool thing is that you can specify a dotted line. This is easily done with thestroke-dasharray attribute.

[![dotted](http://stimms.files.wordpress.com/2013/06/dotted.jpg)](http://stimms.files.wordpress.com/2013/06/dotted.jpg)This line is created with a value of 10,10. That's 10 pixels of line then 10 pixels of space. The hilarious thing is that you can specify very complex pattern of dots and dashes. Each alternate number is either a space or a visible part of the line. For instance this is how it looks with a value ofstroke-dasharray=â€3,3,3,3,3,3,10,10,10,10,10,10,3,3,3,3,3,3â€³.

[![sos](http://stimms.files.wordpress.com/2013/06/sos.jpg)](http://stimms.files.wordpress.com/2013/06/sos.jpg)For the very observant you may recognize this pattern as SOS in Morse code.

Don't say I've never given you information you can use to signal planes from a life raft using only a mirror.



