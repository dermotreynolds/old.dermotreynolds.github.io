---
layout: post
title: "Barracuda Vulnerability Scanning Of Azure App"
date: 2017-04-28
category: security
tags: [azure, Barracuda, Security, v1]

---
In a CI/CD environment Vulnerability Scanning should be automated.

Below is a very quick intro to Barracuda Vulnerability Manager.  As background, the application scanned was a single HTML "Hello World" page. 

- Enabling Barracuda Vulnerability Manager

![](/images/New-Vulerability-Scan-01.png)

- The report shows the vulnerabilities found.  

![](/images/New-Vulerability-Scan-02.png)

- Vulnerabilities including SSL certs for Azure App Service being Untrusted!
      The following problems were found with the certificate chain supplied by the server: 

      Unable to get issuer cert locally
      Subject: /ST=Washington/C=US/OU=Microsoft IT/CN=Microsoft IT TLS CA 4/L=Redmond/O=Microsoft Corporation
      Issuer: /CN=Baltimore CyberTrust Root/O=Baltimore/C=IE/OU=CyberTrust
      Certificate depth: 1