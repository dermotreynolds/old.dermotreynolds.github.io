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

Assuming that this classification allows you to use PaaS you can perform an interoperability check with Azure.
i.e. 

- Check the version of PostgreSQL supported on Azure

PostgreSQL Versions __[here](https://docs.microsoft.com/en-us/azure/postgresql/concepts-supported-versions){:target="_blank"}__.

- Check the version of app service components supported:

App Service Runtime Versions __[here](https://docs.microsoft.com/en-us/azure/app-service/containers/app-service-linux-intro){:target="_blank"}__.

- Check the version of MySQL supported:

MySQL Versions __[here](https://docs.microsoft.com/en-us/azure/mysql/concepts-supported-versions){:target="_blank"}__.

At this point you will need to address the architectural issue with the above where you have a database residing on an app server.

Other considerations are:

1. Is there a simplification agenda i.e. reducing the number of disparate technologies being used?  If this is the case then you may be moving to a single database technology.

2. What is the capacity requirements of the above elements?  All services on Azure have __[limits](https://docs.microsoft.com/en-us/azure/azure-subscription-service-limits){:target="_blank"}__.

3. Are any of the above components using specific extensions not supported on Azure.  For example PostgreSQL supports __[extension](https://docs.microsoft.com/en-us/azure/postgresql/concepts-extensions){:target="_blank"}__.

4. Is there a consolidation opportunity to collapse the 2 databases into one?

5. Does the the target services support the operational aspects i.e. backup & recovery, security and monitoring.  All Azure services are developing rapidly and features that exist for one may not exist for another.

6. What __[parameters](https://docs.microsoft.com/en-us/azure/postgresql/howto-configure-server-parameters-using-cli){:target="_blank"}__ need to be set on the target for the application to function.  

7. Probably the most important of all, you should determine what the TCO of the solution is.  This allows you to determine if the solution is within the budget but also to determine if you have chosen the best combination for the use-case.  For example, it may be that to support a specific number of concurrent users a different technology is lower cost.  Whilst this may not align with the strategic direction it is worth while sharing your observations with your peers to asses whether you are on the right path.  