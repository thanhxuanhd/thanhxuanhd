## Introduction

You've heard buzzwords about the cloud—scale, elasticity, microservices. Perhaps you've seen other companies be successful with the cloud and wondered how it could help you meet your business challenges or even grow your career. Did you know that more than 90% of Fortune 500 companies run on the Microsoft Cloud?

The cloud helps power your everyday life, and it's often present in ways you don't even realize. In this new connected world, we believe technology creates opportunity. To keep up with today's ever-changing digital world, understanding cloud technology can help align your career to this exciting revolution.

In less time than it takes to eat lunch, you'll create and configure your first website on Azure, a foundational building block of everything from digital transformation to the next big startup.

In this module, you will:

- Learn what Microsoft Azure is and how it relates to cloud computing
- Deploy and configure a web server
- Learn how to scale up your server to give you more compute power
- Use Azure Cloud Shell to interact with your web server

### What is Azure?

Azure is Microsoft's cloud computing platform. Azure is a continually (liên tục) expanding set of cloud services that help your organization meet your current and future business challenges. Azure gives you the freedom to build, manage, and deploy applications on a massive global network using your favorite tools and frameworks.

Before we go further, let's briefly define cloud computing.

**What is cloud computing?**

Cloud computing is the delivery of computing services over the Internet using a pay-as-you-go pricing model. Put another way; it's a way to rent compute power and storage from someone else's data center.

Instead of maintaining CPUs and storage in your data center, you rent them for the time that you need them. The cloud provider takes care of maintaining the underlying infrastructure for you.

You can treat cloud resources like you would your resources in your own data center. When you're done using them, you give them back. You're billed only for what you use.

While this approach is great, the real value of the cloud is that it enables you to quickly solve your toughest business challenges and bring cutting edge solutions to your users.

Why should I move to the cloud?

The cloud helps you move faster and innovate in ways that were once nearly impossible.

In our ever-changing digital world, two trends emerge:

- Teams are delivering new features to their users at record speeds.
- End users expect an increasingly rich and immersive experience with their devices and with software.

Software releases were once scheduled in terms of months or even years. Today, teams are releasing features in smaller batches. Releases are now often scheduled in terms of days or weeks. Some teams even deliver software updates continuously—sometimes with multiple releases within the same day.

Think of all the ways you interact with devices that you couldn't do just a few years ago. Many devices can recognize your face and respond to voice commands. Augmented reality changes the way you interact with the physical world. Household appliances are even beginning to act intelligently. These technologies are just a few examples, many of which are powered by the cloud.

To power your services and deliver innovative and novel user experiences more quickly, the cloud provides on-demand access to:

- A nearly limitless pool of raw compute, storage, and networking components.
- Speech recognition and other cognitive services that help make your application stand out from the crowd.
- Analytics services that enable you to make sense of telemetry data coming back from your software and devices.

Let's see how Azure fits in with cloud computing.

**What can I do on Azure?**

Azure provides over 100 services that enable you to do everything from running your existing applications on virtual machines to exploring new software paradigms such as intelligent bots and mixed reality.

Many teams start exploring the cloud by moving their existing applications to virtual machines that run in Azure. While migrating your existing apps to virtual machines is a good start, the cloud is more than just "a different place to run your virtual machines".

For example, Azure provides AI and machine-learning services that can naturally communicate with your users through vision, hearing, and speech. It also provides storage solutions that dynamically grow to accommodate massive amounts of data. Azure services enable solutions that are not feasible without the power of the cloud.

### Tour of Azure services

Azure can help you tackle tough business challenges. You bring your requirements, creativity, and favorite software development tools. Azure brings a massive global infrastructure that's always available for you to build your applications on.

Let's take a quick tour of the high-level services Azure offers.
Azure services
Here's a big-picture view of the available services and features in Azure.

Let's take a closer look at the most commonly used categories:

- Compute
- Networking
- Storage
- Mobile
- Databases
- Web
- Internet of Things
- Big Data
- Artificial Intelligence
- DevOps

