---
layout: post
title: "Desired State Configuration"
date: 2018-06-20
featured: true
category: DSC
tags: [Azure, Desired State Configuration, v1]

---
Azure Automation and Desired State Configuration is an ideal way to get an newly created server into a specific state.

In this scenario we will walk through getting a newly created Windows Server into an IIS server ready to receive a build via Web Deploy.

##### 1. Create An Azure Automation Account

An azure automation account is the service under which a DSC is applied to servers within your infrastructure.  Servers can be both local to Azure and remote i.e. On-Prem.

This is very straight forward and there is a quick link __[here](https://docs.microsoft.com/en-us/azure/automation/automation-quickstart-create-account){:target="_blank"}__.

##### 2. Add the Chocolatey package manager module to our Automation Account.

Chocolatey is a package manger for Windows - similar to apt for Debian or zypper for Suse - and we are going to use it to install WebDeploy.

###### 2.1 Select Module gallery in the left hand side of your Automation Account

###### 2.2 In the search box enter "cChoco"

###### 2.2 Select the "cChoco" module and select "Import"

This will now make the Module appear in the Modules area of your Automation Account.

##### 3.  We now need to add our DSC resource script

We are going to deploy the resource script below.

This is very straight forward and there is a quick link __[here](https://docs.microsoft.com/en-us/azure/automation/automation-quickstart-dsc-configuration){:target="_blank"}__.


``` javascript

Configuration FullIIS
{

    Import-DscResource -ModuleName cChoco

    Node "localhost"
    {

        #Install the IIS Role
        WindowsFeature IIS
        {
            Ensure = "Present"
            Name = "Web-Server"
        }

        #Install ASP.NET 4.5
        WindowsFeature ASP
        {
            Ensure = "Present"
            Name = "Web-Asp-Net45"
        }

        #Add the Management Console
        WindowsFeature WebServerManagementConsole
        {
            Name = "Web-Mgmt-Console"
            Ensure = "Present"
        }

        #Add the Management Service
        WindowsFeature WebServerManagementService
        {
            Name = "Web-Mgmt-Service"
            Ensure = "Present"
        }

        LocalConfigurationManager
        {
            DebugMode = 'ForceModuleImport'
        }

        #Install our package manager
        cChocoInstaller installChoco
        {
        InstallDir = "c:\choco"
        }

        #Install WebDeploy
        cChocoPackageInstaller installWebDeploy
        {
        Name        = "webdeploy"
        Params      = "-y"
        DependsOn   = @('[cChocoInstaller]installChoco','[WindowsFeature]IIS','[WindowsFeature]ASP','[WindowsFeature]WebServerManagementConsole','[WindowsFeature]WebServerManagementService')
        #This will automatically try to upgrade if available, only if a version is not explicitly specified.
        AutoUpgrade = $True
        }

        #Enable the management service to start automatically
        Service StartWMSVC {
            Name = 'WMSVC'
            StartupType = 'Automatic'
            State = 'Running'
            DependsOn = '[cChocoPackageInstaller]installWebDeploy'
       }

    }
}
```

##### 4.  We can now register one of our servers with DSC.

The mini script below will install the Microsoft.Powershell.DSC extension on the VM, register the VM with DSC and apply the above configuration to it.

``` javascript

Connect-AzureRmAccount

Register-AzureRmAutomationDscNode -AzureVMName <VMName> -AzureVMResourceGroup <VMResourceGroup> -NodeConfigurationName FullIIS.localhost -AutomationAccountName <Automation account name -ResourceGroupName <Automation resource group> -ConfigurationMode ApplyAndAutocorrect
```

##### 5.  Last but not least

In order to Web Deploy to the above server we need to open up ports 8172 - required for Web Deploy - and 80 - required by our deployed app
