---
layout: post
title: Creating a Rabbit MQ Container
authorId: simon_timms
date: 2017-03-25
---
Originally posted to the ASP.NET Monsters blog at https://aspnetmonsters.com/2017/03/2017-03-09-rabbitmq/

I bought a new laptop, a Dell XPS 15 and my oh my is it snazzy. The thing I was most excited about was that I'd get to play with Windows containers again. I have 3 other machines in the house but they're either unsuitable for containers (OSX running Windows in parallels) or I've so toally borked them playing with early betas of containers they need to be formatted and reinstalled - possibly also thrown into the sun.

So when I found myself presented with the question "how can we get into messaging in our apps for free?" I figured I'd crack open the laptop and build something with MassTransit. I found that MassTransit supports running on RabbitMQ. Why that sounds like a perfect opportunity to deploy RabbitMQ to a container. Only problem was that I didn't really know how to do that. 

<!-- more -->

In my heart I felt like running the installer wasn't quite the right way to go. I'd just copy the installation file into their destination. Problem is that RabbitMQ relies on erlang so I'd have to install that too. I build a docker file which looked something like 

```dockerfile
FROM microsoft/windowsservercore
ENV rabbitSourceDir "RabbitMQ Server"
ENV erlngDir "C:/program files/erl8.2/"
ENV rabbitDir "C:/program files/RabbitMQ Server/"
ADD ${rabbitSourceDir} ${rabbitDir}
ADD erl8.2 ${erlngDir}
ENV ERLANG_HOME "c:\program files\erl8.2\erts-8.2"
```

In the erlngDir and rabbitDir I dumped the contents of an install of erlang and rabbitmq. Then I built the container with 

`docker build -t monsters/rabbitmq .`

Didn't work. There must be something useful the installer actually does as part of installing files. So next I considered putting in the installers and running them when building the container. That seemed like a huge pain so I got to thinking about using chocolatey. At first I was pretty deadset against using choco my reasoning being that containers should be lightweight and have only one purpose. Having one time software like chocolatey on there which wouldn't ever be used seemed like it would make... whoever invented containers mad. 

So attempt number two:

```dockerfile
FROM microsoft/windowsservercore

#install chocolatey
RUN @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

#install rabbitmq
RUN choco install -y rabbitmq

```

That was enough to get Rabbit MQ installed. I still needed to expose some ports for RabbitMQ so I added 

```dockerfile
####### PORTS ########
#Main rabbitmq port
EXPOSE 5672
#port mapper daemon (epmd)
EXPOSE 4369
#inet_dist_listen
EXPOSE 35197
#rabbitmq management console
EXPOSE 15672
```

Rabbit also likes to know where Erlang lives so some environmental variables for that aren't going to hurt. 

```dockerfile
#set the home directory for erlang so rabbit can find it easily
ENV ERLANG_HOME "c:\program files\erl8.2\erts-8.2"
ENV ERLANG_SERVICE_MANAGER_PATH "c:\program files\erl8.2\erts-8.2"
```

We could forward the RabbitMQ ports to our local machine but I like the idea of using the container as if it were a distinct machine so let's also enable the management UI from anywhere on the network. To do that we'll replace the default config file with one that has 

```
{loopback_users, []},
```

in it. We can copy our new config file over the one in the container from the dockerfile and set up a variable to point Rabbit at it.

```dockerfile
ENV RABBITMQ_CONFIG_FILE "C:\rabbitmq"
COPY ["rabbitmq.config"," C:/"]
```

The config file looks like

```
[{rabbit, [{loopback_users, []}]}].
```

Finally we'll start the actual rabbit process as the default action of the container

```dockerfile
ENV RABBIT_MQ_HOME "C:\Program Files\RabbitMQ Server\rabbitmq_server-3.6.5"
CMD "${RABBIT_MQ_HOME}/sbin/rabbitmq-server.bat"
```

Now you can log into the management portal using the guest/guest account.

![Admin login](http://i.imgur.com/KvDVTb9.png)

It takes quite a while to start up the container and it took me close to 40 years to figure out building the container but it does save me installing rabbitmq on my local machine and makes experimenting with multiple instances pretty jolly easy.

The complete docker file is here:

```dockerfile
FROM microsoft/windowsservercore

####### PORTS ########
#Main rabbitmq port
EXPOSE 5672
#port mapper daemon (epmd)
EXPOSE 4369
#inet_dist_listen
EXPOSE 35197
#rabbitmq management console
EXPOSE 15672

#set the home directory for erlang so rabbit can find it easily
ENV ERLANG_HOME "c:\program files\erl8.2\erts-8.2"
ENV ERLANG_SERVICE_MANAGER_PATH "c:\program files\erl8.2\erts-8.2"

#install chocolatey
RUN @powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

#install rabbitmq
RUN choco install -y rabbitmq

#set up the path to the config file
ENV RABBITMQ_CONFIG_FILE "C:\rabbitmq"

#copy a config file over
COPY ["rabbitmq.config"," C:/"]

#set the startup command to be rabbit
CMD ["C:/Program Files/RabbitMQ Server/rabbitmq_server-3.6.5/sbin/rabbitmq-server.bat"]

```

In my next post I'll get around to actually using Rabbit MQ because all the yaks are shaved now... I hope.