#### Compute

Compute services are often one of the primary reasons why companies move to the Azure platform. Azure provides a range of options for hosting applications and services. Here are some examples of compute services in Azure:

| Service name                     | Service function                                                         |
| -------------------------------- | ------------------------------------------------------------------------ |
| Azure Virtual Machines           | Windows or Linux virtual machines (VMs) hosted in Azure                  |
| Azure Virtual Machine Scale Sets | Scaling for Windows or Linux VMs hosted in Azure                         |
| Azure Kubernetes Service         | Enables management of a cluster of VMs that run containerized services   |
| Azure Service Fabric             | Distributed systems platform. Runs in Azure or on-premises               |
| Azure Batch                      | Managed service for parallel and high-performance computing applications |
| Azure Container Instances        | Run containerized apps on Azure without provisioning servers or VMs      |
| Azure Functions                  | An event-driven, serverless compute service                              |

#### Networking

Linking compute resources and providing access to applications is the key function of Azure networking. Networking functionality in Azure includes a range of options to connect the outside world to services and features in the global Microsoft Azure datacenters.

Azure networking facilities have the following features:

| Service name                   | Service function                                                                     |
| ------------------------------ | ------------------------------------------------------------------------------------ |
| Azure Virtual Network          | Connects VMs to incoming Virtual Private Network (VPN) connections                   |
| Azure Load Balancer            | Balances inbound and outbound connections to applications or service endpoints       |
| Azure Application Gateway      | Optimizes app server farm delivery while increasing application security             |
| Azure VPN Gateway Accesses     | Azure Virtual Networks through high-performance VPN gateways                         |
| Azure DNS                      | Provides ultra-fast DNS responses and ultra-high domain availability                 |
| Azure Content Delivery Network | Delivers high-bandwidth content to customers globally                                |
| Azure DDoS Protection          | Protects Azure-hosted applications from distributed denial of service (DDOS) attacks |
| Azure Traffic Manager          | Distributes network traffic across Azure regions worldwide                           |
| Azure ExpressRoute             | Connects to Azure over high-bandwidth dedicated secure connections                   |
| Azure Network Watcher          | Monitors and diagnoses network issues using scenario-based analysis                  |
| Azure Firewall                 | Implements high-security, high-availability firewall with unlimited scalability      |
| Azure Virtual WAN              | Creates a unified wide area network (WAN), connecting local and remote sites         |

#### Storage

Azure provides four main types of storage services. These services are:

| Service name        | Service function                                                               |
| ------------------- | ------------------------------------------------------------------------------ |
| Azure Blob storage  | Storage service for very large objects, such as video files or bitmaps         |
| Azure File storage  | File shares that you can access and manage like a file server                  |
| Azure Queue storage | A data store for queuing and reliably delivering messages between applications |
| Azure Table storage | A NoSQL store that hosts unstructured data independent of any schema           |

These services all share several common characteristics:

- **Durable** and highly available with redundancy and replication.
- **Secure** through automatic encryption and role-based access control.
- _Scalable_ with virtually unlimited storage.
- **Managed**, handling maintenance and any critical problems for you.
- **Accessible** from anywhere in the world over HTTP or HTTPS.

#### Mobile

Azure enables developers to create mobile backend services for iOS, Android, and Windows apps quickly and easily. Features that used to take time and increase project risks, such as adding corporate sign-in and then connecting to on-premises resources such as SAP, Oracle, SQL Server, and SharePoint, are now simple to include.

Other features of this service include:

- Offline data synchronization.
- Connectivity to on-premises data.
- Broadcasting push notifications.
- Autoscaling to match business needs.

#### Databases

Azure provides multiple database services to store a wide variety of data types and volumes. And with global connectivity, this data is available to users instantly.

