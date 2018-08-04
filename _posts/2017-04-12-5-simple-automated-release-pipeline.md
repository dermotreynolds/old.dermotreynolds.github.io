---
layout: post
title: "Create Visual Studio Team Services automated release Pipeline"
date: 2017-04-24
category: CI/CD

---
This article is a walk through of setting up a Release Pipeline using Visual Studio Team Services.


This uses the release that created during *Create Visual Studio Team Services automated build Pipeline* article.

## Create a new release under your Visual Studio Team Services project

- Create a new Release Pipeline.

![](/images/Setup-VSTS-Release-01-01.png)

- Select Azure App Service deployment.

![](/images/Setup-VSTS-Release-02.png)

- You can give the target environment a name.

 _We will specify the target app server shortly._

![](/images/Setup-VSTS-Release-03.png)

- Add an artefact to the pipeline.

 _This is where it picks up the build created earler._

![](/images/Setup-VSTS-Release-04.png)


![](/images/Setup-VSTS-Release-05.png)

- Lets put in the deployment steps.

![](/images/Setup-VSTS-Release-06.png)

- Here we specify the subscription that we will deploy into and the app server.

![](/images/Setup-VSTS-Release-07.png)

- By clicking on the manage link you are taken to the Service endpoint screen.

![](/images/Setup-VSTS-Release-08.png)

- Create a new Resource Manager endpoint.

![](/images/Setup-VSTS-Release-09.png)

- You will have to authenticate against your Azure Subscription.

![](/images/Setup-VSTS-Release-10.png)

- You will have to authenticate against your Azure Subscription and specify the ResourceGroup.

![](/images/Setup-VSTS-Release-11.png)

![](/images/Setup-VSTS-Release-12.png)

- Return to your release pipeline and select the new service endpoint and App Service

![](/images/Setup-VSTS-Release-13.png)

- Select Save.

![](/images/Setup-VSTS-Release-14.png)

- Select release and as before it will give you a link to the new release.  Select it and you will be able to see its progress.

![](/images/See-VSTS-Release-01.png)

- Select In Progress.

![](/images/See-VSTS-Release-02.png)

- Once complete go to Azure and select the App Service.

![](/images/See-VSTS-Release-03.png)

- Great!  All done.

![](/images/See-VSTS-Release-04.png)
