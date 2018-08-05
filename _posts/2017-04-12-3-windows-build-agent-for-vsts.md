---
layout: post
title: "Creating a Visual Studio Team Services Build Agent"
date: 2017-04-13
category: CI/CD
tags: [Azure, Visual Studio Team Services, v1]

---
A build agent runs your configured build and release tasks.

When you sign up for Visual Studio Team Services you are allocated a given amount of build/release hours for free.  After this you will have to either buy more or create your own Build Agent.


- Select Windows based Build Agent for VSTS from the Azure Marketplace
![](/images/New-Windows-Build-Agent-01-01.png)

*I wont walk through creating a VM.  But the significant screen is below.*

- Get hold of your _[Personal Access Token](https://docs.microsoft.com/en-us/vsts/organizations/accounts/use-personal-access-tokens-to-authenticate?view=vsts)_

- Which you can then put into the VSTS Settings.

![](/images/New-Windows-Build-Agent-02.png)

- It achieved this - pretty obviously - by running a custom script against the VM.

![](/images/New-Windows-Build-Agent-03.png)

- Run the agent installation manually.
{% highlight yaml %}

PS C:\agent> .\config.cmd
Connect:
Enter server URL > https://dermot-singlepane.visualstudio.com/
Enter authentication type (press enter for PAT) >
Enter personal access token >
Connecting to server ...
Register Agent:
Enter agent pool (press enter for default) > Azure-Hosted-Agents
Enter agent name (press enter for vstsagentvm1) >
Scanning for tool capabilities.
Connecting to the server.
Successfully added the agent
Testing agent connection.
Enter work folder (press enter for _work) >
2018-07-31 11:24:44Z: Settings Saved.
Enter run agent as service? (Y/N) (press enter for N) > Y
Enter User account to use for the service (press enter for NT AUTHORITY\NETWORK SERVICE) >
{% endhighlight %}

![](/images/New-Windows-Build-Agent-04.png)

- We can now return to our build definition and select this agent pool

![](/images/New-Windows-Build-Agent-05.png)

- But it fails

      ## Error 1
      No agent found in pool Azure-Hosted-Agents which satisfies the specified demands:
      msbuild
      visualstudio
      vstest
      Agent.Version -gtVersion 2.115.0 

![](/images/New-Windows-Build-Agent-06.png)

To resolve this install Visual Studio Community on the host.

- You can see what capabilities are installed on the build agent by going to the Capabilities link to the right hand side of the agent.

![](/images/New-Windows-Build-Agent-07.png)

- Rerun the build against the new agent