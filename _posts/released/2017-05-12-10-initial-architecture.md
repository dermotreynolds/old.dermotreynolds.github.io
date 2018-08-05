---
layout: post
title: "Terraform/ARM Architecture Dos & Don'ts"
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2013-05-31
category: architecture
tags: [Azure, Key Vault, Architecture, CI/CD, v1]
comments: false
share: true
---

As general guidance you should never have multiple provisioning scripts performing changes on the same resources.  

Resources should only have one owner.

It does make sense to have services logically isolated from one another where those systems are independent and/or managed by different teams.  In this instance all of the resources for a system should be self contained.

In the scenario below we have a centralised Key Vault which stores our secrets.  This is analagous to having a central Active Directory.  However, there are 2 things:

1. We have 2 provisioning scripts that have their own independent state view of the Key Valult resource.
2. Key Vault is negligible in cost and is irrelevant in cost relative to an Active Directory deployment.

![](/images/WeFinance-POC-02-wont-work.png)

A simpler architecture would be the one below where the application itself deploys and managed its own Key Vault.

![](/images/WeFinance-POC-02-will-work.png)

A further refinement would be to move the state storage also.