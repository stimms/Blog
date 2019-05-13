---
layout: post
title: Running MiniKube on an Azure VM
authorId: simon_timms
date: 2019-05-10 19:00
draft: true
---

MiniKube is a Kubernetes distribution designed to make running a single node Kubernetes cluster easy for local development. However MiniKube relies on docker which relies on you having a Professional version of Windows on your laptop. If you don't have that then it is really easy to stand up an Azure VM which can run MiniKube.

<!-- more -->

The first step is to log into the Azure portal and head over to Virtual Machines. You're going to want a VM with a little bit of power too it so I recommend a D2s v3 or a D4s v3. That v3 part is important because you want a VM which can support nested virtualization. 

Next pick an operating system. You're best off going with Windows 10 Pro 1809 or Windows Server 2019. I'm using the former, but this may only be available if you have MSDN. You're also going to want to open up the RDP and HTTP ports. HTTP isn't wholly necessary but if you're running a website on that box it might be nice to have. Set a password and away you go.

Once the VM is created RDP into it. 

#kubectl

The first step is to install kubectl which can control your Kubernetes cluster. Open an admin powershell windows and run 

```
Set-ExecutionPolicy Unrestricted
Install-Script -Name install-kubectl -Scope CurrentUser -Force
install-kubectl.ps1 -DownloadLocation c:\bin
```

Now kubectl is installed in `c:\bin`, feel free to add it to your path.

#MiniKube

Download the installer from [here](https://github.com/kubernetes/minikube/releases/download/v1.0.1/minikube-installer.exe) and run it. That will default to installing MiniKube in an awkward location, you might want to change it to install in `c:\bin` as above.

#Hyper-V

You can chose from a number of different vm technologies for MiniKube but Hyper-V comes with Windows so let's use that. You need to install Hyper-V so go to the start menu and search `Features`. Select `Turn Windows Features On or Off`. In that select all the Hyper-V stuff including the management tools.

You'll likely need to reboot here. 

Now open up the Hyper-V management tool and add a new external virtual switch. With that enabled you can finally start MiniKube.

#Starting up

I left my MiniKube in the default directory so to start it I ran

```
& 'C:\Program Files\Kubernetes\Minikube\minikube.exe' start --vm-driver=hyperv
```

It will churn for a little while during the first install as it downloads VM images and sets up Hyper-V. 