| Service name                     | Service function                                                                              |
| -------------------------------- | --------------------------------------------------------------------------------------------- |
| Azure Cosmos DB                  | Globally distributed database that supports NoSQL options                                     |
| Azure SQL Database               | Fully managed relational database with auto-scale, integral intelligence, and robust security |
| Azure Database for MySQL         | Fully managed and scalable MySQL relational database with high availability and security      |
| Azure Database for PostgreSQL    | Fully managed and scalable PostgreSQL relational database with high availability and security |
| SQL Server on VMs                | Host enterprise SQL Server apps in the cloud                                                  |
| Azure Synapse Analytics          | Fully managed data warehouse with integral security at every level of scale at no extra cost  |
| Azure Database Migration Service | Migrates your databases to the cloud with no application code changes                         |
| Azure Cache for Redis            | Caches frequently used and static data to reduce data and application latency                 |
| Azure Database for MariaDB       | Fully managed and scalable MariaDB relational database with high availability and security    |

#### Web

Having a great web experience is critical in today's business world. Azure includes first-class support to build and host web apps and HTTP-based web services. The Azure services focused on web hosting include:

Having a great web experience is critical in today's business world. Azure includes first-class support to build and host web apps and HTTP-based web services. The Azure services focused on web hosting include:

| Service Name                          | Description                                                                |
| ------------------------------------- | -------------------------------------------------------------------------- |
| Azure App Service                     | Quickly create powerful cloud web-based apps                               |
| Azure Notification Hubs               | Send push notifications to any platform from any back end.                 |
| Azure API Management                  | Publish APIs to developers, partners, and employees securely and at scale. |
| Azure Cognitive Search                | Fully managed search as a service.                                         |
| Web Apps feature of Azure App Service | Create and deploy mission-critical web apps at scale.                      |
| Azure SignalR Service                 | Add real-time web functionalities easily.                                  |

Internet of Things
People are able to access more information than ever before. It began with personal digital assistants (PDAs), then morphed into smartphones. Now there are smart watches, smart thermostats, even smart refrigerators. Personal computers used to be the norm. Now the internet allows any item that's online-capable to access valuable information. This ability for devices to garner and then relay information for data analysis is referred to as the Internet of Things (IoT).

There are a number of services that can assist and drive end-to-end solutions for IoT on Azure.

| Service Name  | Description                                                                                                                                                      |
| ------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| IoT Central   | Fully-managed global IoT software as a service (SaaS) solution that makes it easy to connect, monitor, and manage your IoT assets at scale                       |
| Azure IoT Hub | Messaging hub that provides secure communications between and monitoring of millions of IoT devices                                                              |
| IoT Edge      | Push your data analysis models directly onto your IoT devices, allowing them to react quickly to state changes without needing to consult cloud-based AI models. |

#### Big Data

Data comes in all formats and sizes. When we talk about Big Data, we're referring to large volumes of data. Data from weather systems, communications systems, genomic research, imaging platforms, and many other scenarios generate hundreds of gigabytes of data. This amount of data makes it hard to analyze and make decisions. It's often so large that traditional forms of processing and analysis are no longer appropriate.

Open source cluster technologies have been developed to deal with these large data sets. Microsoft Azure supports a broad range of technologies and services to provide big data and analytic solutions.

| Service Name            | Description                                                                                                                                                                                   |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure Synapse Analytics | Run analytics at a massive scale using a cloud-based Enterprise Data Warehouse (EDW) that leverages massive parallel processing (MPP) to run complex queries quickly across petabytes of data |
| Azure HDInsight         | Process massive amounts of data with managed clusters of Hadoop clusters in the cloud                                                                                                         |
| Azure Databricks        | Collaborative Apache Spark–based analytics service that can be integrated with other Big Data services in Azure.                                                                              |

#### Artificial Intelligence

Artificial Intelligence, in the context of cloud computing, is based around a broad range of services, the core of which is Machine Learning. Machine Learning is a data science technique that allows computers to use existing data to forecast future behaviors, outcomes, and trends. Using machine learning, computers learn without being explicitly programmed.

