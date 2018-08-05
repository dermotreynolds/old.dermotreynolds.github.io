---
layout: post
title: "Terraform/Azure Resource Manager Architecture - Dos and Donts"
date: 2017-05-12
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
modified: 2013-05-31
category: architecture
tags: [Azure, Key Vault, Function App, Service Principle, CI/CD, v0.9]
# imagefeature: cover6.jpg
comments: true
share: true
---

As general guidance you should never have multiple provisioning scripts performing changes on the same resource.  There should always be one owner.

If we have a situation like the one below where we have a Core Infrastructure and then an Infrastructure for each application.  In this instance the application deployment would need to allow access to the Key Vault.  This would mean that there would be 2 different views of the state of the Key Vault.
![](/images/WeFinance-POC-02-wont-work.png)

A simpler architecture would be the one below where the application itself deploys and managed its own Key Vault.

![](/images/WeFinance-POC-02-will-work.png)

This is the simple architecture which we are going to deploy.
