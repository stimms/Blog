---
layout: post
title: Running MiniKube on an Azure VM
authorId: simon_timms
date: 2019-05-13 19:00
draft: true
---

MiniKube is a Kubernetes distribution designed to make running a single node Kubernetes cluster easy for local development. However MiniKube relies on docker which relies on you having a Professional version of Windows on your laptop. If you don't have that then it is really easy to stand up an Azure VM which can run MiniKube.

<!-- more -->

The first step is to log into the Azure portal and head over to Virtual Machines. You're going to want a VM with a little bit of power too it so I recommend a D2s v3 or a D4s v3. That v3 part is important because you want a VM which can support nested virtualization. 

Next pick an operating system. I like Ubuntu so I went with that.

#kubectl

The first step is to install kubectl which can control your Kubernetes cluster. In an ssh session run 

```
sudo snap install kubectl --classic
```

Now kubectl is installed. 

#MiniKube

Download MiniKube and chmod it so you can run it.

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64   && chmod +x minikube
```

# Virtual Box

Now go ahead and install Virtual Box to be your virtual machine hypervisor. I know it suck that Oracle own it but hold your nose.

```
sudo apt-get install virtualbox
```


# Starting up

Wit MiniKube in the current directory you can start it up with 

```
./minikube start
```

It will churn for a little while during the first install as it downloads VM images and sets everything up. 

Now you can run kubectl commands as if you were pointing at a huge K8S cluster. 
