---
layout: post
title: "Terraform deployment via Visual Studio Team Services."
date: 2017-05-03
category: terraform
tags: [Azure, Terraform, CI/CD, v1]

---
Infrastructure can be automatically deployed in the same way as code.

This article follows on from the *Command Line* deployment article.  We will reuse the same terraform file.

When we run terraform from VSTS we need to ensure that we maintain the state file - and its backup - in a central location.

Luckily, terraform supports storing state in blob storage.

- To enable state to be persisted to blob we need to add a provider block to main file

        main.tf

        terraform {
           backend "azurerm" {
             storage_account_name = "statefile01012"
             container_name       = "statefile01012"
             key                  = "prod.terraform.tfstate"
           }
        }

- We can then push this to github

For consistency lets push it to the GitHub repository TestWebTerraform.

- Create a new Build

![](/images/New-IaC-Release-01.png)

- Connect it to our GitHub repository

![](/images/New-IaC-Release-02.png)

- Start with an empty process 

![](/images/New-IaC-Release-03.png)

- Lets use our Azure-Hosted-Agents for performing this build

![](/images/New-IaC-Release-04.png)


- We are going to use a variable to hold the access key for our state blob container.  Lets set it.

![](/images/New-IaC-Release-05.png)


- Add a new Terraform Task to your Phase 1 - you may need to add this to your VSTS.

![](/images/New-IaC-Release-06.png)


- Configure this task as our terraform init.

You will note that we are passing a parameter into init.  This is set via an environment variable and is used to allow us to connect to our blob storage.

The parameters are:
init -backend-config="access_key=$(v_access_key)"

![](/images/New-IaC-Release-07.png)



- Create another task as our terraform plan

![](/images/New-IaC-Release-08.png)

- Create another task as our terraform apply

      apply  -auto-approve 

![](/images/New-IaC-Release-09.png)

- Lets give it a meaningful name - BuildWebInfra - and save it

- Letw queue a new build

![](/images/New-IaC-Release-10.png)

- And look at the Logs

![](/images/New-IaC-Release-11.png)

- After a successful run we can take a look at the state file.

This is equivalent to the state file which was stored on our filesystem when we ran from our command line.

![](/images/New-IaC-Release-12.png)

-  Great, all done!