Forecasts or predictions from machine learning can make apps and devices smarter. For example, when you shop online, machine learning helps recommend other products you might like based on what you've purchased. Or when your credit card is swiped, machine learning compares the transaction to a database of transactions and helps detect fraud. And when your robot vacuum cleaner vacuums a room, machine learning helps it decide whether the job is done.

Some of the most common Artificial Intelligence and Machine Learning service types in Azure are:

| Service Name                   | Description                                                                                                                                                                                                                                                  |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Azure Machine Learning Service | Cloud-based environment you can use to develop, train, test, deploy, manage, and track machine learning models. It can auto-generate a model and auto-tune it for you. It will let you start training on your local machine, and then scale out to the cloud |
| Azure Machine Learning Studio  | Collaborative, drag-and-drop visual workspace where you can build, test, and deploy machine learning solutions using pre-built machine learning algorithms and data-handling modules                                                                         |

A closely related set of products are the cognitive services. These are pre-built APIs you can leverage in your applications to solve complex problems.

| Service Name                | Description                                                                                                                              |
| --------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Vision                      | Image-processing algorithms to smartly identify, caption, index, and moderate your pictures and videos.                                  |
| Speech                      | Convert spoken audio into text, use voice for verification, or add speaker recognition to your app.                                      |
| Knowledge mapping           | Map complex information and data in order to solve tasks such as intelligent recommendations and semantic search.                        |
| Bing Search                 | Add Bing Search APIs to your apps and harness the ability to comb billions of webpages, images, videos, and news with a single API call. |
| Natural Language processing | Allow your apps to process natural language with pre-built scripts, evaluate sentiment and learn how to recognize what users want.       |

#### DevOps

DevOps (Development and Operations) brings together people, processes, and technology, automating software delivery to provide continuous value to your users. Azure DevOps Services allows you to create build and release pipelines that provide continuous integration, delivery, and deployment for your applications. You can integrate repositories and application tests, perform application monitoring, and work with build artifacts. You can also work with and backlog items for tracking, automate infrastructure deployment and integrate a range of third-party tools and services such as Jenkins and Chef. All of these functions and many more are closely integrated with Azure to allow for consistent, repeatable deployments for your applications to provide streamlined build and release processes.

Some of the main DevOps services available with Azure are Azure DevOps Services and Azure DevTest Labs.

| Service Name       | Description                                                                                                                                                                                                                                                                    |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Azure DevOps       | Azure DevOps Services (formerly known as Visual Studio Team Services, or VSTS), provides development collaboration tools including high-performance pipelines, free private Git repositories, configurable Kanban boards, and extensive automated and cloud-based load testing |
| Azure DevTest Labs | Quickly create on-demand Windows and Linux environments you can use to test or demo your applications directly from your deployment pipelines                                                                                                                                  |

### Exercise - Create a website hosted in Azure

As a technology professional, you likely have expertise in a specific area. Perhaps you're a storage admin or virtualization expert, or maybe you focus on the latest security practices. If you're a student, you may still be exploring what interests you most.

No matter your role, most people get started with the cloud by creating a website. Here you'll deploy a website hosted in an App Service.

Let's review some basic terms and get your first website up and running.

#### What is an App Service?

Azure App Service is an HTTP-based service that enables you to build and host many types of web-based solutions without managing infrastructure. For example, you can host web apps, mobile back ends, and RESTful APIs in several supported programming languages. Applications developed in .NET, .NET Core, Java, Ruby, Node.js, PHP, or Python can run in and scale with ease on both Windows and Linux-based environments.

We aim to create a website in less than the time it takes to eat lunch. Therefore, we're not going to write any code and will instead deploy a predefined application from the Microsoft Azure Marketplace.

#### What is the Microsoft Azure Marketplace?

The Microsoft Azure Marketplace is an online store that hosts applications that are certified and optimized to run in Azure. Many types of applications are available, ranging from AI + Machine Learning to Web applications. As you'll see in a couple of minutes, deployments from the store are done via the Azure portal using a wizard-style user interface. This user interface makes evaluating different solutions easy.

