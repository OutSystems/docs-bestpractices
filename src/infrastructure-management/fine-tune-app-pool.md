---
summary: 
locale: en-us
guid: 939F9A46-EBA1-4442-BD8B-6CAAA5CDE555
---

# Understanding and fine tuning application pool recycling for use with OutSystems

An Application Pool is a mechanism used by IIS to isolate Web applications, allowing you to have different configurations (security, resource usage, etc) and preventing misbehaving applications from interfering with other applications.

Generally, each Application Pool corresponds to one worker process. A worker process is a windows process (w3wp.exe) which runs Web Applications, and is responsible for handling requests sent to a Web Server for a specific application pool.

## What is application pool recycling in IIS?

Recycling means that the worker process that handles requests for that application pool is terminated and a new one is started. This is generally done to avoid unstable states that can lead to application crashes, hangs, or memory leaks.

By default IIS uses the overlapped recycle method, which keeps the old process up until the current requests are finished processing (or a set timeout elapses) while the new process handles new requests. This ensures service continuity so that you usually do not notice a recycle.

## Where can I configure automatic application pool recycling?

In Windows Server, open the Start Menu -> Administrative Tools -> Internet Information Services (IIS) Manager. Expand the server and click on “Application Pools”. In the center window right-click the Application Pool you wish to configure and select **Recycling...** (be careful not to click **Recycle...** which will start a recycle).

A two-step "wizard" appears. In the first step select which settings to use, and in the second step select which events you want to log in the Windows Event Logs if they are triggered.

## What are the recommended values for OutSystems applications?

You can check the baseline values that are recommended for an OutSystems installation in the Installation Checklist. These values are only initial guidelines: you should fine-tune your memory configuration by collecting performance data from each application pool and adjusting these values accordingly. You can use the method described in [this forum post](https://www.outsystems.com/forums/discussion/10298/identifying-application-related-processor-overload-under-the-net-stack/) to collect memory usage data (you only need to use the "Process\Private Bytes" counter for all the w3wp.exe processes instead of all the counters mentioned in the post).

The values should be:

* High enough not to cause unnecessary recycles under load;
* Low enough so that the recycles are triggered before they affect other application pools.

After collecting data on real-world usage, you can parcel the available memory proportionally between the application pools. You should review these values periodically as the usage of your applications changes or when you deploy new applications.

## Example configuration

You have two application pools: OutSystemsApplications and ServiceCenterAppPool. Real-world usage data shows that the maximum consumed memory for the former is 700 MB and for the latter (even when publishing a large solution) is 300 MB.

If your server has 4 GB of RAM memory, you should set aside some for the operating system and divide the remaining among the application pools as follows:

* Operating system: 1 GB
* ServiceCenterAppPool: 900 MB
* OutSystemsApplications: 2100 MB
