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

~~~~~~
# Replace the following URL with a public GitHub repo URL
$gitrepo="https://github.com/Azure-Samples/app-service-web-dotnet-get-started.git"
$webappname="mywebapp$(Get-Random)"
$location="West Europe"
$rgname="WeFinance"

# Create a resource group.
New-AzureRmResourceGroup -Name $rgname -Location $location

# Create an App Service plan in Free tier.
New-AzureRmAppServicePlan -Name $webappname -Location $location `
-ResourceGroupName $rgname -Tier Free

# Create a web app.
New-AzureRmWebApp -Name $webappname -Location $location `
-AppServicePlan $webappname -ResourceGroupName $rgname

# Upgrade App Service plan to Standard tier (minimum required by deployment slots)
Set-AzureRmAppServicePlan -Name $webappname -ResourceGroupName $rgname `
-Tier Standard

#Create a deployment slot with the name "staging".
New-AzureRmWebAppSlot -Name $webappname -ResourceGroupName $rgname `
-Slot staging -AppServicePlan $webappname

# Configure GitHub deployment to the staging slot from your GitHub repo and deploy once.
$PropertiesObject = @{
    repoUrl = "$gitrepo";
    branch = "master";
    isManualIntegration = "true";
}
Set-AzureRmResource -PropertyObject $PropertiesObject -ResourceGroupName $rgname `
-ResourceType Microsoft.Web/sites/slots/sourcecontrols `
-ResourceName $webappname/staging/web -ApiVersion 2015-08-01 -Force

# Swap the verified/warmed up staging slot into production.
Switch-AzureRmWebAppSlot -Name $webappname -ResourceGroupName $rgname `
-SourceSlotName staging -DestinationSlotName production
~~~~~~
