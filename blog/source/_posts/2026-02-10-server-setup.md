---
layout: post
title: Server Setup
authorId: simon_timms
date: 2026-01-25
---

Well it took 5 days but the server is finallyl provisioned. That's super exciting and super slow. Certainly not the speed of provisioning that I've been treated to on cloud providers. But leveraging my positive attitude here: it is faster than the 6-12 month lead time I had to deal with when I was an intern. So that's a win.

Well, it took five days, but the server is finally provisioned. That's exciting — even if the process was slow. Certainly not the provisioning speed I've experienced on cloud providers, but looking on the bright side, it's faster than the 6–12 month lead time I dealt with when I was an intern. So that's a win.

Okay, now let's see how much I can get the AI to help with the setup. When I provisioned the server I went with Ubuntu, so everything from here on out is going to be Ubuntu. Ubuntu is a pretty safe bet for a server or a desktop, I guess. I will admit, when I saw FreeBSD as an option it was really tempting. Back in the day I ran FreeBSD at home, and the handbook in particular was amazing.

OVH sent me a password for the `ubuntu` user for initial login. What they didn't send me was the server fingerprint so when I logged in I just had to blindly accept it. Next I copied over my SSH key using 

```
➜  Downloads ssh-copy-id ubuntu@vps-b0f3ae73.vps.ovh.ca
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/stimms/.ssh/id_ecdsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
ubuntu@vps-b0f3ae73.vps.ovh.ca's password: 

Number of key(s) added:        1

Now try logging into the machine, with: "ssh 'ubuntu@vps-b0f3ae73.vps.ovh.ca'"
and check to make sure that only the key(s) you wanted were added.
```

Let's go ahead and install some sort of agenty thing up there so it can do things for me. Cursor would work for me but I've got some tokens for Codex hanging around so let's burn those. We need npm first which installs a hilarious 750MB of packages.

Let's go ahead and install some kind of agent up there so it can do things for me. Cursor would work, but I've got some tokens for Codex hanging around, so let's burn those. We need npm first, which installs a hilarious 750 MB of packages.

```
sudo apt install nodejs npm
sudo npm i -g @openai/codex
```

Now I can run `codex` and tell it what to do. I still approved every step, but I was nervous it was going to destroy my brand new server.

```
Can you update this machine to the latest?
```

Codex churned for a bit and ran not just the package upgrades but a whole distro update. Did it work? Yes, but it required a few reboots (what happened to not needing to reboot Linux?). Lordy, that model was slow and verbose. It probably took five times as long as it would have taken me to just run the commands myself. Also, it probably burned down a chunk of the rainforest to do that.

Let's give it something a bit more complex to do. Let's start with setting up a PostgreSQL database. This time we'll leverage `--yolo` to let it do its thing without me approving every step.

```
Install a postgresql server on this machine
```

a few moments later we're at 

```
• PostgreSQL is installed and running on this machine.

  Current state:

  - Version: PostgreSQL 17.9
  - Service: active and enabled
  - Cluster: 17/main
  - Port: 5432

  I verified it with a local psql query as the postgres system user.

  Useful commands:

  sudo -u postgres psql
  sudo systemctl status postgresql
  sudo pg_lsclusters

  If you want, I can also do the next setup step:

  - create an application database and user
  - enable remote access
  - set a password for postgres
  - install pgvector or other extensions
  ```

That's great — it didn't open a port to the world in a dangerously insecure way, so that's a win.

So far this seems like a fine approach for folks who don't necessarily know how to do this stuff themselves but want to be able to ask an agent to do it for them. It's not super fast, and it's not super efficient, but it is pretty easy. I could see this being a great way for a small business owner to set up a server without needing to hire an expert.

