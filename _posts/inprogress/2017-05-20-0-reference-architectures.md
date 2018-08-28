---
layout: post
title: "Reference Architectures"
date: 2017-05-20
featured: true
category: Architecture
tags: [Azure, Architecture, v1]

---
Both Azure and AWS has a wealth of "patterns" which can be reused in your architecture.

If, for example you wanted to isolate your cloud deployment from your On-Prem services you may want to implement the following pattern:

![](/images/Reference-Architecture-Azure-Dmz-Private.png)

If you want to expose services to the internet with the same level of isolation you may want to implement the following:

![](/images/Reference-Architecture-Azure-Dmz-Public.png)

The reference architectures for Azure and AWS are located here:
Azure Architecture Center __[here](https://docs.microsoft.com/en-us/azure/architecture/){:target="_blank"}__.
AWS Cloudformation Examples __[here](https://aws-quickstart.github.io/templates-examples.html){:target="_blank"}__.