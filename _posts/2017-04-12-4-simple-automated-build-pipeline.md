---
layout: post
title: "Create Visual Studio Team Services automated build Pipeline"
date: 2017-04-16
category: CI/CD
tags: [Azure, Visual Studio Team Services, CI/CD, v1]

---
This article is a walk through of setting up a Build Pipeline using Visual Studio Team Services.

We will build on this in our Release Pipeline.

This uses the app that was published to GitHub as part of the "Getting Started" article.

## Create a new project in Visual Studio Team Services

- Create a new Project.

<img src="/images/Setup-VSTS-Build-01-01.png" alt="drawing" width="400px"/>

- Give it a name.

<img src="/images/Setup-VSTS-Build-02-01.png" alt="drawing" width="400px"/>

## Create a new Build Pipeline

- Create a new build definition.

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

## Let's test it

- Now that we have set it up, lets create a build.

<img src="/images/Setup-VSTS-Build-09-01.png" alt="drawing" width="400px"/>

- We are given a link to the newly created build.

<img src="/images/Setup-VSTS-Build-10-01.png" alt="drawing" width="400px"/>

- Clicking on the link and selecting Logs we can see the progress.

<img src="/images/See-VSTS-Build-01.png" alt="drawing" width="400px"/>