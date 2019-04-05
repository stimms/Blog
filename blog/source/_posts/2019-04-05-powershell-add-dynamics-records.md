---
layout: post
title: Add records to Dynamics 365 from PowerShell
authorId: simon_timms
date: 2019-04-05 19:00

---

Dynamics 365 is a low code solution for a variety of different businesses offered by Microsoft. It is a fully hosted solution which means that you don't really get access to the underlying database. That's a good thing (less to manage) but also a bad thing (hard to work with for power users).

In this post we'll look at how to manipulate records in the CRM using some powershell. 

<!-- more-->

The first thing you're going to need is to install the  Microsoft.Xrm.Data.PowerShell module

```powershell
Install-Module Microsoft.Xrm.Data.PowerShell
```

You'll be prompted to allow installations from the PSGallery, you should probably allow it otherwise this blog post stops being useful. 

With that module installed you can now log in

```powershell
$conn = Connect-CrmOnline -ServerUrl https://someinstance.crm.dynamics.com
```
This will prompt you for your credentials. Obviously you don't want to be prompted if you're running this in an unattended script. You can work around this by doing 

```powershell
$password=ConvertTo-SecureString "yourpassword" -AsPlainText -Force
$username="username@example.com"
$cred = new-object -typename System.Management.Automation.PSCredential ` -argumentlist $username, $password
$conn = Connect-CrmOnline -ServerUrl https://someinstance.crm.dynamics.com -Credential $cred
```

<div style="margin: 5px; background-color: #f3f3f3">
    <b style="font-weight: 600; color: black">Warning</b>
    Putting passwords in scripts is obviously not a really good idea. Be cautious and make sure you understand the implications of doing so.
</div>

With the connection stored in $conn you can now retrieve records. In this example we're pulling the program record from the Higher Ed Accelerator from Microsoft

```
(Get-CrmRecords -EntityLogicalName mshied_program -Fields "*").CrmRecords
```

This will dump the records out to powershell

```
owningbusinessunit_Property : [owningbusinessunit, Microsoft.Xrm.Sdk.EntityReference]
owningbusinessunit          :
statecode_Property          : [statecode, Microsoft.Xrm.Sdk.OptionSetValue]
statecode                   : Active
statuscode_Property         : [statuscode, Microsoft.Xrm.Sdk.OptionSetValue]
statuscode                  : Active
createdby_Property          : [createdby, Microsoft.Xrm.Sdk.EntityReference]
createdby                   : Simon Timms
mshied_name_Property        : [mshied_name, Program2]
mshied_name                 : Program2
mshied_programid_Property   : [mshied_programid, d7a873b5-b757-e911-a825-000d3a3b4b32]
mshied_programid            : d7a873b5-b757-e911-a825-000d3a3b4b32
ownerid_Property            : [ownerid, Microsoft.Xrm.Sdk.EntityReference]
ownerid                     : Simon Timms
modifiedby_Property         : [modifiedby, Microsoft.Xrm.Sdk.EntityReference]
modifiedby                  : Simon Timms
owninguser_Property         : [owninguser, Microsoft.Xrm.Sdk.EntityReference]
owninguser                  :
createdon_Property          : [createdon, 4/5/2019 3:30:23 PM]
createdon                   : 2019-04-05 9:30 AM
modifiedon_Property         : [modifiedon, 4/5/2019 3:30:23 PM]
modifiedon                  : 2019-04-05 9:30 AM
ReturnProperty_EntityName   : mshied_program
original                    : {[owningbusinessunit_Property, [owningbusinessunit,
                              Microsoft.Xrm.Sdk.EntityReference]], [owningbusinessunit, ],
                              [statecode_Property, [statecode,
                              Microsoft.Xrm.Sdk.OptionSetValue]], [statecode, Active]...}
logicalname                 : mshied_program
ReturnProperty_Id           : d7a873b5-b757-e911-a825-000d3a3b4b32
EntityReference             : Microsoft.Xrm.Sdk.EntityReference

