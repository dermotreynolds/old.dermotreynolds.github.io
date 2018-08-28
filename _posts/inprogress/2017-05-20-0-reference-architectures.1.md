---
layout: post
title: "Dedicated PaaS"
date: 2018-08-22
featured: true
category: Security
tags: [Azure, Architecture, v1]

---
Not All Azure PaaS Services Are Shared!

One common issue with Azure PaaS services from a security perspective is that they are internet attached by default.  Whilst this may be ok for serving data which is "public domain" it is not ideal for sensitive data.

There are 2 services which have more secure alternatives - App Services & SQL.  The variations of these can be deployed into a private vNet with no internet access if you so wish.

They have a variation on the standard names and the specifications are given here:

Azure SQL Database Managed Instance __[here](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance){:target="_blank"}__.

App Service Environments __[here](https://docs.microsoft.com/en-us/azure/app-service/environment/intro){:target="_blank"}__.