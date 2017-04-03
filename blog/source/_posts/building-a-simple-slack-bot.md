---
layout: post
title: Building a Simple Slack Bot
authorId: simon_timms
date: 2015-06-07
---
A couple of friends and I have a slack channel we use to discuss deep and powerful questions like "should we make a distilled version of the ASP.net community standup that doesn't waste everybody's time?" or "could we create a startup whose business model was to create startups?". We have so many <s>terrible</s> earth-shatteringly brilliant idea we needed a place to keep them. Fortunately Trello provides just such list functionality. There is already a Trello integration for Slack but it doesn't have the ability to create cards but just notifies about changes to existing cards. 

Lame.

Thus began our quest to build a slackbot. We wanted to be able to use /commands for our bot so 

```
/trellobot add Buy a cheese factory and replace the workers with robotic rats
```

The bot should then reply to us with a link to the card should we need to fill in more details like the robot rat to worker ratio. 

We started by creating what slack call a slash integration. This means that it will respond to IRC style commands (/join, /leave, ...). This can be done from the slack webapp. Most of the fields were intuitive to full out but then we got to the one for a URL.  This is the address to which slack sends an HTTP request when it sees a slash command matching yours.  

This was a bit tricky as we were at a conference on wifi without the ability to route to our machines.  We could have set up a server in the cloud but this would slow us down iterating. So we used http://localtunnel.me/ to tunnel request to us.  What a great service!

The service was going to be simple so we opted for nodejs.  This let us get up and running without ceremony. You can build large and impressive applications also with node but I always feel it excels at rapidly iterating. It other words we just hacked some thing out and you shouldn't base your banking software on the terrible code here.

To start we needed an http server

```
  var http = require('http');
  var Uri = require('jsuri');
  
  http.createServer(function (req, res) {
  req.setEncoding('utf8');
  req.on('data', function(data){

    startResponse(res);

    var uri = new Uri();
    uri.setQuery(data);
    
    var text = uri.getQueryParamValue('text');

    var responseSettings = {
                            channelId: uri.getQueryParamValue('channel_id'),
                            userId: uri.getQueryParamValue('user_id')
                          };
    if(text.split(' ')[0] === "add")
    {
      performAdd(text, res, responseSettings);
    }
  });

}).listen(port);
console.log('Server running at http://127.0.0.1:/' + port);
```

The information passed to us by slack is URL encoded so we can just parse it out using the jsuri package. We're looking for any message that starts with "add".  When we find it we run the ```performAdd``` function giving it the message, the response to write to and the response settings extracted from the request. We want to know the channel in which the message was sent and the user who sent it so we can reply properly.  

If your bot doesn't need to reply to the whole channel and instead needs to whisper back to the person sending the command that can be done by just writing back to the response. The contents will be show in slack.

Now we need to create our Trello card. I can't help but feel that coupling a bunch of APIs together is going to be a big thing in the future. 

Trello uses OAuth to allow authentication. This is slightly problematic, as we need to have a user agree to allow our bot to interact with it as them. This is done using a prompt on a website, which we don't really have. If this was a fully-fledged bot we could find a way around it but instead we're going to take advantage of Trello permitting a key that never expires. This is kind of a security problem on their end but for our purposes it is great. 

Visit https://trello.com/1/appKey/generate and generate a key pair for Trello integration. I didn't find a need for the private one but I wrote it down anyway, might need it in the future. 

With that key visit ```https://trello.com/1/authorize?key=PUBLIC_KEY_HERE&name=APPLICATION_NAME_HERE&expiration=never&response_type=token&scope=read,write``` in a browser logged in using the account you want to use to post to Trello. The resulting key will never expire and we can use it in our application. 

We'll use this key to find the list to which we want to post. I manually ran 

```
curl "https://trello.com/1/members/my/boards?key=PUBLIC_KEY_HERE&token=TOKEN_GENERATED_ABOVE_HERE"
```

