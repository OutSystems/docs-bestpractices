---
summary: Deploy business critical applications in a separate pipeline to ensure runtime and data isolation.
--- 

# Application isolation in OutSystems

As some of your OutSystems applications grow in terms of the number of users or data volume, they may start affecting the performance of unrelated applications, since they share some resources. OutSystems allows you to get both data and runtime isolation by using **pipelines**. This isolation is guaranteed between applications in different pipelines.

A pipeline allows independent development and release of one or more related applications. You can use several pipelines to handle apps with different compliance or scalability needs. For example, you can have one pipeline for the apps of each line of business, or one pipeline for a critical business application.

Check the following example of an OutSystems infrastructure with two pipelines:

![Diagram of an OutSystems infrastructure with two pipelines](images/architecture-pipelines-diag.png?width=600)

To understand what's included by default in each pipeline you must also get familiar with the concept of **environment**. An environment is a step in a pipeline that is associated with a set of runtime resources (e.g. URL, database), and it's where applications run. An environment has a specific purpose (Development, Non-Production or Production). By default, a pipeline comes with a Non-Production and a Production environment.

All the environments in your OutSystems infrastructure are managed through [LifeTime](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle), a central management console for all your infrastructure. This console allows you to:

* have a birds-eye view of all the environments across pipelines
* deploy apps and their dependencies between environments
* manage IT users, roles, and teams in a single place across your infrastructure
* check the deployed applications and their versions for every environment

To add a new pipeline to your infrastructure, install each new environment of the pipeline and register these new environments in LifeTime. Check [Register your OutSystems environments](https://success.outsystems.com/Documentation/11/Setting_Up_OutSystems/Configure_the_infrastructure_management_console#Register_your_OutSystems_environments) for more information.

To deploy an application to a specific environment belonging to your second pipeline, for example from "Development" to "Quality P2", you must [choose the correct target environment](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Deploy_Applications/Deploy_an_Application#change-target-environment) when defining the deployment plan in LifeTime.

## Managing app resources through configuration

If you have an on-premises infrastructure, and since OutSystems relies on standard technologies for running its generated apps, you can make some configuration changes to adjust how your OutSystems apps consume resources. By making these adjustments you can make sure that your critical applications do not lack needed resources.  
You can make some adjustments at the database level, for example, if the strained resource impacting your apps is the database, or at the application server level.