We're going to use one of the WordPress application options from the Azure Marketplace for our website.

#### Creating resources in Azure

Typically, the first thing we'd do is to create a resource group to hold all the things that we need to create. The resource group allows us to administer all the services, disks, network interfaces, and other elements that potentially make up our solution as a unit. We can use the Azure portal to create and manage our solution's resource groups. However, keep in mind that you can also manage resources via a command line using the Azure CLI. The Azure CLI is a useful option should you need to automate the process in the future.

In the free Azure sandbox environment, you'll use the pre-created resource group learn-dec5e429-ff20-471f-be7f-38c796a4bf70, and you don't need to do this step.

#### Choosing a location

The free sandbox allows you to create resources in a subset of the Azure global regions. Select a region from this list when you create resources:

- westus2
- southcentralus
- centralus
- eastus
- westeurope
- southeastasia
- japaneast
- brazilsouth
- australiasoutheast
- centralindia

#### Create a WordPress website

1. If you haven't already, verify that you have activated the sandbox above. Activating the sandbox will allocate the subscription and resource group you will use in this exercise. This step is required for any Microsoft Learn exercises that use a sandbox.

2. Sign in to the Azure portal using the same account you activated the sandbox with.

> Note<br>
> When using the Microsoft Learn Sandbox, it's always good to verify you are in the Microsoft Learn Sandbox directory when working within the Azure portal. The directory name is listed either under your email at the top of page or above your account information when you select the user icon. Additionally, make sure you have activated the sandbox at the start of any exercise that uses the sandbox. This will ensure you are allocated an Azure subscription and your own resource group to use for resources created in the exercise

3. Expand the left-hand navigation panel.

4. From the top of the Azure portal navigation list, select Create a resource. <br>
   This option takes you to the Azure Marketplace.
5. The Azure Marketplace has many services, solutions, and resources available for you to use. Since we know that we want to install WordPress, we can do a quick search for it. In the Search the Marketplace box above the listed application options, type in WordPress. Select the default WordPress option from the list of options available.

6. In the newly presented panel, you'll typically find additional information about the item you're about to install, including the publisher, a brief description of the resource, and links to more information. Make sure to review this information. Select Create to begin the process to create a WordPress app.

7. Next, you're presented several options to configure your deployment. Enter the following information:

- App Name: Choose a unique value for the App name. It will form part of a Fully Qualified Domain Name (FQDN).
- Subscription: Make sure the Concierge Subscription is selected.
- Resource Group: Select the Use existing radio button, then select the learn-dec5e429-ff20-471f-be7f-38c796a4bf70 resource group from the drop-down list.
- Database Provider: Select MySQL in App.
- App Service plan/location: You'll change the App Service plan in the next step.
- Application Insights: Leave at the default configuration.
  > Note <br>
  > If you still see a section called Database, make sure you selected the correct Database Provider described in the configuration above.

8. Now let's configure the App Service plan to use a specific pricing tier. The App Service plan specifies the compute resources and location for the web app. Select App Service plan/location.

9. In the App Service plan panel, select Create new.

10. In the New App Service plan panel, enter a name for the new service plan.

11. For Location, pick Central US to make sure we pick a region that allows the service plan you will pick. Normally, you would pick the region that is closest to your customers while offering the services you need.

12. Select Pricing tier to see the performance and feature options of the various types of service plans.

13. The Spec Picker allows us to select a new pricing tier for our application. This screen opens to the Production tab, with the S1 pricing tier selected. We'll select a new pricing tier from the Dev / Test tab for our website.

Select the Dev / Test tab and select the F1 pricing tier. Then select Apply.

14. Back on the New App Service plan panel, select OK to create the new plan and close the panel.

15. Finally, select the Create button to start the deployment of your new site.

> Note <br>
> If you encounter an issue creating the resources, verify you've selected the F1 pricing tier in the new App Service plan. This is a requirement of the sandbox system when creating this WordPress site.

