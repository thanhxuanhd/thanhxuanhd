#  Coud Concepts - Principles of cloud computing 

##  2 What is cloud computing?
__(cloud computing - Điện toán đám mây)__

Cloud computing is renting resources (thuê tài nguyên), like storage space or CPU cycles, on another company's computers. You only pay for what you use. The company providing these services is referred (giới thiệu) to as a cloud provider. Some example providers are Microsoft, Amazon, and Google.

The cloud provider is responsible for the physical hardware required to execute your work, and for keeping it up-to-date. The computing services offered tend(xu hướng) to vary(thay đổi) by cloud provider. However, typically they include:

 * __Compute power__ - such(chẳng hạn) as Linux servers or web applications used for computation and processing tasks
 * __Storage__ - such as files and databases
* __Networking__ - such as secure connections between the cloud provider and your company
* __Analytics__ - such as visualizing telemetry and performance data

###  1.1 Cloud computing services
The goal of cloud computing is to make running a business easier and more efficient (hiệu quả), whether it's a small start-up or a large enterprise. Every business is unique and has different needs. To meet those needs, cloud computing providers offer a wide range of services.

You need to have a basic understanding of some of the services it provides. Let's briefly discuss the two most common services that all cloud providers offer – _compute power_ and _storage_.

### 1.2 Compute power

When you send an email, book a reservation(dự phòng) on the Internet, pay a bill online, or even take this Microsoft Learn module you're interacting with cloud-based servers that are processing each request and returning a response. As a consumer, we're all dependent (phụ thuộc) on the computing services provided by the various cloud providers that make up the Internet. 

When you build solutions using cloud computing, you can choose how you want work to be done based on your resources and needs. For example, if you want to have more control and responsibility over maintenance, you could create a virtual machine (VM). A VM is an emulation of a computer - just like your desktop or laptop you're using now. Each VM includes an operating system and hardware that appears to the user like a physical computer running Windows or Linux. You can then install whatever software you need to do the tasks you want to run in the cloud.

The difference is that you don't have to buy any of the hardware or install the OS. The cloud provider runs your virtual machine on a physical server in one of their datacenters - often sharing that server with other VMs (isolated and secure).  __With the cloud, you can have a VM ready to go in minutes at less cost than a physical computer__.

VMs aren't the only computing choice - there are two other popular options: _containers_ and _serverless computing_.

### 1.3 What are containers?

__Containers__ provide a consistent, isolated execution (chấp hành) environment for applications. They're similar to VMs except they don't require a guest operating system. Instead, the application and all its dependencies is packaged into a "container" and then a standard runtime environment is used to execute the app. This allows the container to start up in just a few seconds, because there's no OS to boot and initialize. You only need the app to launch.

The open-source project, Docker, is one of the leading(dẫn đầu) platforms for managing containers. Docker containers provide an efficient(có hiệu quả), lightweight approach (tiếp cận) to application deployment because they allow different components of the application to be deployed independently into different containers. Multiple containers can be run on a single machine, and containers can be moved between machines. The portability (tính di động) of the container makes it easy for applications to be deployed in multiple environments, either on-premises(tại chỗ) or in the cloud, often with no changes to the application.

### 1.4 What is serverless computing?

__Serverless computing__ lets you run application code without creating, configuring (cấu hình), or maintaining a server. The core idea is that your application is broken into separate(chia ra) functions that run when triggered by some action. This is ideal for automated tasks - for example, you can build a serverless process that automatically sends an email confirmation after a customer makes an online purchase.

The serverless model differs from VMs and containers in that you only pay for the processing time used by each function as it executes. VMs and containers are charged while they're running - even if the applications on them are idle. This architecture doesn't work for every app - but when the app logic can be separated to independent units, you can test them separately, update them separately, and launch them in microseconds, making this approach the fastest option for deployment.

### 1.5 Storage

Most devices and applications read and/or write data. Here are some examples:

*  Buying a movie ticket online
*  Looking up the price of an online item
*  Taking a picture
*  Sending an email
*  Leaving a voicemail

In all of these cases, data is either _read_ (looking up a price) or _written_ (taking a picture). The type of data and how it's stored can be different in each of these cases.

Cloud providers typically offer services that can handle all of these types of data. For example, if you wanted to store text or a movie clip, you could use a file on disk. If you had a set of relationships such as an address book, you could take a more structured approach like using a database.

The advantage(lợi thế ) to using cloud-based data storage is you can scale to meet your needs. If you find that you need more space to store your movie clips, you can pay a little more and add to your available space. In some cases, the storage can even expand and contract automatically - so you pay for exactly what you need at any given point in time.

