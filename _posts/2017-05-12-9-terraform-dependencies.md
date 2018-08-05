---
layout: post
title: "Azure DevOps: #9 Dealing with dependencies"
date: 2017-05-04
category: terraform
tags: [Azure, Terraform, CI/CD, v0.9]

---

You may have situations where you want to use the result or output of one resource in another resource.

An example of this is setting the Key Vault access policy for a new App Service that you are creating.

The chicken and egg is how do you know what the service principle is for a a resource that you havent created?

You will get errors like this:

      terraform plan
      * data.azurerm_azuread_service_principal.function_app_service_principle: data.azurerm_azuread_service_principal.function_app_service_principle: A Service Principal with the Display Name "wfecomprd2020" was not found


This is where dependencies come in.  You can use these to tell terraform not to worry about these resource until the prerequisites have been built.

For example assume we are creating an app service and want to use the service principle to give it access to keyvault

    resource "azurerm_function_app" "wfbill_function_app" {
    ...
    }

    data "azurerm_azuread_service_principal" "function_app_service_principle" {
        display_name = "${var.organisation}${var.department}${var.environment}${var.project}"

    }

    resource "azurerm_key_vault_access_policy" "wfbill_app_policy" {
    ...
    object_id = "${data.azurerm_azuread_service_principal.function_app_service_principle.id}"
    }

This would give us the above error.

Adding the *depends_on* fixes this.

    resource "azurerm_function_app" "wfbill_function_app" {
      ...
    }

    data "azurerm_azuread_service_principal" "function_app_service_principle" {
        display_name = "${var.organisation}${var.department}${var.environment}${var.project}"

        ==depends_on = ["azurerm_function_app.wfbill_function_app"]==
    }

    resource "azurerm_key_vault_access_policy" "wfbill_app_policy" {
      ...
      object_id = "${data.azurerm_azuread_service_principal.function_app_service_principle.id}"
    }