#### Verify your website is running

The deployment of the new website can take a few minutes to complete, and you're welcome to explore the portal further on your own.

We can track the progress of the deployment at any time.

1. Select the notification bell icon at the top of the portal. If your browser window width is smaller, it may be shown when you click on the ellipsis (...) icon at the top right.

2. Select Deployment in progress... to see the details about all the resources that are created.

Notice how resources are listed as they're created and the status changes to a green check as each component in the deployment completes.

3. Once the deployment status message changes to Your deployment is complete, you'll notice the status in the notification dialogue changes to Deployment succeeded. Select Go to resource to navigate to the App Service overview.

Find the URL in the Overview section.

Copy the URL information. Open a new tab in your browser and use the information to browse to your new WordPress site. You can now configure your WordPress website and add content.

### Exercise - Configure an App Service

Recall from earlier, that we're using an App Service to run our WordPress application. Here we'll look at additional information exposed about our application and explore some of the available options to configure our website.

Let's have a look at some of this information.

1. Open the Azure portal .
2. From the left-hand navigation menu, select Dashboard to access a list of all resources in your subscription. You may have to click the menu icon to show the navigation choices.

3. Select the App Service with the name you chose it in the previous exercise.

4. Scroll down in the overview view to where you can see the graphs for your newly created website. These graphs provide statistics about the number of requests received by our website, the amount of data in, data out, and the number of errors encountered on the site.
   The information displayed here is near real-time data and gives a quick overview of the performance of your website. Problems with the site's performance will manifest in these graphs as early warnings.

#### What is scale?

Suppose you deployed your website and it becomes popular. By looking at the graphs in the overview, you realize that your site can't effectively manage all the requests it's receiving. To solve the problem, you'll need to increase the server's hardware capacity.

Scale refers to adding network bandwidth, memory, storage, or compute power to achieve better performance.

You may have heard the terms _scaling up_ and _scaling out_.

Scaling up, or vertical scaling means to increase the memory, storage, or compute power on an existing virtual machine. For example, you can add additional memory to a web or database server to make it run faster.

Scaling out, or horizontal scaling means to add extra virtual machines to power your application. For example, you might create many virtual machines configured in exactly the same way and use a load balancer to distribute work across them.

> **Tip** <br>
> The cloud is elastic. You could scale down or scale in your deployment if you needed to scale up or scale out only temporarily. Scaling down or scaling in can help you save money. Azure Advisor and Azure Cost Management are two services that help you optimize cloud spend. You can use these services to identify where you're using more than you need, and then scale back to the capacity you're actually using.

When you have more time, feel free to go through each section and explore the various options available.

#### How to change the App Service configuration

The App Services has many configurable options available and groups these options in sections of functionality.

The first section displayed is a group of common options you'd access to get a view of the health of your application. However, each following section provides additional functionality and information.

For example, the Settings section gives you access to configure various aspects such as application settings, backups, custom domains, TLS/SSL settings, options to scale up the resources of the application, and so on.

#### Scale up your App Service

1. In the Settings configuration section for your app service, select Scale up (App service plan).

2. Notice that there are three workload categories to choose from in the configuration pane. These three categories make it easier to decide the type of workload we'll run.

| Category   | Description                                                                                                                                                                                                                                                                              |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Dev / Test | This category is ideal for less demanding workloads. This category is predominantly focused on providing shared infrastructure. In this category, you have additional features that become available to the App Service application. For example, Custom domains / SSL and manual scale. |
| Production | This category is ideal for more demanding workloads. In this category, you'll also notice added features such as staging slots, daily backups, and a traffic manager.                                                                                                                    |
| Isolated   | This category is ideal for workloads that require advanced networking and fine-grained scaling.                                                                                                                                                                                          |

Within each category, there are pricing tiers that will allow us to scale the resources available to our App service. These pricing tiers give us access to the additional features mentioned above.