Which gave me back a list of all of the boards in my account. I searched through the content using the powerful combination of less and my powerful reading eyes finding, in short order, the ID of a board I had just created for this purpose. Using the ID of the board I wanted I ran 

```
curl "https://api.trello.com/1/boards/BOARD_ID_HERE?lists=open&list_fields=name&fields=name,desc&key=PUBLIC_KEY_HERE&token=TOKEN_GENERATED_ABOVE_HERE"
```

Again using my reading eyes I found the ID of the list in the board I wanted. (It wasn't very hard, there was only one). Now I could hard code that into the script along with all the other bits and pieces (I mentioned not writing your banking software like this, right?). I put everything into a config object, because that sounded at least a little like something a good programmer would do - hide the fact I'm using global variables by putting them in an object, stored globally. 

```
function performAdd(text, slackResponse, responseSettings){
  var pathParameters = "key=" + config.trello.key + "&token=" + config.trello.token + "&name=" + querystring.escape(text.split(' ').splice(1).join(" ")) + "&desc=&idList=" + config.trello.listId;
  var post_options = {
      host: 'api.trello.com',
      port: '443',
      path: '/1/cards?' + pathParameters,
      method: 'POST'
  };

  // Set up the request
  var post_req = https.request(post_options, function(res) {
      res.setEncoding('utf8');
      res.on('data', function (chunk) {
          var trelloResponse = JSON.parse(chunk);
          getUserProperties(responseSettings.userId, 
          					function(user){ 
                            	responseSettings.user = user; 
                                postToSlack(["/" + text, "I have created a card at " + trelloResponse.shortUrl], responseSettings)});
      });
  });
  
  // post an empty body to trello, content is in url
  post_req.write("");
  post_req.end();
  //return url
}
```

Here we send a request to Trello to create the card. Weirdly, despite the request being a POST, we put all the data in the URL. I honestly don't know why smart people like those at Trello design APIs like this...

Anyway the callback will send a message to Slack with the short URL extracted from the response from Trello. We want to make the response from the bot seem like it came from one of the people in the channel, specifically the one who sent the message. So we'll pull the user information from Trello and set the bot's name to be theirs as well as matching the icon. 

```

function postToSlack(messages, responseSettings){
console.dir(responseSettings);
  for(var i = 0; i < messages.length; i++)
  {

    var pathParameters = "username=" + responseSettings.user.name + "&icon_url=" + querystring.escape(responseSettings.user.image) + "&token=" + config.slack.token + "&channel=" + responseSettings.channelId + "&text=" + querystring.escape(messages[i]);
    var post_options = {
        host: 'slack.com',
        port: '443',
        path: '/api/chat.postMessage?' + pathParameters,
        method: 'POST'
    };

    // Set up the request
    var post_req = https.request(post_options, function(res) {
        res.setEncoding('utf8');
        res.on('data', function (chunk) {

          console.log(chunk);
        });
    });
    post_req.on('error', function(e) {
      console.log('problem with request: ' + e);
    });
    // post the data
    post_req.write("");
    post_req.end();
  }
}

function getUserProperties(userId, callback){
  var pathParameters = "user=" + userId + "&token=" + config.slack.token;
  var post_options = {
      host: 'slack.com',
      port: '443',
      path: '/api/users.info?' + pathParameters,
      method: 'GET'
  };
  var get_req = https.request(post_options, function(res) {
      res.setEncoding('utf8');
      res.on('data', function (chunk) {
        var json = JSON.parse(chunk);
        callback({name: json.user.name, image: json.user.profile.image_192});
      });
  });
  get_req.end();
}
```

This is all we need to make a Slack bot that can post a card to trello. As it turns out this was all made rather more verbose by the use of callbacks and API writers inability to grasp what the body of a POST request is for. Roll on ES7 async/await, I say. 

It should be simple to apply this same sort of logic to any number of other Slack bots.

Thanks to [Canadian James](http://jameschambers.com/) for helping me build this bot. 
