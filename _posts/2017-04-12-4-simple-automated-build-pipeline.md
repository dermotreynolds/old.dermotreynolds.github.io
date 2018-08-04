---
layout: post
title: "Azure DevOps: #4 Create a basic automated build Pipeline"
date: 2017-04-16
category: CI/CD

---

The purpose of this series is to walk through how to create a CI/CD pipeline using:
- Visual Studio
- Visual Studio Team Services
- Azure
- Terraform

This uses the app that was published to GitHub as part of the Publish A Very Basic App To GitHub article.

## Create a new project in Visual Studio Team Services

- Create a new Project.

<img src="/images/Setup-VSTS-Build-01-01.png" alt="drawing" width="400px"/>

- Give it a name.

<img src="/images/Setup-VSTS-Build-02-01.png" alt="drawing" width="400px"/>

- Create a new build pipeline.

<img src="/images/Setup-VSTS-Build-03-01.png" alt="drawing" width="400px"/>

- Connect it to our GitHub project.

<img src="/images/Setup-VSTS-Build-04-01.png" alt="drawing" width="400px"/>

- Specify the project and branch.

<img src="/images/Setup-VSTS-Build-05-01.png" alt="drawing" width="400px"/>

- Select a template.

<img src="/images/Setup-VSTS-Build-06-01.png" alt="drawing" width="400px"/>

- Specify the agent queue.  
Note: If this is the first time that you have setup Visual Studio Team Services then you will not have to do this.

<img src="/images/Setup-VSTS-Build-07-01.png" alt="drawing" width="400px"/>

- If you want to enable continuous integration.
This will trigger a build each time you update your GitHub repository.

<img src="/images/Setup-VSTS-Build-08-01.png" alt="drawing" width="400px"/>

- Now that we have set it up, lets create a build.

<img src="/images/Setup-VSTS-Build-09-01.png" alt="drawing" width="400px"/>

- We are given a link to the newly created build.

<img src="/images/Setup-VSTS-Build-10-01.png" alt="drawing" width="400px"/>

- Clicking on the link and selecting Logs we can see the progress.

<img src="/images/See-VSTS-Build-01.png" alt="drawing" width="400px"/>