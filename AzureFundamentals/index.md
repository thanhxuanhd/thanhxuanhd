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

## Compliance terms (tuân thủ nguyên tắc) and requirements

When selecting a cloud provider to host your solutions, you should understand how that provider can help you comply with regulations and standards. Some questions to ask about a potential provider include:

* How compliant is the cloud provider when it comes to handling sensitive data?
* How compliant are the services offered by the cloud provider?
* How can I deploy my own cloud-based solutions to scenarios that have accreditation(công nhận) or compliance requirements?
* What terms are part of the privacy statement for the provider?

### Compliance Offerings
The following list provides details about some of the compliance offerings available.

* __Criminal Justice Information Services (CJIS)__. Any US state or local agency that wants to access the FBI's CJIS database is required to adhere to the CJIS Security Policy. Azure is the only major cloud provider that contractually commits to conformance with the CJIS Security Policy, which commits Microsoft to adhering to the same requirements that law enforcement and public safety entities must meet.

* __Cloud Security Alliance (CSA) STAR Certification__. Azure, Intune, and Microsoft Power BI have obtained STAR Certification, which involves a rigorous independent third-party assessment of a cloud provider's security posture. This STAR certification is based on achieving ISO/IEC 27001 certification and meeting criteria specified in the Cloud Controls Matrix (CCM). This certification demonstrates that a cloud service provider:

    *   Conforms to the applicable requirements of ISO/IEC 27001.
    * Has addressed issues critical to cloud security as outlined in the CCM.
    *  Has been assessed against the STAR Capability Maturity Model for the management of activities in CCM control areas.

* __General Data Protection Regulation (GDPR)__  As of May 25, 2018, a European privacy law — GDPR — is in effect. GDPR imposes new rules on companies, government agencies, non-profits, and other organizations that offer goods and services to people in the European Union (EU), or that collect and analyze data tied to EU residents. The GDPR applies no matter where you are located.

* __EU Model Clauses__. Microsoft offers customers EU Standard Contractual Clauses that provide contractual guarantees around transfers of personal data outside of the EU. Microsoft is the first company to receive joint approval from the EU's Article 29 Working Party that the contractual privacy protections Azure delivers to its enterprise cloud customers meet current EU standards for international transfers of data. This ensures that Azure customers can use Microsoft services to move data freely through Microsoft's cloud from Europe to the rest of the world.

* __Health Insurance Portability and Accountability Act (HIPAA)__. HIPAA is a US federal law that regulates patient Protected Health Information (PHI). Azure offers customers a HIPAA Business Associate Agreement (BAA), stipulating adherence to certain security and privacy provisions in HIPAA and the __Health Information Technology for Economic and Clinical Health (HITECH) Act__. To assist customers in their individual compliance efforts, Microsoft offers a BAA to Azure customers as a contract addendum.

* __International Organization for Standardization__ (ISO) and the International Electrotechnical Commission (IEC) 27018. Microsoft is the first cloud provider to have adopted the ISO/IEC 27018 code of practice, covering the processing of personal information by cloud service providers.

* __Multi-Tier Cloud Security__ (MTCS) Singapore. After rigorous assessments conducted by the MTCS Certification Body, Microsoft cloud services received MTCS 584:2013 certification across all three service classifications:

    * Infrastructure as a Service (IaaS)
    * Platform as a Service (PaaS)
    * Software as a Service (SaaS)

* __Service Organization Controls (SOC) 1, 2, and 3__. Microsoft-covered cloud services are audited at least annually against the SOC report framework by independent third-party auditors. The Microsoft cloud services audit covers controls for data security, availability, processing integrity, and confidentiality as applicable to in-scope trust principles for each service.

* __National Institute of Standards and Technology (NIST) Cybersecurity Framework (CSF)__. NIST CSF is a voluntary Framework that consists of standards, guidelines, and best practices to manage cybersecurity-related risks. Microsoft cloud services have undergone independent, third-party Federal Risk and Authorization Management Program (FedRAMP) Moderate and High Baseline audits, and are certified according to the FedRAMP standards. Additionally, through a validated assessment performed by the Health Information Trust Alliance (HITRUST), a leading security and privacy standards development and accreditation organization, Office 365 is certified to the objectives specified in the NIST CSF.

* __UK Government G-Cloud__. The UK Government G-Cloud is a cloud computing certification for services used by government entities in the United Kingdom. Azure has received official accreditation from the UK Government Pan Government Accreditor.

## Economies of scale

_Economies of scale_ is the ability to do things more efficiently(hiệu quả) or at a lower-cost per unit when operating at a larger scale. This cost advantage is an important benefit in cloud computing.

Cloud providers such as Microsoft, Google, and Amazon are large businesses leveraging the benefits of economies of scale. These providers can then pass the savings on to their customers.

These savings are apparent to end users in a number of ways, one of which is the ability to acquire hardware at a lower cost. Cloud providers can also make deals with local governments and utilities to get tax savings, lowering the price of power, cooling, and high-speed network connectivity between sites. Cloud providers are then able to pass on these benefits to end users in the form of lower prices than what you could achieve on your own.

