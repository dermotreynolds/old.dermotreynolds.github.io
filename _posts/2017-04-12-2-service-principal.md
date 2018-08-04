---
layout: post
title: "What is a Service Principal?"
date: 2017-04-12
category: security

---
In order for our project within VSTS to perform actions on our Azure Subscription we need to create a Service endpoint which in turn sets up a Service Principal.

There are 3 terms in play here which you should be aware of:

1. **A Service Endpoint.**  This is the configuration within VSTS which points to your Service Principal.  It is also know as a Service connection.
2. **An Application Registration.**  This is the configuration within Azure which is creates an identity which can be given permissions.
3. **A Service Principal.**  This is what the identity created by the Application Registration is known as.

In summary, to set this up we have 2 areas which hold configuration.

The the official documentation describes a Service Principal as:

: When you have an app or script that needs to access resources, you can set up an identity for the app and authenticate the app with its own credentials. This identity is known as a Service Principal.

This approach enables us to:

- Assign permissions to the app identity that are different than your own permissions. Typically, these permissions are restricted to exactly what the app needs to do.

- Use a certificate for authentication when executing an unattended script.*

In summary, a Service Principal is an "Account" which you assign certain access to.  In our case we can give this Service Principal access to the Resource Group that will hold our resources.

## To find your Service Principal from VSTS
- Under your project go to Project Settings/Service endpoints.

![](/images/Service-Principle-01.png)

- Selecting Manage Service Principal will take you to the definition in Azure.

![](/images/Service-Principle-02.png)

You will notice that it has a very ugly auto-generated name.  My recommendation is to rename it to something meaningful and unique.

I am renaming mine to VisualStudio-Service-Principal.

If you go to the Resource Group that you created you will find this Service Principal under Access control(IAM)

![](/images/Add-Service-Principal-02.png)

## To create a Service Principal from VSTS
1. Go to your Project Settings on the bottom left hand side.
2. Select Service Endpoints.
![](/images/Add-Service-Principal-01.png)
3. Select Azure Resource Manager.
4. Follow the Azure authentication process and select your Resource Group.




