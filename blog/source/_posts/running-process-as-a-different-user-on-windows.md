---
layout: post
title: Running Process As A Different User On Windows
authorId: simon_timms
date: 2015-09-02
---
As part of a build pipeline I'm working on the octopus deploy process needs to talk to the database using roundhouse as a different user from the one running the deployment. This is done because the database uses integrated AD authentication, which I quite like. If this build were running on Linux then it would be as simple as editing the sudoers file and calling the command using sudo. Unfortunatly this is Windows and the command line has long been a secondary concern. 

I started by asking on the [western devs](http://westerndevs.com) slack channel to see if anybody else had done this and how. [Dave Paquette](http://www.westerndevs.com/bios/dave_paquette/) suggested using [psexec](https://technet.microsoft.com/en-us/sysinternals/bb897553.aspx). This is a tool designed for running commands on a remote computer but if you leave the computer name off it will run on the local machine. This sounded perfect. 

However I had a great deal of trouble getting psexec to work in the way I wanted. The command I wanted to run seemed to fail all the time giving an confusing error code [-1073741502](https://social.technet.microsoft.com/forums/en-US/db8ce5e5-988c-4f1c-93f4-5ff1f2fa29e8/psexec-on-remote-server-giving-program-exit-code1073741502). The fix provided didn't seem to work for me so after an afternoon of bashing my head against psexec I went looking for another solution. Running remote processes gave me an idea: what about powershell remoting?

Some investigation suggested that the command I wanted to run would look like

	Invoke-command localhost -scriptblock { rh.exe --some-parameters }

This would remote to localhost and run the roundhouse command as the current user. To get it to work using a different user then the command needed credentials passed into it. I had the credentials stored as [sensitive variables](http://docs.octopusdeploy.com/display/OD/Sensitive+variables) in Octopus which set them up as variables in powershell. To turn these into credentials you need to do
    
	$pwd = ConvertTo-SecureString $deployPassword -asplaintext -force
    $cred =new-object -TypeName System.Management.Automation.PSCredential -argumentlist $deployUser,$pwd

Now these can be passed into invoke command as 

    Invoke-command localhost -authentication credssp -Credential $cred -scriptblock { rh.exe --some-parameters }

You might notice that authentication flag, this tells powershell the sort of authentication and cor credssp you also need to enable [Credential Security Service Provider](http://blogs.technet.com/b/heyscriptingguy/archive/2012/11/14/enable-powershell-quot-second-hop-quot-functionality-with-credssp.aspx). To do this we run

    Enable-WSManCredSSP -Role server
    Enable-WSManCredSSP -Role client -DelegateComputer "*"
From an admin powershell session on the machine. Normall you would run these on different machines but we're remoting to local host so it is both the client and the server. 

Finally I needed to pass some parameters to roundhouse proper. 

    Invoke-command localhost -authentication credssp -Credential $cred -scriptblock { param($roundHouseExe,$databaseServer,$targetDb,$databaseEnvironment,$fName) & "$roundHouseExe" --s=$databaseServer --d="${targetDb}"  --f=$fName /ni --drop } -argumentlist $roundHouseExe,$databaseServer,$targetDb,$databaseEnvironment,$fName
