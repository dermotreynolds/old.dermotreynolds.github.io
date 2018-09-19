---
layout: post
title: "Architecture Patterns"
date: 2016-08-20
featured: false
category: Architecture
tags: [Azure, Architecture, v1]

---
You can architect solutions in Azure in a variety of different ways that align to security or availability requirements.

Here we explore the different architectures which can can be implemented.

### 1. Security

##### Background:

There are a standard set of security controls within Azure which are enabled by default - i.e. encryption in transit for Azure Storage, Azure App Services & Azure SQL - or configurable - i.e. encryption at rest for Azure SQL - which align perfectly with most company security policies. When it comes to the use of PaaS services there is a misalignment as most security policies are based on traditional On-Prem LAN architecture.


###### 1.1 Standard PaaS Architecture:

Below is a typical PaaS architecture.  
![](/images/APP-SQL-PAAS.png)

###### 1.2 PaaS App Service & Private PaaS SQL Architecture:

![](/images/APP-PAAS-SQL-PPAAS.png)

###### 1.3 Private PaaS App Service & Private PaaS SQL Architecture:

![](/images/APP-PPAAS-SQL-PPAAS.png)

###### 1.4 Private PaaS App Service & IaaS SQL Architecture:

![](/images/APP-PPAAS-SQL-IAAS.png)

###### 1.5 IaaS "App Service" & IaaS SQL Architecture:

![](/images/APP-IAAS-SQL-IAAS.png)


### 2. Redundancy

##### Background:

###### 2.1 Standard SQL Redundancy:

![](/images/SQL-NON-REDUNDANT.png)

###### 2.2 In-Zone SQL Redundancy:

![](/images/SQL-IN-ZONE-REDUNDANT.png)

###### 2.3 Intra-Zone SQL Redundancy:

![](/images/SQL-ZONE-REDUNDANT.png)

###### 2.4 Intra-Region SQL Redundancy:

![](/images/SQL-REGION-REDUNDANT.png)

###### 2.5 "Full" SQL Redundancy:

![](/images/SQL-FULL-REDUNDANT.png)

