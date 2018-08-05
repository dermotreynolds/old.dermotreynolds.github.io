---
layout: post
title: "Terraform/Key Vault/Function Infrastructure Deployment"
date: 2017-05-28
category: terraform
tags: [Azure, Key Vault, Function App, Service Principle, CI/CD, v0.9]
---
This article will walk you through setting up the infrastructure required for a Function App.


##### 1. Persist our state to Azure Blob storage

In order to persist our state to Azure Storage we need to include an AzureRm block.

``` javascript
terraform {
  backend "azurerm" {
    storage_account_name = "wfinfraprd010105"
    container_name       = "wfinfraprd010105"
    key                  = "terraform.polbill.state"
  }
}
```

If you have been running from the command line then you can add the above block and then run the terraform init as follows 

    terraform init -backend-config="access_key=<Azure Storage Key>)"

This will put your state into the remote location.


##### 2. Lets specify the version of the AzureRm Terraform module that we want to use

Given that terraform is an evolving project it is always a good idea to specify the AzureRm version that you are working with.  You can lock down the version as follows:

``` javascript

provider "azurerm" {
  version = "~> 1.11.0"
}
```

##### 3. Create a resource group to put all of our new services into

It is good practice to put all the resources for a solution into the same resource group.  A resource group is simply a logical container but you can assign policies and security to it at a later date.

``` javascript

resource "azurerm_resource_group" "wfbill_resource_group" {
  name     = "${var.organisation}-${var.department}-${var.environment}-${var.project}"
  location = "${var.azure_location}"

  tags {
    environment  = "${var.environment}"
    department   = "${var.department}"
    organisation = "${var.organisation}"
  }
}
```

##### 4. Create a Key Vault instance where we will store our secrets

In this instance we are going to put our Azure Storage connect string into a Key Vault.  Using Key Vault in this way ensures that:

- There is no human intervention required to setup connectivity.
- As there is no human intervention the risk of credentials being lost is minimised.

``` javascript
data "azurerm_client_config" "current" {}

resource "azurerm_key_vault" "wfcore_key_vault" {
  name                = "${var.organisation}${var.department}${var.environment}-core"
  location            = "${azurerm_resource_group.wfbill_resource_group.location}"
  resource_group_name = "${azurerm_resource_group.wfbill_resource_group.name}"
  tenant_id           = "${data.azurerm_client_config.current.tenant_id}"

  access_policy {
    tenant_id = "${data.azurerm_client_config.current.tenant_id}"
    object_id = "${data.azurerm_client_config.current.service_principal_object_id}"

    key_permissions = [
      "create",
      "get",
      "list",
      "backup",
      "decrypt",
      "delete",
      "encrypt",
      "get",
      "import",
      "list",
      "purge",
      "recover",
      "restore",
      "sign",
      "unwrapKey",
      "update",
      "verify",
      "wrapKey",
    ]

    secret_permissions = [
      "backup",
      "delete",
      "get",
      "list",
      "purge",
      "recover",
      "set",
      "restore",
    ]
  }

  sku {
    name = "standard"
  }
}
```

##### 5. Create a storage account where our Function App will store its data

``` javascript
resource "azurerm_storage_account" "wfbill_storage_account" {
  name                     = "${var.organisation}${var.department}${var.environment}${var.project}"
  resource_group_name      = "${azurerm_resource_group.wfbill_resource_group.name}"
  location                 = "${azurerm_resource_group.wfbill_resource_group.location}"
  account_tier             = "Standard"
  account_replication_type = "GRS"

  tags {
    environment  = "${var.environment}"
    department   = "${var.department}"
    organisation = "${var.organisation}"
  }
}
```

##### 5. Save our connection string to Key Vault

``` javascript
resource "azurerm_key_vault_secret" "wfbill_store_accesskey" {
  name      = "${var.organisation}${var.department}${var.environment}${var.project}-accesskey"
  value     = "${azurerm_storage_account.wfbill_storage_account.primary_connection_string}"
  vault_uri = "${azurerm_key_vault.wfcore_key_vault.vault_uri}"

  tags {
    environment  = "${var.environment}"
    department   = "${var.department}"
    organisation = "${var.organisation}"
  }
}
```

##### 6. Create an App Service Plan
``` javascript
resource "azurerm_app_service_plan" "wfbill_app_service_plan" {
  name                = "${var.organisation}${var.department}${var.environment}${var.project}"
  location            = "${azurerm_resource_group.wfbill_resource_group.location}"
  resource_group_name = "${azurerm_resource_group.wfbill_resource_group.name}"

  sku {
    tier = "Standard"
    size = "S1"
  }
}
```

