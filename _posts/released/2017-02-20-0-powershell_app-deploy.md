---
layout: post
title: "Deploying to App Services Deployment Slot with Powershell"
date: 2017-02-20
featured: true
category: powershell
tags: [Azure, Powershell,App Service, v1]

---
You can set up Continuous Deployment by connecting an App Service directly to a source code repository.

Below is a Powershell script that spins up an App Service and connects it to a Git Repository.

It also swaps the deployment slot to make the app live.

One of the great things about Azure is that Powershell can do pretty much anything...and it runs on Mac!

##### 1. Create some variables.  Set the URL to that of your app.

``` javascript

$gitrepo="https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git"
$webappname="mywebapp$(Get-Random)"
$location="West Europe"
$rgname="WeFinance"
```

##### 2. Create a resource group.

``` javascript
# Create a resource group.
New-AzureRmResourceGroup -Name $rgname -Location $location
```

#### 3. Create an App Service plan in Free tier.

``` javascript
New-AzureRmAppServicePlan -Name $webappname -Location $location `
-ResourceGroupName $rgname -Tier Free
```

#### 4. Create a web app.

``` javascript
New-AzureRmWebApp -Name $webappname -Location $location `
-AppServicePlan $webappname -ResourceGroupName $rgname
```

#### 5. Upgrade App Service plan to Standard tier (minimum required by deployment slots)

``` javascript
Set-AzureRmAppServicePlan -Name $webappname -ResourceGroupName $rgname `
-Tier Standard
```
#### 6. Create a deployment slot with the name "staging".

``` javascript
New-AzureRmWebAppSlot -Name $webappname -ResourceGroupName $rgname `
-Slot staging -AppServicePlan $webappname
```

#### 7. Configure GitHub deployment to the staging slot from your GitHub repo and deploy once.

``` javascript
$PropertiesObject = @{
    repoUrl = "$gitrepo";
    branch = "master";
    isManualIntegration = "true";
}

Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $rgname `
-ResourceType Microsoft.Web/sites/slots/sourcecontrols `
-ResourceName $webappname/staging/web -ApiVersion 2015-08-01 -Force
```

#### 8. Swap the verified/warmed up staging slot into production.

``` javascript
Switch-AzureRmWebAppSlot -Name $webappname -ResourceGroupName $rgname `
-SourceSlotName staging -DestinationSlotName production
```
