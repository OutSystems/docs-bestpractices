---
summary: Avoid stopping or slowing down your application development by keeping your development environment clean.
tags: 
en_title: Best Practices for a Tidy and Clean Development Environment
---
# Best Practices for a Tidy and Clean Development Environment

<pre class="script-css">
.subtopic {
    font-size: 20px;
    font-weight: bold;
}
</pre>

As your factory of applications grows, more database space will be consumed. Being the database a core piece of the OutSystems platform, it’s important that you keep the development environment tidy and clean so it doesn’t slow down your team. Although you can always improve the processing power capacity, this usually brings an additional cost, and requires the involvement of SysAdmin profiles to perform infrastructure tasks.

Having an untidy development environment may result in:

* Timeout errors while publishing
* Timeout while opening modules
* Slow deployments
* Overall bad performance across the environment (development and application)

In this article we will explore the different types of data that may be consuming unnecessary database space and guide you on different ways to clean it.


## Module Versions

Manage module versions is one of the most relevant actions as it has an impact on several tasks of the application development (open modules, adding/removing references, publish).

Each time you publish a module, a new version is created in your environment. Versions are especially useful if you need to rollback to a previous stage of your development.
However, as the application continues to grow, if you keep a high publish rate, you’ll soon end up having hundreds or thousands of versions, and you may start experiencing some slowness on your daily development tasks.

That being said is important in Development environments, to regularly clean up old module versions that you don’t need anymore. You can  delete old module versions directly via Service Center or using the DbCleaner API, that provides functionality for freeing up database space.

<div class="subtopic">Delete Old Module Versions Using Service Center</div>