### 1.6 Summary
Every business has different needs and requirements. Cloud computing is __flexible__  and __cost-efficient__ (hiệu quả chi phí), which can be beneficial to every business, whether it's a small start-up or a large enterprise.

## 2. Benefits of cloud computing

Cloud computing isn't an all-or-nothing service approach. Companies can choose to use the cloud to store their data and execute logic as much, or as little, as necessary to fulfill their business requirements. Existing businesses might choose a gradual movement to save money on infrastructure and administration costs (referred to as "lift and shift"), while a new company might start in the cloud.

Let's learn some of the top benefits of cloud computing.

### 2.1 It's cost-effective

Cloud computing provides a __pay-as-you-go__ or __consumption-based__ pricing model.

This consumption-based model brings with it many benefits, including:

* No upfront(trả trước) infrastructure (cơ sở hạ tầng) costs
* No need to purchase and manage costly infrastructure that you may not use to its fullest
* The ability to pay for additional resources only when they are needed
* The ability to stop paying for resources that are no longer needed

This also allows for better cost prediction. Prices for individual resources and services are provided so you can predict how much you will spend in a given billing period based on your expected usage. You can also perform analysis based on future growth using historical usage data tracked by your cloud provider.

### 2.2 It's scalable

You can increase(tăng) or decrease(giảm) the resources and services used based on the demand(nhu cầu) or workload at any given time. Cloud computing supports both vertical and horizontal scaling depending on your needs.

__Vertical scaling__, also known as "scaling up", is the process of adding resources to increase the power of an existing server. Some examples of vertical scaling are: adding more CPUs, or adding more memory.

__Horizontal scaling__, also known as "scaling out", is the process of adding more servers that function together as one unit. For example, you have more than one server processing incoming requests.

Scaling can be done manually or automatically based on specific triggers such as CPU utilization or the number of requests and resources that can be allocated or de-allocated in minutes.

### 2.3 It's elastic (đàn hồi)

As your workload changes due(đến hạn) to a spike or drop in demand, a cloud computing system can compensate by automatically adding or removing resources.

For example, imagine your website is featured in a news article, leading to a spike in traffic overnight. Since the cloud is elastic, it automatically allocates more computing resources to handle the increased traffic. When the traffic begins to normalize, the cloud automatically de-allocates the additional resources to minimize cost.

Another example is if you are running an application used by employees, you can have the cloud automatically add resources for the peak operating hours during which most people access the application, and remove the resources at the usual end of the day.

### 2.4 It's current

When you use the cloud, you're able to focus on what matters: building and deploying applications. Cloud usage eliminates the burdens of maintaining software patches, hardware setup, upgrades, and other IT management tasks. All of this is automatically done for you to ensure you're using the latest and greatest tools to run your business.

Additionally, the computer hardware is maintained and upgraded by the cloud provider. For example, if a disk fails, the disk will be replaced by the cloud provider. If a new hardware update becomes available, you don't have to go through the process of replacing your hardware. The cloud provider will ensure that the hardware updates are made available to you automatically.

### 2.5 It's reliable (tin cậy)
When you're running a business, you want to be confident your data is always going to be there. Cloud computing providers offer data backup, disaster recovery, and data replication services to make sure your data is always safe. In addition, redundancy is often built into cloud services architecture so if one component fails, a backup component takes its place. This is referred to as __fault tolerance__ and it ensures that your customers aren't impacted when a disaster occurs.

### 2.6 It's global

Cloud providers have fully redundant(dư thừa) datacenters located in various regions all over the globe. This gives you a local presence close to your customers to give them the best response time possible no matter where in the world they are.

You can replicate your services into multiple regions for redundancy and locality, or select a specific region to ensure you meet data-residency and compliance laws for your customers.

### 2.7 It's secure

Think about how you secure your datacenter. You have _physical security_ – who can access the building, who can operate the server racks, and so on. You also have _digital security_ – who can connect to your systems and data over the network.

Cloud providers offer a broad set of policies, technologies, controls, and expert technical skills that can provide better security than most organizations can otherwise achieve. The result is strengthened security, which helps to protect data, apps, and infrastructure from potential threats.

When it comes to physical security – threats to cloud infrastructure, cloud providers invest heavily in walls, cameras, gates, security personnel, and so on, to protect physical assets. They also have strict procedures in place to ensure employees have access only to those resources that they've been authorized to manage.

Let us talk about digital security. You want only authorized users to be able to log into virtual machines or storage systems running in the cloud. Cloud providers offer tools that help you mitigate security threats, and you must use these tools to protect the resources you use.

### 2.8 Summary

Cloud computing makes running a business easier. It's cost-effective, scalable, elastic, current, reliable, and secure. This means you're able to spend more time on what matters and less time managing the underlying details.