## Capital expenditure (CapEx) versus operational expenditure (OpEx)

In the past, companies needed to acquire physical premises and infrastructure to start their business. There was a substantial(đáng kể) up-front cost in hardware and infrastructure to start or grow a business. Cloud computing provides services to customers without significant(có ý nghĩa) upfront costs or equipment setup time.

These two approaches(tiếp cận) to investment are referred to as:

* __Capital Expenditure(Vốn) (CapEx)__: CapEx is the spending of money on physical infrastructure up front, and then deducting that expense from your tax bill over time. CapEx is an upfront cost, which has a value that reduces over time.

* __Operational Expenditure(Chi phí hoạt động) (chi phí) (OpEx)__: OpEx is spending money on services or products now and being billed for them now. You can deduct this expense from your tax bill in the same year. There's no upfront cost. You pay for a service or product as you use it.

###  CapEx computing costs
A typical on-premises datacenter includes costs such as:

#### __Server costs__

This area includes all hardware components and the cost of supporting them. When purchasing servers, make sure to design fault tolerance and redundancy, such as server clustering, redundant power supplies, and uninterruptible power supplies. When a server needs to be replaced or added to a datacenter, you need to pay for the computer. This can affect your immediate cash flow because you must pay for the server up front.

####  __Storage costs__

This area includes all storage hardware components and the cost of supporting it. Based on the application and level of fault tolerance, centralized storage can be expensive. For larger organizations, you can create tiers of storage where more expensive fault‐tolerant storage is used for critical applications and lower expense storage is used for lower priority data.

#### __Network costs__

Networking costs include all on-premises hardware components, including cabling, switches, access points, and routers. This also includes wide area network (WAN) and Internet connections.

#### __Backup and archive costs__

This is the cost to back up, copy, or archive data. Options might include setting up a backup to or from the cloud. There's an upfront cost for the hardware and additional costs for backup maintenance and consumables like tapes.

#### __Organization continuity and disaster recovery costs__

Along with server fault tolerance and redundancy, you need to plan for how to recover from a disaster and continue operating. Your plan should consist of creating a disaster recovery site. It could also include backup generators. Most of these are upfront costs, especially if you build a disaster recovery site, but there's an additional ongoing cost for the infrastructure and its maintenance.

#### __Datacenter infrastructure costs__

These are costs for construction and building equipment, as well as future renovation and remodeling costs that may arise as demands grow. Additionally, this infrastructure incurs operational expenses for electricity, floor space, cooling, and building maintenance.

#### __Technical personnel__

While not a capital expenditure, the personnel required to work on your infrastructure are specific to on-premises datacenters. You will need the technical expertise and workforce to install, deploy, and manage the systems in the datacenter and at the disaster recovery site.

### OpEx cloud computing costs

With cloud computing, many of the costs associated with an on-premises datacenter are shifted to the service provider. Instead of thinking about physical hardware and datacenter costs, cloud computing has a different set of costs. For accounting purposes, all these costs are operational expenses:

#### __Leasing software and customized features__

Using a pay-per-use model requires actively managing your subscriptions to ensure users do not misuse the services, and that provisioned accounts are being utilized and not wasted. As soon as the provider provisions resources, billing starts. It is your responsibility to de-provision the resources when they aren't in use so that you can minimize costs.

#### __Scaling charges based on usage/demand instead of fixed hardware or capacity.__

Cloud computing can bill in various ways, such as the number of users or CPU usage time. However, billing categories can also include allocated RAM, I/O operations per second (IOPS), and storage space. Plan for backup traffic and disaster recovery traffic to determine the bandwidth needed.

#### __Billing at the user or organization level.__

The subscription (pay-per-use) model is a computing billing method that is designed for both organizations and users. The organization or user is billed for the services used, typically on a recurring basis. You can scale, customize, and provision computing resources, including software, storage, and development platforms. For example, when using a dedicated cloud service, you could pay based on server hardware and usage.

#### __Benefits of CapEx__

With capital expenditures, you plan your expenses at the start of a project or budget period. Your costs are fixed, meaning you know exactly how much is being spent. This is appealing when you need to predict the expenses before a project starts due to a limited budget.

#### __Benefits of OpEx__
Demand and growth can be unpredictable and can outpace expectation, which is a challenge for the CapEx model as shown in the following graph.

With the OpEx model, companies wanting to try a new product or service don't need to invest in equipment. Instead, they pay as much or as little for the infrastructure as required.

OpEx is particularly appealing if the demand fluctuates or is unknown. Cloud services are often said to be agile. Cloud agility is the ability to rapidly change an IT infrastructure to adapt to the evolving needs of the business. For example, if your service peaks one month, you can scale to demand and pay a larger bill for the month. If the following month the demand drops, you can reduce the used resources and be charged less. This agility lets you manage your costs dynamically, optimizing spending as requirements change.