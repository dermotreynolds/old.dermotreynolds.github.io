---
layout: post
title: "Azure DevOps: #15 Putting it together Function Infrastructure Deployment"
date: 2017-05-28
category: terraform
tags: [Azure, Key Vault, Function App, Service Principle, CI/CD, v0.9]
---
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