owningbusinessunit_Property : [owningbusinessunit, Microsoft.Xrm.Sdk.EntityReference]
owningbusinessunit          :
foundry_type_Property       : [foundry_type, Microsoft.Xrm.Sdk.OptionSetValue]
foundry_type                : Certification
statecode_Property          : [statecode, Microsoft.Xrm.Sdk.OptionSetValue]
statecode                   : Active
statuscode_Property         : [statuscode, Microsoft.Xrm.Sdk.OptionSetValue]
statuscode                  : Active
createdby_Property          : [createdby, Microsoft.Xrm.Sdk.EntityReference]
createdby                   : Simon Timms
mshied_name_Property        : [mshied_name, Program the first]
mshied_name                 : Program the first
mshied_programid_Property   : [mshied_programid, 4cb70731-b557-e911-a826-000d3a3b4f42]
mshied_programid            : 4cb70731-b557-e911-a826-000d3a3b4f42
ownerid_Property            : [ownerid, Microsoft.Xrm.Sdk.EntityReference]
ownerid                     : Simon Timms
modifiedby_Property         : [modifiedby, Microsoft.Xrm.Sdk.EntityReference]
modifiedby                  : Simon Timms
owninguser_Property         : [owninguser, Microsoft.Xrm.Sdk.EntityReference]
owninguser                  :
createdon_Property          : [createdon, 4/5/2019 3:12:14 PM]
createdon                   : 2019-04-05 9:12 AM
mshied_code_Property        : [mshied_code, 123456]
mshied_code                 : 123456
modifiedon_Property         : [modifiedon, 4/5/2019 3:16:17 PM]
modifiedon                  : 2019-04-05 9:16 AM
ReturnProperty_EntityName   : mshied_program
original                    : {[owningbusinessunit_Property, [owningbusinessunit,
                              Microsoft.Xrm.Sdk.EntityReference]], [owningbusinessunit, ],
                              [foundry_type_Property, [foundry_type,
                              Microsoft.Xrm.Sdk.OptionSetValue]], [foundry_type,
                              Certification]...}
logicalname                 : mshied_program
ReturnProperty_Id           : 4cb70731-b557-e911-a826-000d3a3b4f42
EntityReference             : Microsoft.Xrm.Sdk.EntityReference
```
# Update a Record

Now let's try updating a record

```powershell
Set-CrmRecord -conn $conn -EntityLogicalName mshied_program -Fields @{"mshied_name"="Doctor of Dinosaurs(awesome)"} -Id 4cb70731-b557-e911-a826-000d3a3b4f42

```

Boom, record updated. Equally we can create a new record 

# Create a Record

```powershell
New-CrmRecord -conn $conn -EntityLogicalName mshied_program -Fields @{"mshied_name"="Professor of Puddles(jumping)"} 
```

From that you'll get an id back of the newly created record. 

# Export Records

Exporting records to a csv file for later import is just a variation on what we've already done. 

```
(Get-CrmRecords -EntityLogicalName mshied_program -Fields "*").CrmRecords | Export-Csv crmtest.csv
```


# Import Records

I found re-importing the records to be by far the hardest thing. Some of the fields in the export can't be directly imported so I trim them out of the import.

```powershell
$records = import-csv .\crmtest.csv
$records | foreach-object {
    $hash = @{}
    $_.psobject.properties | Foreach { 
        if(-Not($_.Name -like "*_Property")){
            $hash[$_.Name] = $_.Value 
        }
    }
    #remove a bunch of stuff which causes errors
    $hash.Remove("ReturnProperty_Id")
    $hash.Remove("ReturnProperty_EntityName")
    $hash.Remove("original")
    $hash.Remove("EntityReference")
    $hash.Remove("mshied_programid")
    $hash.Remove("logicalname")
    $hash.Remove("owningbusinessunit")
    $hash.Remove("owninguser")
    $hash.Remove("ownerid")
    $hash.Remove("modifiedby")
    $hash.Remove("createdby")

    New-CrmRecord -conn $conn -EntityLogicalName mshied_program -Fields $hash
}

```