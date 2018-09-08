---
layout: post
title: "Detailed Migration Planning"
date: 2018-09-08
featured: true
category: Migration
tags: [Azure, Architecture, v1]

---
Before you start migrating to Azure or AWS you need to know what its current and future state is.

A great tool for discovering your application estate is BMC Discovery - which used to be called ADDM.

Below is an example of an environment which I created in a lab consisting of a couple of servers with various different components installed.

BMC Discovery will "discover" the connectivity between elements i.e. if an Application Server connects to a DB Server.  It uses this information to generate these visual representations.

Once you have scanned your environment you can see these by selecting

Explore / Data / Candidate Software Instance List

![](/images/BMC-Discover-Application-01.png)

In my case I have published the application and given the service classification - C3 - and an indication of the data classification - S3 - where both of these classifications indicate the availability and security requirements.

You can then drill into the application versions by selecting Sofware Dependencies from one of the associated reports:

![](/images/BMC-Discover-Application-01.1.png)

You now have a list of the versions of the various application components

![](/images/BMC-Discover-Application-02.png)