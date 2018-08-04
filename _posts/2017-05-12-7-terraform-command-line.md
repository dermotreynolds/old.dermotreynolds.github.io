---
layout: post
title: "Azure DevOps: #7 Terraform deployment via command line."
date: 2017-05-01
category: terraform

---

So far the provisioning of infrastructure was manual through the portal.

There are a number of issues with this:

1. There is no documentation of the state of the infrastructure.

2. There is no repeatability.  Most organisations have multiple environments which are identical to one another apart from scale.  Maintaining through manual means is difficult.

3. There is no repeatable means by which the security posture can be maintained - apart from rigorous manual reviews.

Infrastructure As Code(IaC) is a means by which the intended state of an infrastructure can be codified.

We will first show how this can be achieved through the command line and then do so via Visual Studio Team Services.

-  Create your terraform file

        #Create a resource group to put our resources into
        resource "azurerm_resource_group" "nft_resource_group" {
        name     = "TestWebAppRG"
        location = "West Europe"
        }

        #Create an App Service Plan
        resource "azurerm_app_service_plan" "nft_app_service_plan" {
        name                = "some-app-service-plan"
        location            = "${azurerm_resource_group.nft_resource_group.location}"
        resource_group_name = "${azurerm_resource_group.nft_resource_group.name}"

        sku {
            tier = "Standard"
            size = "S1"
        }
        }

        #Create an App Service
        resource "azurerm_app_service" "nft_app_service" {
        name                = "TestWebApp0101"
        location            = "${azurerm_resource_group.nft_resource_group.location}"
        resource_group_name = "${azurerm_resource_group.nft_resource_group.name}"
        app_service_plan_id = "${azurerm_app_service_plan.nft_app_service_plan.id}"

        site_config {
            default_documents = [
            "index.html",
            ]
        }
        }

-   Firstly, login to azure

        az login

-   Run it via the command line

        #This will install any modules
        terraform init

        #This will show you the changes that will be deployed
        terraform plan

        #This will make the actual change
        terraform apply

-  Terraform plan will show you what it will create/remove

        Terraform will perform the following actions:

        + azurerm_app_service.nft_app_service
            id:                                      <computed>
            app_service_plan_id:                     "${azurerm_app_service_plan.nft_app_service_plan.id}"
            app_settings.%:                          <computed>
            client_affinity_enabled:                 <computed>
            connection_string.#:                     <computed>
            default_site_hostname:                   <computed>
            enabled:                                 "true"
        ...
        ...
        Plan: 3 to add, 0 to change, 0 to destroy.
- Terraform apply has completed and created 2 state files - well one and its backup.

        Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

        $ ls terraform.tfstate*
        terraform.tfstate		terraform.tfstate.backup


- To tidy up

      terraform destroy

      ...
      ...
      azurerm_resource_group.nft_resource_group: Still destroying... (ID: /subscriptions/bc0b3b15-7d6c-4fa4-b3a7-d208b08fde5c/resourceGroups/TestWebAppRG, 40s elapsed)azurerm_resource_group.nft_resource_group: Destruction complete after 46s

      Destroy complete! Resources: 3 destroyed.

