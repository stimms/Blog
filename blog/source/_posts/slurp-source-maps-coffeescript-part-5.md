---
layout: post
title: Slurp -  Source Maps CoffeeScript Part 5
authorId: simon_timms
date: 2013-04-25
---

Today I would like to talk a bit about source maps. Back when the world was new and languages like TypeScript and CoffeeScript which transcompile to JavaScript were just poking their heads out of the primordial soup there was no way to trance an error in your JavaScript back to the line of code in the generating language. For simple programs this wasn't a huge deal because you could squint a bit and figure it out. But as the languages became more powerful and programs more complicated the situation worsened.Fortunatelya hero arose in the form of source maps.

Source maps are about what you would expect: they are a way of translating from one representation of the source to another. In our case from JavaScript to the source language, CoffeeScript. These maps can be consumed by the browser and the output shown in the debugging consoles in Chrome and FireFox. As source maps are still a bit new you may need to enable them manually before you make use of them. In Chrome, my development browser, this can be done by hitting F12 the clicking on the little cog icon in the bottom right corner of the developer tools. Under sources there is a check box for Enable source maps

<div class="wp-caption aligncenter" id="attachment_2647" style="width: 760px">[![Enabling source maps](http://stimms.files.wordpress.com/2013/04/enable-souce-maps.jpg?w=750)](http://stimms.files.wordpress.com/2013/04/enable-souce-maps.jpg)Enabling source maps

</div>Next up is telling the CoffeeScript compiler to generate source maps too while it is compiling. That is just a question of including the -m flag

coffee -m -c test.coffee

You will now see that the compiler does two things. First it emits a .map file which is the mapping from JavaScript back to CoffeeScript. Next it adds the line

/* //@ sourceMappingURL=test.map */

To the bottom of the generated JavaScript file. That's pretty much it now. I tested mine out with a little bit of mildly complicated CoffeeScript

<script src='https://gist.github.com/stimms/5460096.js'></script>

This generates output which is pretty different from the input so source mapping is handy

<script src='https://gist.github.com/stimms/5460733.js'></script>

In Chrome now I see not only the JavaScript but also the CoffeeScript when I jump into the source view. I'm able to set break points on the CoffeeScript and execution will actually stop there allowing me to debug CoffeeScript directly instead of guess at the source CoffeeScript line.

<div class="wp-caption aligncenter" id="attachment_2650" style="width: 476px">[![Breaking on CoffeeScript](http://stimms.files.wordpress.com/2013/04/breakpoint.jpg)](http://stimms.files.wordpress.com/2013/04/breakpoint.jpg)Breaking on CoffeeScript

</div>Pretty nifty! You can actually use source maps to map minified JavaScript too, you can even use multiple layers of maps to translate from minified JavaScript -> full JavaScript -> CoffeeScript. Not every minifier has support for this but[UglifyJS](http://lisperator.net/blog/uglifyjs-v2-news/) certainly does.



