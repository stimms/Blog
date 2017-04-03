---
layout: post
title: CSS Animated Confirmations
authorId: simon_timms
date: 2015-01-24
---
<script src="http://code.jquery.com/jquery-2.1.3.min.js"></script>

I've been playing a little bit with changing how my app acknowledges that actions are running and have completed. I use a system, from time to time, that doesn't do good feedback for when an action is running. It drives me up the wall and puts me back in my Comput 301 class with the teacher talking about the importance of feedback. Rapid feedback is what makes the difference between an application feeling snappy and it feeling slow. Even if the actions take the same time to complete a system that does something to indicate it is working right away will make users feel far better. 

So knowing that my users are more and more on modern browsers I though I would give some animation a try.

<div style="margin-top: 20px; margin-bottom:20px">
<input type="text" class="target"/><input type="button" value="Save" class="trigger">
</div>

This was my first attempt and I'm going to put it into the field and see what people think. It is implemented using the animation css attribute. I start with some key frame defintions:
```css
  @keyframes save-sucessful-animation {
      0%  { background-color: white}
      25% { background-color: #DFF2BF;}
      75% { background-color: #DFF2BF;}
      100%{ background-color: white}
  }
```

I've listed the un-prefixed ones here but you'll need to prefix with -moz or -webkit or -ms for your needs. You could also use a CSS precompiler to do that for you (check out http://bourbon.io/). The transform here changes the colour to green, pauses for 50% of the animation and turns it back to white. 

Next we need to apply the style to our element

```css
.saving {
    animation: save-sucessful-animation 3s forwards;
}
```

And finally hook up some JavaScript to trigger it on click

```javascript
$(".trigger").click(function(event){
          var target = $(event.target).siblings(".target");
          if(target.hasClass("saveSuccessful"))
          {
          	var replacement = target.clone(true);
            target.before(replacement);
            target.remove();
            }
          else{
          	target.addClass("saveSuccessful"); 
          }
      });
```

I remove the element if the class already exists as that is the easiest way to restart the animation. (See http://css-tricks.com/restart-css-animation/)


Now when users click on the button they get a nifty little animation while the save is going on. I've ignored failure conditions here but this is going to be a big win already for my users. 

```html
<style>

@-moz-keyframes save-sucessful-animation {
    0% { background-color: white}
    25%{ background-color: #DFF2BF}
    75% {
        background-color: #DFF2BF;
    }
    100%{ background-color: white}
}

@-ms-keyframes save-sucessful-animation {
    0% { background-color: white}
    25%{ background-color: #DFF2BF}
    75% {
        background-color: #DFF2BF;
    }
    100%{ background-color: white}
}

@-webkit-keyframes save-sucessful-animation {
    0% { background-color: white}
    25%{ background-color: #DFF2BF}
    75% {
        background-color: #DFF2BF;
    }
    100%{ background-color: white}
}

@keyframes save-sucessful-animation {
    0% { background-color: white}
    25%{ background-color: #DFF2BF;}
    75% {
        background-color: #DFF2BF;
    }
    100%{ background-color: white}
}

.saveSuccessful {
    -moz-animation: save-sucessful-animation 3s forwards;
    -webkit-animation: save-sucessful-animation 3s forwards;
    -o-animation: save-sucessful-animation 3s forwards;
    animation: save-sucessful-animation 3s forwards;
}
</style>

<script>
    $(function(){ 
      $(".trigger").click(function(event){
          var target = $(event.target).siblings(".target");
          if(target.hasClass("saveSuccessful"))
          {
          	var replacement = target.clone(true);
            target.before(replacement);
            target.remove();
            }
          else{
          	target.addClass("saveSuccessful"); 
          }
      });
     });
</script>
```