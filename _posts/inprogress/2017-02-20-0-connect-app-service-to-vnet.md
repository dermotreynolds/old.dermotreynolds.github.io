---
layout: post
title: "Connect App Service To VNET"
date: 2017-11-15
featured: true
category: security
tags: [Azure, App Services, Security, v1]

---
You can connect an App Service to a VNet.  Documented __[here](https://docs.microsoft.com/en-us/azure/app-service/web-sites-integrate-with-vnet){:target="_blank"}__.

You can test the connectivity to a backend server using:

``` javascript
tcpping 10.1.0.4
D:\home\site\wwwroot
Connection attempt failed: Connection timed out.
Connected to 10.1.0.4:80, time taken: <1ms
Connected to 10.1.0.4:80, time taken: 5ms
Connected to 10.1.0.4:80, time taken: <1ms
Complete: 3/4 successfull attempts (75%). Average success time: 1.666667ms
```