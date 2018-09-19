---
layout: post
title: "Architecture Patterns"
date: 2016-08-20
featured: true
category: Architecture
tags: [Azure, Architecture, v1]

---
You can architect solutions in Azure in a variety of different ways that align to security or availability requirements.

Here we explore the variety of different architectures  which can can be implemented.

##### Background:
There are a standard set of security controls within Azure which are enabled by default - i.e. encryption in transit for Azure Storage, Azure App Services & Azure SQL - or configurable - i.e. encryption at rest for Azure SQL - which align perfectly with most company security policies. When it comes to the use of PaaS services there is a misalignment as most security policies are based on traditional Datacentre LAN architecture.


###### 1. Standard PaaS Architecture:

![](/images/APP-SQL-PAAS.png)

![](/images/APP-PAAS-SQL-PPAAS.png)

![](/images/APP-PPAAS-SQL-PPAAS.png)


![](/images/APP-PPAAS-SQL-IAAS.png)

![](/images/APP-IAAS-SQL-IAAS.png)


![](/images/SQL-NON-REDUNDANT.png)

![](/images/SQL-IN-ZONE-REDUNDANT.png)


![](/images/SQL-ZONE-REDUNDANT.png)


![](/images/SQL-REGION-REDUNDANT.png)


![](/images/SQL-FULL-REDUNDANT.png)

