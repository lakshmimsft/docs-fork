---
type: docs
title: "Swap connector resource"
linkTitle: "Swap connector resource"
description: "Swap a connector resource for an Azure resource to back the connector and deploy it to an environment with Azure cloud provider configured"
weight: 4000
slug: "swap-connector"
---

This step of swapping a connector resource for an Azure resource closely relates to an infrastructure-admin responsibility in enterprises. Once the developer does a handoff, the infrastructure-admin sets up the production environments to port the application. If you are a developer who handles everything related to deployments, this would be applicable to you as well.

To abstract the infrastructure workflows from the development workflows, we are working on a solution called **Radius recipes** that will enable the infra-admin to prepare connector recipes with all the organizational requirements which can be used by the developers without having to worry about provisioning or managing a resource. Check back for more updates in our future releases

## Initialize Radius environment with Azure cloud provider

Make sure you have the [environment initialized with Azure cloud provider]({{<ref webapp-initialize-environment>}}) 

## Swap the connector for an Azure resource

The app.bicep from the previous step should look like this 

{{< rad file="snippets/app-container.bicep" embed=true >}}

Update the bicep file like below to swap the connector resource with an Azure cosmosdb resource

{{< rad file="snippets/app-azure.bicep" embed=true >}}

## Deploy the application with Azure database

{{% alert title="Known issue: Azure deployments" color="warning" %}}
There is a known issue where deployments to Azure will fail with a "NotFound" error for templates containing starters. This is being addressed in an upcoming release. As a workaround submit the deployment a second time. The second deployment should succeed.
{{% /alert %}}

1. In a terminal window deploy the app.bicep file :

   ```sh
   rad deploy app.bicep
   ```
   This may take a few minutes to create the database. On completion, you will see the following resources:

     ```sh
   Deployment In Progress:

     Completed       Application                Applications.Core/applications
     Completed       Container                  Applications.Core/containers
     Completed       frontend-route             Applications.Core/httpRoutes
     Completed       gateway                    Applications.Core/gateways
     Completed       mongo.com.MongoDatabase    db

   Deployment Complete 
   ```

   Just like before, a public endpoint will be available through the gateway.

   ```sh
   Public Endpoints:
    gateway  Gateway            http://20.252.19.39 
   ```

1. To test your application, navigate to the public endpoint that was printed at the end of the deployment. You should see a page like:

   <img src="todoapp-withdb.png" width="400" alt="screenshot of the todo application with a database">

   If your page matches, then it means that the container is able to communicate with the database. Just like before, you can test the features of the todo app. Add a task or two. Now your data is being stored in an actual database.

## Cleanup

{{% alert title="Delete application" color="warning" %}} If you're done with testing, you can use the rad CLI to [delete an environment]({{< ref rad_env_delete.md >}}) to prevent additional charges in your Azure subscription. {{% /alert %}}

{{<button text="Previous step: Add a database connector" page="webapp-add-database">}}