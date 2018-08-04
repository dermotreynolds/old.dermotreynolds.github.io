---
layout: post
title: "What is a Managed Service Identity?"
date: 2017-05-18
category: security

---
A Managed Service Identity creates an automatically managed identity in Azure AD.

This identity can then be used to provide access to resources.

For example we can create an MSI for a VM and give that VM access to a SQL database.  We can then restrict access by only assigning the roles necessary for the application to function.

You can assign an MSI to a range of resources.  At the time of writing these were:

- Azure Data Lake
- Azure SQL
- Virtual machines - Linux & Windows
- Azure App Service
- Azure Functions
- Azure Service Bus
- Azure Event Hubs
- Azure API Management

Within VM you can enable this feature by selecting Configuration and selecting "Yes":

![](/images/Enable-VM-MSI-01.png)

You can see it what is created by via Powershell:

{% highlight yaml %}
Get-AzureRmADServicePrincipal -DisplayName vstsagentvm1
ServicePrincipalNames : {4d266yyy-XXXX-49b2-b637-Nfd0cNNNfff,
https://identity.azure.net/adf90adfadfde=}
ApplicationId         : 4d266yyy-XXXX-49b2-b637-Nfd0cNNNfff
DisplayName           : vstsagentvm1
Id                    : 4d266yyy-XXXX-49b2-b637-Nfd0cNNNfff
Type                  : ServicePrincipal

{% endhighlight %}

You can now use IAM to give permissions to the MSI to resources:

![](/images/Enable-VM-MSI-02.png)

You can enable MSI via terraform:

    resource "azurerm_function_app" "wfbill_function_app" {
        ...

        identity {
            type = "SystemAssigned"
        }
        ...
    }


We can then use the service principle to define a policy against keyvault: 

    resource "azurerm_key_vault_access_policy" "wfbill_app_policy" {
        vault_name          = "<key vault name>"
        resource_group_name = "<key vault resource group>"
        tenant_id = "<your tenant id>"
        object_id = "<the app service principle id>"

        key_permissions = [<permissions>]
        secret_permissions = [<permissions>]

    }

Your app service can now access key vault.