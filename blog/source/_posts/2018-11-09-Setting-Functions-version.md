layout: post
title: Setting the version of Azure functions from ARM
authorId: simon_timms
date: 2018-11-09
---

Need to set the version of Azure functions runtime from an ARM template? Weirdly it is actually an app setting and not something on the site. The variable is called `FUNCTIONS_EXTENSION_VERSION` and can take on the values `~1` or `~2`. In an ARM template it looks like 

```javascript
{
    "type": "Microsoft.Web/sites",
    "kind": "functionapp",
    "name": "[variables('webSiteName')]",
    "tags": {
        "displayName": "Site"
    },
    "apiVersion": "2016-08-01",
    "location": "[resourceGroup().location]",
    "scale": null,
    "resources": [
    {
        "name": "appsettings",
        "type": "config",
        "apiVersion": "2015-08-01",
        "dependsOn": [ "[concat('Microsoft.Web/sites/', variables('webSiteName'))]" ],
        "tags": {
        "displayName": "App settings"
        },
        "properties": {
        "FUNCTIONS_EXTENSION_VERSION": "~2",
        }
    }
    ],...
}
```