---
layout: post
title: "Terraform deployment via command line"
date: 2017-05-01
category: terraform
tags: [Azure, Terraform, CI/CD, v1]

---
The GUI should only be used for those functions that are not available via command line means.

There are a number of issues with using the GUI and ad-hoc commands:

1. There is no documented state of the infrastructure.

2. There is no repeatability.  Most organisations have multiple environments which are identical to one another apart from scale.  Maintaining this through manual means is difficult.

Infrastructure As Code (IaC) is a means by which the intended state of an infrastructure can be codified.  This give us our documentation.  It can be used to deployed to multiple environments and committed to source control, giving us repeatability and audit.

The purpose of this article is to walk you through a very simple terraform deployment via the command line and then use the same script in a subsequent automated deployment via Visual Studio Team Services.

- Create your terraform file
~~~~~~
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
~~~~~~
- Login to azure
~~~~~~
az login
~~~~~~
- Run the following commands via the command line
~~~~~~
#This will install any modules
terraform init

#This will show you the changes that will be deployed
terraform plan

#This will make the actual change
terraform apply
~~~~~~
- Terraform plan will show you what it will create/remove
~~~~~~
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
~~~~~~
- Terraform apply has completed and created 2 state files - well one and its backup.
~~~~~~
Apply complete! Resources: 3 added, 0 changed, 0 destroyed.

$ ls terraform.tfstate*
terraform.tfstate		terraform.tfstate.backup
~~~~~~

- To tidy up
~~~~~~
terraform destroy

...
...
azurerm_resource_group.nft_resource_group: Still destroying... (ID: /subscriptions/bc0b3b15-7d6c-4fa4-b3a7-d208b08fde5c/resourceGroups/TestWebAppRG, 40s elapsed)azurerm_resource_group.nft_resource_group: Destruction complete after 46s

Destroy complete! Resources: 3 destroyed.
~~~~~~