1. Navigate to [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform) (https://YOUR_ENVIRONMENT/ServiceCenter)
1. Click on the Factory tab
1. Click on the Modules submenu option. Then click the Check Old Module Versions to Delete link
1. Choose the time period to delete, and click the Check Versions to Delete button
1. A list of the module versions will be displayed (list limited to 100 records)
1. Proceed by clicking Delete Displayed Versions

![](images/servicecenter-old-module-versions.png?width=500)

<div class="subtopic">DbCleaner API</div>

You can also implement your own solution, allowing a more flexible clean up of old modules, using the DbCleaner API actions:

* [ModuleVersion_ListOldest](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#ModuleVersion_ListOldest): Returns a list of module versions that are stored in the database and that were published before the specified date and time. This action does not return the module version that is currently published nor module versions used in tagged versions of applications or solutions.
* [ModuleVersion_Delete](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#ModuleVersion_Delete): Deletes the specified module version of the specified module from the database.
* [ModuleVersion_DeleteAll](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#ModuleVersion_DeleteAll): Deletes module versions that were published before the specified date and time. This action does not delete the module version that is currently published nor module versions used in tagged versions of applications or solutions.


<div class="info" markdown="1">
In both cases, be aware that only the module versions not associated with tagged Application or Solution versions will be deleted. This way, if you have the need to rollback the application to a previous version, it’s guaranteed that the code is not lost.
</div>


## Solution Versions

Solutions aggregate sets of modules in the environment. If you have created Solution versions that you don’t need anymore, you can delete them in Service Center.

1. Navigate to [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform) (https://YOUR_ENVIRONMENT/ServiceCenter)
1. Click on the Factory tab
1. Click on the Solutions submenu option
1. Navigate to a specific Solution detail screen
1. In the Versions tab, select the version to delete
1. Proceed by clicking the Delete button

![](images/servicecenter-solution-version-delete.png?width=500)

Once you delete the Solution version, you can then delete the module version associated with it, as described in the [Module versions](#module-versions) section.

## Application Tagged Versions

In case you are confident that you won’t need to rollback to a specific [tagged version](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Deploy_Applications/Tag_a_Version), you can also delete application versions.
Since LifeTime Management Console Release Feb.2019, [LifeTime API v2](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/LifeTime_API_v2) provides a method to delete an application version if not “InUse”.

` DELETE /lifetimeapi/rest/v2/applications/{ApplicationKey}/versions/{VersionKey}/ `

Once you delete the tagged application version, you can then delete the module version associated with it, as described in the [Module versions](#module-versions) section.


## Temporary Test Modules

While developing applications your team may need to create some temporary test modules, proof of concept (POC’s), and often deploy and test some Forge components.
All these modules are useful during the implementation phase, but soon they may become obsolete, and you will end up having unused code in your development environment.
It is a best practice, as part of a regular Factory management, to clean/delete these modules from the environment. You can delete the whole application or individual modules.

<div class="subtopic">Delete Application</div>

1. Navigate to [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform) (https://YOUR_ENVIRONMENT/ServiceCenter)
1. Click on the Factory tab
1. Click on the Applications submenu option
1. Navigate to a specific Application detail screen
1. Proceed by clicking the Delete button

![](images/servicecenter-application-delete.png?width=500)

<div class="subtopic">Delete Module</div>

1. Navigate to [Service Center](https://success.outsystems.com/Support/Enterprise_Customers/Licensing/Overview/How_to_access_your_OutSystems_Platform) (https://YOUR_ENVIRONMENT/ServiceCenter)
1. Click on the Factory tab
1. Click on the Modules submenu option
1. Navigate to a specific Module detail screen
1. Proceed by clicking the Delete button

![](images/servicecenter-module-delete.png?width=700)


## Application Data

Application data is basically all the information that is stored in the database Entities of your applications. This includes the data that you'll be adding during the unit testing as well as deleted Entities and Attributes of the data model.

<div class="subtopic">Test Data</div>

Especially in the development environment is common to have test/dummy data. This is the result of the developers’ tests during the development of different features. Although this data is not critical for the application to run, it’s common that this data will grow and sometimes may even have some inconsistencies, being useful to clean it from time to time. 
With OutSystems, you don’t need to have access to the database to perform these cleaning operations. Check [How to delete data from Entities](https://success.outsystems.com/Documentation/How-to_Guides/Data/How_to_delete_data_from_Entities) to implement your own delete data logic.

<div class="subtopic">Entities and Attributes</div>

When you delete Entities and Attributes in your applications, OutSystems doesn't delete the corresponding table or column in the database. Your data is safely stored just in case you want to rollback your application. If you feel that those entities and attributes will no longer be used, then it’s a best practice to delete them, thus freeing database space. For that OutSystems provides the DbCleaner API methods:

* [Attribute_ListDeleted](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Attribute_ListDeleted): Returns a list of attributes, with their information, that have been deleted from module’s meta model but are still physically present in the database.
* [Attribute_DropColumn](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Attribute_DropColumn): Physically deletes the database table column associated to the specified entity attribute. If the entity attribute still exists in a module's meta model, the delete operation will not be performed.
* [Entity_ListDeleted](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Entity_ListDeleted): Returns a list of entities, with their information, that have been deleted from module’s meta model but are still physically present in the database.
* [Entity_DropTable](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/DbCleaner_API#Entity_DropTable): Physically deletes the database table associated to the specified entity. If the entity still exists in a module's meta model, the delete operation will not be performed.


## Processes

Business Process Technology (BPT) Processes can also be consuming unnecessary database space. It’s common that you launch some processes to test the functionality when you are developing.
To clean all the logged information of old processes, OutSystems provides the [BPT API](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/BPT_API) methods:

* [Process_Delete](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/BPT_API#Process_Delete): Deletes all the logged information of an instance of a top level Process, which must be either terminated or closed.
* [Process_BulkDelete](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/BPT_API#Process_BulkDelete): Deletes all the logged information of instances of top level Processes that have terminated or closed before the given date.

Add them as references on your application and invoke them to delete old instances.


## Emails

Such as Processes, Emails can also be consuming unnecessary database space. This is especially relevant as emails can have attachments, sometimes quite big (>1MB).
In order to clean email information, OutSystems provides the [Emails API](https://success.outsystems.com/Documentation/11/Reference/OutSystems_APIs/Emails_API) so you can easily implement your cleaning mechanism.


## Logs

[Log information](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Monitor_and_Troubleshoot/Logging_database_and_architecture) reflects the lifecycle of accesses to applications by end users or external systems. There are two log types:

* Top-level logs (Screen, Integration, Mobile Request, Cyclic Job)
* Drill logs (Error, General, Integration, Extension)

The log model is designed to have minimal interference in the application runtime and even in the Development environment their impact is somehow controlled with the [rotation of the logs](https://success.outsystems.com/Documentation/11/Managing_the_Applications_Lifecycle/Monitor_and_Troubleshoot/Logging_database_and_architecture/The_log_tables_and_views#The_rotation_of_the_logs) (every week the Log Service will truncate one of the weekly tables to clear space from the database and allow new logs to be inserted when the time for that table to be used again comes).

While delete logs is not a supported action, from OutSystems 11 you can [store log data in a separate database](https://success.outsystems.com/Support/Enterprise_Customers/Upgrading/Keep_OutSystems_log_data_in_a_separate_database), reducing the impact that log-writing operations could have on running applications while application data is being accessed. 


## Additional Considerations

This article guides you across the multiple components that consume space on the database of the Development environment, explaining how you can clean unnecessary data, without the need to directly access the database itself.

While these best practices can be applied ad hoc, without limitations, you should also have in mind the maintenance plans of an on-premises installation type and that this shouldn't be just a one-time concern.

### Cloud vs On-Premises

While the **above clean-up options are valid on both installation types**, there are some additional actions that you can perform on-premises as you are the responsible to maintain the infrastructure.

If you have a development environment installed **on-premises**, besides having more flexibility with the database (allowing you to truncate data for example), you should also consider a regular maintenance of the file system of your servers - refer to the [Guide to disk space usage and control on OutSystems Platform servers](http://www.outsystems.com/goto/disk-space-usage-guide).

### Automate the Clean-Up

The clean-up of the Development environment should be a regular concern, and not just an on-time task because you are experiencing some issues ("better safe than sorry").
With that in mind, a possible approach is to implement a periodic clean-up logic (using [OutSystems Timers](https://success.outsystems.com/Documentation/11/Developing_an_Application/Use_Timers/Create_and_Run_Timers)), that can address the different components described in this article.

If you don’t want to create your clean-up solution from scratch, you can always explore the components available on the OutSystems Forge (search for “cleaner” or “database space” and you’ll find some different options from our Community members).
