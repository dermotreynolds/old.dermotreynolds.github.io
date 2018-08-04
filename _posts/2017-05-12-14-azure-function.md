---
layout: post
title: "Azure DevOps: #14 Azure Functions"
date: 2017-05-25
category: Azure Function

---


As per the documentation *Azure Functions is a serverless compute service that enables you to run code on-demand without having to explicitly provision or manage infrastructure.*

It allows you to develop small single purpose functions in virtually any language and for each to be triggered an a variety of different ways that to be scaled independently of one another.  For example, you may have a place order function which listens on a queue and when a message appears to perform the action.

Under the hood a function is an App Service.  One of the advantages of functions is that you can pay on a pure consumption model.  There is no need to keep multiple servers running during quiet periods.

Our azure function looks as follows once deployed:

![](/images/Azure-Function-01.png)

We can look at the function responding whilst issuing a Postman request.

![](/images/Azure-Function-02.png)