We'll leave the configuration on the F1 tier, but know that this pane is where you can go to make scaling adjustments in your app service if you have changes in load for your application.

Let's now take a look at how to use the Cloud Shell to configure Azure resources, such as App Service.

### Exercise - Access an App Service using Azure Cloud Shell

#### What is Azure Cloud Shell?

Azure Cloud Shell is a browser-based command-line experience for managing and developing Azure resources. Think of Cloud Shell as an interactive console that you run in the cloud.

Cloud Shell provides two experiences to choose from, Bash and PowerShell. Both include access to the Azure command-line interface called Azure CLI and to Azure PowerShell.

You can use any Azure management interface, including the Azure portal, Azure CLI, and Azure PowerShell, to manage Azure resources. For learning purposes, here you'll use the Azure CLI to start and stop the WordPress site we created earlier.

Suppose you have several websites deployed and want to stop or start each of these websites without accessing each App service individually using the portal. This effort is an easy task that you can convert into a script using Cloud Shell and Azure CLI.

In this exercise, you'll use the Cloud Shell window shown side by side with the exercise instructions. When normally accessing the Cloud Shell from within the Azure portal, you'll click the Cloud Shell icon from the top navigation bar. This icon is sometimes within the ellipsis (...) menu icon next to your profile.

For this exercise, we'll use the Cloud Shell experience as part of our sandbox implementation.

> Tip <br>
> You can use the Copy button to copy commands to the clipboard. To paste, right-click on a new line in the Cloud Shell window and select Paste or use the Shift+Insert keyboard shortcut (⌘+V on macOS).

1. Our first step is to make sure that we work with the correct Azure subscription before we change any settings. We'll use the az account list list command. By default, the command returns a JSON string. However, we'll format the output as a table to make the information easier to work with. Run the following command.

> `az account list --output table`

2. Recall that we used a pre-created resource group called learn-dec5e429-ff20-471f-be7f-38c796a4bf70 when we created our website. However, if you ever need to list all the resource groups in a subscription, then you'll run the az group list command.

   > `az group list --output table`

3. Next, we'll list all the resources in the learn-dec5e429-ff20-471f-be7f-38c796a4bf70 using the az resource list command. The command will return a list of resources. By specifying, --resource-type we can filter the result to include only the resource information related to websites.

Run the following command.

> ```Azure CLI
> az resource list \
>   --resource-group learn-dec5e429-ff20-471f-be7f-38c796a4bf70 \
>   --resource-type Microsoft.Web/sites
> ```

Here an example of the command's output:

> ```JSON
> {
> "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx/resourceGroups/>learn-dec5e429-ff20-471f-be7f-38c796a4bf70/providers/Microsoft.Web/sites/BlogFor",
> "identity": null,
> "kind": "app",
> "location": "centralus",
> "managedBy": null,
> "name": "MyWebApp",
> "plan": null,
> "properties": null,
> "resourceGroup": "learn-dec5e429-ff20-471f-be7f-38c796a4bf70",
> "type": "Microsoft.Web/sites"
> }
> ```

Copy the value of name. We'll use it in the next steps to first stop and then start our website.

4. We'll use the az webapp stop command to stop the web application running in our app service. Replace <web app name> with the name of your web app you copied, then run this command to stop your web app.

> ```Azure CLI
> az webapp stop \
>   --resource-group learn-dec5e429-ff20-471f-be7f-38c796a4bf70 \
>   --name AzureHostApplication
> ```

5. Open the website in a new browser tab. You'll find the URL to the site in the overview of the App service in the portal. You'll see a message in your browser that reads:

6. Finally, let's start the web app by running the az webapp start command. Replace <web app name> with the name of your web app you copied, then run this command to start your web app.

> ```Azure CLI
> az webapp start \
>   --resource-group learn-dec5e429-ff20-471f-be7f-38c796a4bf70 \
>   --name AzureHostApplication
> ```

7. Switch back to the tab for your website and refresh the page. Your website will be available after a couple of seconds.