##### 7. Create a Function App
``` javascript
resource "azurerm_function_app" "wfbill_function_app" {
  name                      = "${var.organisation}${var.department}${var.environment}${var.project}"
  location                  = "${azurerm_resource_group.wfbill_resource_group.location}"
  resource_group_name       = "${azurerm_resource_group.wfbill_resource_group.name}"
  app_service_plan_id       = "${azurerm_app_service_plan.wfbill_app_service_plan.id}"
  storage_connection_string = "${azurerm_storage_account.wfbill_storage_account.primary_connection_string}"

  identity {
    type = "SystemAssigned"
  }

  app_settings {
    "KeyVaultLocation" = "${azurerm_key_vault.wfcore_key_vault.vault_uri}"
  }
}
```

##### 8. Give our new Function App access to Key Vault via Policy

``` javascript
data "azurerm_client_config" "wfbill_client_config" {}

resource "azurerm_key_vault_access_policy" "wfbill_app_policy" {
  vault_name          = "${azurerm_key_vault.wfcore_key_vault.name}"
  resource_group_name = "${azurerm_key_vault.wfcore_key_vault.resource_group_name}"

  tenant_id = "${data.azurerm_client_config.wfbill_client_config.tenant_id}"
  object_id = "${azurerm_function_app.wfbill_function_app.identity.0.principal_id}"

  key_permissions = []

  secret_permissions = [
    "backup",
    "delete",
    "get",
    "list",
    "purge",
    "recover",
    "set",
    "restore",
  ]

  depends_on = ["azurerm_function_app.wfbill_function_app"]
}
```

##### 9. 
~~~~~~
~~~~~~


We are going to start off with a very simple initial architecture which will:

1. Store secrets in Key Vault.

2. Create an Function App.

3. Use table storage for persisting data.

As the infrastructure is being deployed:

1. The Service Principle that is used for deploying the infrastructure is given access to Key Vault via an access policy.

2. The connection string for the storage is persisted to the Key Vault instance.

3. The Function App is given an Managed Service Identity - see specific article on this.

4. The Function App is then given access to Key Vault - via an access policy.

5. The Key Vault URI is set as a parameter in the Function App.

The application can now, find the Key Vault, access the secret containing the connection string and then use this string to persist data.

The plan is that I get more time I will add more features to the solution.

The first thing that we need to do is to state where we are going to persist our state.

        #Persist our state to blob storage
        terraform {
        backend "azurerm" {
            storage_account_name = "wfinfraprd010105"
            container_name       = "wfinfraprd010105"
            key                  = "terraform.polbill.state"
        }
        }

We then need to create a Resource Group to put all of our services into.  
A Resource Group is a logical container.  A good practice is to put all the unique elements for an application to function into the same one.

        #Create a resource group to put our resources into
        resource "azurerm_resource_group" "wfbill_resource_group" {
            name     = "${var.organisation}-${var.department}-${var.environment}-${var.project}"
            location = "${var.azure_location}"

            tags {
                environment  = "${var.environment}"
                department   = "${var.department}"
                organisation = "${var.organisation}"
            }
        }

We can the create a Key Vault. To allow our Service Princple to write secrets to this we need to configure an access policy for it.

We need to use a Data Source in order to get our tenant_id and service_principle_object_id.  The latter is only available if we are using a Service Principle to connect to the resource - which is the case if we are using VSTS to perform this deployment.


        data "azurerm_client_config" "current" {}

        resource "azurerm_key_vault" "wfecom_key_vault" {
        name                = "<Key Vault Name>"
        location            = "<Region>"
        resource_group_name = "<Resource Group Name>"
        tenant_id           = "${data.azurerm_client_config.current.tenant_id}"

        access_policy {
            tenant_id = "${data.azurerm_client_config.current.tenant_id}"
            object_id = "${data.azurerm_client_config.current.service_principal_object_id}"

            key_permissions = [
            "create",
            "get",
            "list",
            "backup",
            "decrypt",
            "delete",
            "encrypt",
            "get",
            "import",
            "list",
            "purge",
            "recover",
            "restore",
            "sign",
            "unwrapKey",
            "update",
            "verify",
            "wrapKey",
            ]

            secret_permissions = [
            "backup",
            "delete",
            "get",
            "list",
            "purge",
            "recover",
            "set",
            "restore",
            ]
        }

        sku {
            name = "standard"
        }
        }

In our case we are not going to use the Hardware Security Module version and instead opt for the Standard version.













# #Create a local storage account for our application



#Get a reference to the keyvault as we want to push the storage connection string to it
# data "azurerm_key_vault" "wfcore_key_vault" {
#   name                = "wfinfraprd-core"
#   resource_group_name = "wf-infra-prd-core"
# }

#Save the storage connection string to the key vault


# #Ceate an app service plan


# #Create a function app which is registered with Azure AD


# #Get a handle to the current client, so that we can get the tenant_id
