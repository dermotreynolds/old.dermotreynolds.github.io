---
layout: post
title: "Dedicated PaaS"
date: 2018-08-22
featured: true
category: Security
tags: [Azure, Architecture, v1]

---
Not all Azure PaaS Services are wide open to the internet!

A common issue with Azure PaaS services from a security perspective is that they are internet attached by default.  Whilst this may be ok for serving data which is "public domain" it is not ideal for sensitive data.

Two services which have more secure alternatives - App Services & SQL.  The variations of these can be deployed into a private vNet with no internet access if you so wish.

Some have a variation on the standard names and the specifications are given here:

Azure SQL Database Managed Instance __[here](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance){:target="_blank"}__.

App Service Environments __[here](https://docs.microsoft.com/en-us/azure/app-service/environment/intro){:target="_blank"}__.

Other examples include:
- Virtual machines: Linux or Windows
- Service fabric
- Virtual machine scale sets
- HDInsight
- RedisCache
- API Management
- VPN Gateway
- Application Gateway (internal)
- Azure Kubernetes Service (AKS)
- Azure Container Service Engine with the Azure Virtual Network CNI plug-in
- Azure Active Directory Domain Services
- Azure Batch
- Cloud services: Virtual network (classic) only

More info __[here](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-for-azure-services){:target="_blank"}__.
