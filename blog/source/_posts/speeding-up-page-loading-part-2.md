---
layout: post
title: Speeding up page loading - part 2
authorId: simon_timms
date: 2013-12-09
---

In part 1 of this series I talked about speeding up page loading by combining and minimizing CSS and JavaScript files. Amongst the other things loaded by a web page are images. In fact images can be one of the largest items to load. There are a number of strategies for dealing with large or numerous images. I'll start with the most radical and move to the least radical.

First approach is to get rid of some or all your images. While it seems crazy because you spent thousands on graphically designing your site it might be that you can duplicate the look without images. Removing even a couple of images can speed up page loading significantly. Instead of images you have a couple of options. You can do without images at all or you can replace the images with lower bandwidth options such as an icon font or pure CSS.

Icon fonts are all the rage at the moment. Downloading custom fonts in the browser is nothing new but a couple of years ago somebody realized that instead of having letters make up the font glyphs you could just as easily have icons. Thus fonts like [fontawesome](http://fontawesome.io/icons/)were born. Fonts are a fairly bandwidth efficient method of providing small images for your site. They are vector based thus far smaller and more compressible than raster fonts. The problem is that an icon font might have hundreds of icons in it which you're not using. This can be addressed by building a custom icon font. If you're just interested in font-awesome then [icnfnt](http://www.icnfnt.com/)is the tool for doing that.

Alternately you can build your own images using pure CSS. The logo for one of the sites I run is pure CSS. At first it was kind of a geek thing to do to prove we could create a logo that way but having it in CSS actually allowed us to scale it infinitely and reduced the size of the page payload. It is a bit of an adventure in CSS to build anything complicated but worthwhile. If your image it too complicated for CSS then perhaps SVG is the tool for you. Scalable vector graphics can be directly included in the markup for your site so require no additional requests and are highly compressible.

Some images aren't good candidates for vector images. For these there are fewer options. The first step is to play around with the image compression to see if you can trade some quality for image size. Try different compression algorithms like gif, and png then play with the quality settings. This is a bit of a rabbit hole, you can spend days on this. Eventually you end up kidnapping people off the street, strapping them into a chair and asking them "Which one is better A or B, B or C, A or C?". This is okay if you're an optician but for regular people it usually results in jail.

Once the image is optimized you can actually embed the image into your CSS. This is a brand new thing for me which my mentor told me about. In your CSS you can put a base 64 encoded version of your image. Manually this can be done at this [cool website](http://webcodertools.com/imagetobase64converter)but more reasonably there is an awesome [grunt task](https://npmjs.org/package/grunt-data-uri) for processing CSS.

If you have a series of smaller images then you might consider using CSS sprites. With sprites you combine all your images into a larger image and then use CSS to display only portions of the larger image at a time.[![sprite](http://stimms.files.wordpress.com/2013/12/sprite.jpg)](http://stimms.files.wordpress.com/2013/12/sprite.jpg)

Personally I think this is a huge amount of work with the same results achieved through embedding the images in the CSS. I wouldn't bother, but you might want to give it a try as combining the images can result in a smaller image overall due to encoding efficiencies.

I was pretty impressed with how much image optimization helped our use of data. We saved about 100Kib which doesn't sound like a lot but on slow connections it is a lifetime. It is also a lot of data in aggregate.

So far we've been concentrating on reducing the amount of data sent and received. In the next part we'll look at some activities on the server side to reduce the time spent building the response.



