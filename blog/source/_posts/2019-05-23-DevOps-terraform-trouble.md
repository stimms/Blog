---
layout: post
title: Azure DevOps Terraform Trouble
authorId: simon_timms
date: 2019-05-23 19:00

---

I'm a big fan of Azure DevOps and also of Terraform. I use the Terraform tasks to run deployments of infrastructure in a DevOps pipeline. Today my old reliable build broke because of a change to the version of Terraform.

<!-- More -->

Recently Terraform 0.12 was released and one of the changes in it was related to the validate task. This task can be used to check and make sure that your terraform template is valid. In 0.11 it was possible to pass `-input=false` to this task as you can to many Terraform tasks. However that functionality seems to have been removed in 0.12 (I didn't hunt down the reason for this). Problem is that the task in Azure DevOps passes that by default to all commands so it breaks with used with Terraform 0.12.

Now I didn't discover this problem until we needed to make a change to our environment and the build failed. We were using the `latest` version of Terraform in our builds so as soon as TF updated their latest our build was also updated. I always pitch people on the idea of running nightly build so that when something changes in your environment you catch it right away.

I didn't follow my own advice. Typical Simon.

You can select the version of Terraform to use in the task but I couldn't find what to put into that field to get it to work. Thank goodness the plugin is open source. I was able to read the code at https://github.com/XpiritBV/Xpirit-Vsts-Release-Terraform/blob/174909fa39f3d7c9d4932a73d33056bb8d110909/Xpirit-Vsts-Release-Terraform/terraform.ps1 and find that it was checking the page https://releases.hashicorp.com/terraform/ for valid releases and then stripping the 3 segment version number off as the value. By giving the build task `0.11.4` I was able to fix the build.