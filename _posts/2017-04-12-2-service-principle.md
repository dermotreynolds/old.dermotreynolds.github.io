---
layout: post
title: "Azure DevOps: #2 What is a Service Principle?"
date: 2017-04-12
category: security

---
When we setup the release to Azure we needed to setup a Service endpoint which in turn setup a Service Principle.

As per the official documentation:

*When you have an app or script that needs to access resources, you can set up an identity for the app and authenticate the app with its own credentials. This identity is known as a service principal.*

*This approach enables you to:*

*-Assign permissions to the app identity that are different than your own permissions. Typically, these permissions are restricted to exactly what the app needs to do.*

*-Use a certificate for authentication when executing an unattended script.*

In summary, a Service Principle is an "Account" which you assign certain access to.  In our case we gave this Service Principle access to the Resource Group that we created.

## Find the Service Principle Name
- Under your project go to Project Settings/Service endpoints.

![](/images/Service-Principle-01.png)

- Selecting Manage Service Principal will take you to the definition in Azure.

![](/images/Service-Principle-02.png)

You will notice that it has a very ugly auto-generated name.  My recommendation is to rename it to something meaningful and unique.

I am renaming mine to VisualStudio-Service-Principle.

If you go to the Resource Group that you created you will find this Service Principle under Access control(IAM)

![](/images/See-Service-Principle-01-01.png)
