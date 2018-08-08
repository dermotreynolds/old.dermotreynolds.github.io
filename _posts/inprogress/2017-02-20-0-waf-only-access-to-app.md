---
layout: post
title: "Restricting access to your App Service to just your WAF"
date: 2018-02-15
featured: true
category: security
tags: [Azure, Web App,Web Application Firewall, v1]

---
Your App Service should only allow access from your Web Application Firewall.

A Web Application Firewall protects your web facing services from a variety of attacks.  The script below places a whitelist on an App Service to restrict connectivity.

``` javascript
$gwPublicIpName ="<Name of the public IP assigned to your gateway>"
$rgName="<Your resource group name>"
$WebAppName="<The name of your web app>"

$publicip = Get-AzureRmPublicIpAddress -Name $gwPublicIpName -ResourceGroupName $rgName
 
$r = Get-AzureRmResource -ResourceGroupName $rgName -ResourceType Microsoft.Web/sites/config -ResourceName "$WebAppName/web" -ApiVersion 2016-08-01
$p = $r.Properties
$p.ipSecurityRestrictions = @()
$restriction = @{}
$restriction.Add("ipAddress", $publicip.IpAddress)
$restriction.Add("subnetMask","255.255.255.255")
$p.ipSecurityRestrictions+= $restriction
Set-AzureRmResource -ResourceGroupName  $rgName -ResourceType Microsoft.Web/sites/config -ResourceName "$WebAppName/web" -ApiVersion 2016-08-01 -PropertyObject $p -Force  
```