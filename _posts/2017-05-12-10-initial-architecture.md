---
layout: post
title: "Azure DevOps: #10 Initial architecture"
date: 2017-05-12
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2013-05-31
category: architecture
tags: [sample post]
# imagefeature: cover6.jpg
comments: true
share: true
---


We are going to start off with a very simple initial architecture which will:

1. Store secrets in Key Vault.

2. Create an Function App.

3. Use table storage for persisting data.

As the infrastructure is being deployed:

1. The Service Principle that is used for deploying the infrastructure is given access to Key Vault via an access policy.

2. The connection string for the storage is persisted to the Key Vault instance.

3. The Function App is given an Managed Service Identity - see specific article on this.

4. The Function App is then given access to Key Vault - via an access policy.

5. The Key Vault URI is set as a parameter in the Function App.

The application can now, find the Key Vault, access the secret containing the connection string and then use this string to persist data.

The plan is that I get more time I will add more features to the solution.

As general guidance you should never have multiple provisioning scripts performing changes on the same resource.  There should always be one owner.

If we have a situation like the one below where we have a Core Infrastructure and then an Infrastructure for each application.  In this instance the application deployment would need to allow access to the Key Vault.  This would mean that there would be 2 different views of the state of the Key Vault.
![](/images/WeFinance-POC-02-wont-work.png)

A simpler architecture would be the one below where the application itself deploys and managed its own Key Vault.

![](/images/WeFinance-POC-02-will-work.png)

This is the simple architecture which we are going to deploy.
