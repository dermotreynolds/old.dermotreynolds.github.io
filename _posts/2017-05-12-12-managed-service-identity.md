---
layout: post
title: "Azure DevOps: #12 Application Identity / Managed Service Identity"
date: 2017-05-18
category: security

---

Taken from documentation: *The feature provides Azure services with an automatically managed identity in Azure AD.*

For example we can create an MSI for a VM and give that VM access to a SQL database.  We can then restrict access by only assigning the roles necessary for the application to function.

Enabling MSI via terraform is very straight forward:

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