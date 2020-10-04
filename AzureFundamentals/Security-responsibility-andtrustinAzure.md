### Introduction

Every system, architecture, and application needs to be designed with security in mind. There's too much at risk. For instance, a denial of service attack could prevent your customer from reaching your web site or services and block you from doing business. Defacement of your website damages your reputation(uy tín). And a data breach could be even worse — as it can ruin hard-earned trust, while causing significant personal and financial harm. As administrators, developers, and IT management, we all must work to guarantee the security of our systems.

Let's say you work at a company called Contoso Shipping, and you're spearheading(mũi nhọn) the development of drone deliveries(__N__) in rural areas-while having truck drivers leverage mobile apps to deliver to urban areas. You're in the process of moving much of Contoso Shipping's infrastructure to the cloud to maximize efficiency, as well as moving several physical servers in the company's data center to Azure virtual machines. Your team plans on creating a hybrid solution, with some of the servers remaining on-premises, so you'll need a secure, high-quality connection between the new virtual machines and the existing network.

Additionally, Contoso Shipping has some out-of-network devices that are part of your operations. You are using network-enabled sensors in your drones that send data to Azure Event Hubs, while delivery drivers use mobile apps to get route maps and record signatures for receipt of shipments. These devices and apps must be securely authenticated before data can be sent to or from them.

So how do you keep your data secure?

> __Note__ <br>
>Azure Event Hubs allow you to receive and process millions of events of real-time data each second via dynamic data pipelines. Event Hubs also integrates seamlessly with other Azure services.


### Cloud security is a shared responsibility

Organizations face many challenges with securing their datacenters, including recruiting and keeping security experts, using many security tools, and keeping pace with the volume and complexity(__N__: phứ tạp) of threats.

As computing environments move from customer-controlled datacenters to the cloud, the responsibility of security also shifts(__V__: sự thay đổi). Security of the operational environment is now a concern shared by both cloud providers and customers. By shifting these responsibilities to a cloud service like Azure, organizations can reduce focus on activities that aren't core business competencies. Depending on the specific technology choices, some security protections will be built into the particular service, while addressing others will remain the customer's responsibility. To ensure that the proper security controls are provided, a careful evaluation of the services and technology choices becomes necessary.

#### Understand security threats

#### Security is a shared responsibility
The first shift you'll make is from on-premises data centers to infrastructure as a service (IaaS). With IaaS, you are leveraging(__V__: đồn bẩy) the lowest-level service and asking Azure to create virtual machines (VMs) and virtual networks. At this level, it's still your responsibility to patch and secure your operating systems and software, as well as configure your network to be secure. At Contoso Shipping, you are taking advantage of IaaS when you start using Azure VMs instead of your on-premises physical servers. In addition to the operational advantages, you receive the security advantage of having outsourced concern over protecting the physical parts of the network.

Moving to platform as a service (PaaS) outsources several security concerns. At this level, Azure is taking care of the operating system and of most foundational software like database management systems. Everything is updated with the latest security patches and can be integrated with Azure Active Directory for access controls. PaaS also comes with many operational advantages. Rather than building whole infrastructures and subnets for your environments by hand, you can "point and click" within the Azure portal or run automated scripts to bring complex, secured systems up and down, and scale them as needed. Contoso Shipping uses Azure Event Hubs for ingesting telemetry(__N__:từ xa) data from drones and trucks — as well as a web app with an Azure Cosmos DB back end with its mobile apps — which are all examples of PaaS.

With software as a service (SaaS), you outsource almost everything. SaaS is software that runs with an internet infrastructure. The code is controlled by the vendor but configured to be used by the customer. Like so many companies, Contoso Shipping uses Microsoft 365 (formerly Office 365), which is a great example of SaaS!

For all cloud deployment types, you own your data and identities. You are responsible for helping secure your data and identities, your on-premises resources, and the cloud components you control (which vary by service type).

Regardless of the deployment type, you always retain responsibility for the following items:

* Data
* Endpoints
* Accounts
* Access management


#### Azure security: you versus the cloud

#### A layered approach to security

Defense in depth is a strategy that employs a series of mechanisms(__N__: cơ chế) to slow the advance of an attack aimed at acquiring(mua lại) unauthorized access to information. Each layer provides protection so that if one layer is breached, a subsequent layer is already in place to prevent further exposure. Microsoft applies a layered approach to security, both in physical data centers and across Azure services. The objective of defense in depth is to protect and prevent information from being stolen by individuals who are not authorized to access it.

Defense in depth can be visualized as a set of concentric rings, with the data to be secured at the center. Each ring adds an additional layer of security around the data. This approach removes reliance(dựa dẫm) on any single layer of protection and acts to slow down an attack and provide alert telemetry that can be acted upon, either automatically or manually. Let's take a look at each of the layers.
> [Data] > [Application] > [Compute] > [Networking] > [Perimeter] > [Identity and access] > [Physical security]

##### Data

In almost all cases, attackers are after data:

* Stored in a database
* Stored on disk inside virtual machines
* Stored on a SaaS application such as Microsoft 365
* Stored in cloud storage

It's the responsibility of those storing and controlling access to data to ensure that it's properly secured. Often, there are regulatory requirements that dictate the controls and processes that must be in place to ensure the confidentiality, integrity, and availability of the data.

##### Application

* Ensure applications are secure and free of vulnerabilities.
* Store sensitive application secrets in a secure storage medium.
* Make security a design requirement for all application development.

Integrating security into the application development life cycle will help reduce the number of vulnerabilities(lỗi hổng) introduced in code. We encourage(khuyến khích) all development teams to ensure their applications are secure by default, and that they're making security requirements non-negotiable.

##### Compute
* Secure access to virtual machines.
* Implement endpoint protection and keep systems patched and current.

Malware, unpatched systems, and improperly secured systems open your environment to attacks. The focus in this layer is on making sure your compute resources are secure, and that you have the proper controls in place to minimize security issues.

##### Networking

* Limit communication between resources.
* Deny by default.
* Restrict inbound internet access and limit outbound, where appropriate.
* Implement secure connectivity to on-premises networks.

At this layer, the focus is on limiting the network connectivity across all your resources to allow only what is required. By limiting this communication, you reduce the risk of lateral movement throughout your network.

##### Perimeter

* Use distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for end users.
* Use perimeter firewalls to identify and alert on malicious attacks against your network.

At the network perimeter, it's about protecting from network-based attacks against your resources. Identifying these attacks, eliminating their impact, and alerting you when they happen are important ways to keep your network secure.

##### Identity and access
* Control access to infrastructure and change control.
* Use single sign-on and multi-factor authentication.
* Audit events and changes.

The identity and access layer is all about ensuring identities are secure, access granted is only what is needed, and changes are logged.

##### Physical security
* Physical building security and controlling access to computing hardware within the data center is the first line of defense.

With physical security, the intent is to provide physical safeguards against access to assets. These safeguards ensure that other layers can't be bypassed, and loss or theft is handled appropriately.

Azure helps alleviate your security concerns. But security is still a __shared responsibility__. How much of that responsibility falls on us depends on which model we use with Azure. We use the _defense_ in depth rings as a guideline for considering what protections are adequate for our data and environments.


### Get tips from Azure Security Center

A great place to start when examining the security of your Azure-based solutions is __Azure Security Center__. Security Center is a monitoring service that provides threat protection across all of your services both in Azure, and on-premises. Security Center can:

* Provide security recommendations based on your configurations, resources, and networks.
* Monitor security settings across on-premises and cloud workloads, and automatically apply required security to new services as they come online.
* Continuously monitor all your services, and perform automatic security assessments to identify potential vulnerabilities before they can be exploited(khai thác).
* Use machine learning to detect and block malware from being installed on your virtual machines and services. You can also define a list of allowed applications to ensure that only the apps you validate are allowed to execute.
* Analyze and identify potential inbound attacks, and help to investigate threats and any post-breach activity that might have occurred.
* Provide just-in-time access control for ports, reducing your attack surface by ensuring the network only allows traffic that you require.

Azure Security Center is part of the Center for Internet Security (CIS) recommendations.

#### Available tiers

1. Free. Available as part of your Azure subscription, this tier is limited to assessments and recommendations of Azure resources only.
2. _Standard_. This tier provides a full suite of security-related services including continuous monitoring, threat detection, just-in-time access control for ports, and more.

To access the full suite of Azure Security Center services, you will need to upgrade to a Standard tier subscription. You can access the 30-day free trial from within the Azure Security Center dashboard in the Azure portal. After the 30-day trial period is over, Azure Security Center is $15 per node per month.

#### Usage scenarios

You can integrate Security Center into your workflows and use it in many ways. Here are two examples.
1. Use Security Center for incident response.

    Many organizations learn how to respond to security incidents only after suffering an attack. To reduce costs and damage, it's important to have an incident response plan in place before an attack occurs. You can use Azure Security Center in different stages of an incident response.

    You can use Security Center during the detect, assess, and diagnose stages. Here are examples of how Security Center can be useful during the three initial incident response stages:
    
    * Detect. Review the first indication of an event investigation. For example, you can use the Security Center dashboard to review the initial verification that a high-priority security alert was raised.
    * Assess. Perform the initial assessment to obtain more information about the suspicious activity. For example, obtain more information about the security alert.
    * Diagnose. Conduct a technical investigation and identify containment, mitigation, and workaround strategies. For example, follow the remediation steps described by Security Center in that particular security alert.
2. Use Security Center recommendations to enhance security.

    You can reduce the chances of a significant security event by configuring a security policy, and then implementing the recommendations provided by Azure Security Center.
    
    A security policy defines the set of controls that are recommended for resources within that specified subscription or resource group. In Security Center, you define policies according to your company's security requirements.

    Security Center analyzes the security state of your Azure resources. When Security Center identifies potential security vulnerabilities, it creates recommendations based on the controls set in the security policy. The recommendations guide you through the process of configuring the needed security controls. For example, if you have workloads that do not require the Azure SQL _Database Transparent Data Encryption_ (TDE) policy, turn off the policy at the subscription level and enable it only in the resources groups where SQL TDE is required.

>  Important <br>
> To upgrade a subscription to the Standard tier, you must be assigned the role of _Subscription Owner, Subscription Contributor, or Security Admin_.

### Identity and access

Network perimeters, firewalls, and physical access controls used to be the primary protection for corporate data. But network perimeters have become increasingly porous with the explosion of bring your own device (BYOD), mobile apps, and cloud applications.

Identity has become the new primary security boundary. Therefore, proper authentication and assignment of privileges is critical to maintaining control of your data.

Your company, Contoso Shipping, is focused on addressing these concerns right away. Your team's new hybrid cloud solution needs to account for mobile apps that have access to secret data when an authorized user is signed in — in addition to having shipping vehicles constantly send a stream of telemetry data that is critical to optimizing the company's business.

#### Authentication and authorization
Two fundamental concepts that need to be understood when talking about identity and access control are authentication and authorization. They underpin everything else that happens and occur sequentially in any identity and access process:

* _Authentication_ is the process of establishing the identity of a person or service looking to access a resource. It involves the act of challenging a party for legitimate credentials, and provides the basis for creating a security principal for identity and access control use. It establishes if they are who they say they are.

* _Authorization_ is the process of establishing what level of access an authenticated person or service has. It specifies what data they're allowed to access and what they can do with it.

>  Note <br>
> Authentication is sometimes shortened to AuthN, and authorization is sometimes shortened to AuthZ.

Azure provides services to manage both authentication and authorization through Azure Active Directory (Azure AD).

#### What is Azure Active Directory?
Azure AD is a cloud-based identity service. It has built in support for synchronizing with your existing on-premises Active Directory or can be used stand-alone. This means that all your applications, whether on-premises, in the cloud (including Microsoft 365), or even mobile can share the same credentials. Administrators and developers can control access to internal and external data and applications using centralized rules and policies configured in Azure AD.

Azure AD provides services such as:

* __Authentication__. This includes verifying identity to access applications and resources, and providing functionality such as self-service password reset, multi-factor authentication (MFA), a custom banned password list, and smart lockout services.
* __Single-Sign-On (SSO)__. SSO enables users to remember only one ID and one password to access multiple applications. A single identity is tied to a user, simplifying the security model. As users change roles or leave an organization, access modifications are tied to that identity, greatly reducing the effort needed to change or disable accounts.
* __Application management__. You can manage your cloud and on-premises apps using Azure AD Application Proxy, SSO, the My apps portal (also referred to as Access panel), and SaaS apps.
* __Business to business (B2B) identity services__. Manage your guest users and external partners while maintaining control over your own corporate data
* __Business-to-Customer (B2C) identity services__. Customize and control how users sign up, sign in, and manage their profiles when using your apps with services.
* __Device Management__. Manage how your cloud or on-premises devices access your corporate data.

Let's explore a few of these in more detail.

#### Single sign-on

The more identities a user has to manage, the greater the risk of a credential-related security incident. More identities mean more passwords to remember and change. Password policies can vary between applications and, as complexity requirements increase, it becomes increasingly difficult for users to remember them.

Now, consider the logistics of managing all those identities. Additional strain is placed on help desks as they deal with account lockouts and password reset requests. If a user leaves an organization, tracking down all those identities and ensuring they are disabled can be challenging. If an identity is overlooked, this could allow access when it should have been eliminated.

With single sign-on (SSO), users need to remember only one ID and one password. Access across applications is granted to a single identity tied to a user, simplifying the security model. As users change roles or leave an organization, access modifications are tied to the single identity, greatly reducing the effort needed to change or disable accounts. Using single sign-on for accounts will make it easier for users to manage their identities and will increase the security capabilities in your environment.

##### SSO with Azure Active Directory
By leveraging Azure AD for SSO you'll also have the ability to combine multiple data sources into an intelligent security graph. This security graph enables the ability to provide threat analysis and real-time identity protection to all accounts in Azure AD, including accounts that are synchronized from your on-premises AD. By using a centralized identity provider, you'll have centralized the security controls, reporting, alerting, and administration of your identity infrastructure.

As Contoso Shipping integrates its existing Active Directory instance with Azure AD, you will make controlling access consistent across the organization. Doing so will also greatly simplify the ability to sign into email and Microsoft 365 documents without having to reauthenticate.

#### Multi-factor authentication

Multi-factor authentication (MFA) provides additional security for your identities by requiring two or more elements for full authentication. These elements fall into three categories:

* _Something you know_
* _Something you possess_
* _Something you are_

__Something you know__ would be a password or the answer to a security question. __Something you possess__ could be a mobile app that receives a notification or a token-generating device.__Something you are__ is typically some sort of biometric property, such as a fingerprint or face scan used on many mobile devices.

Using MFA increases security of your identity by limiting the impact of credential exposure. An attacker who has a user's password would also need to have possession of their phone or their security token generator in order to fully authenticate. Authentication with only a single factor verified is insufficient, and the attacker would be unable to use only those credentials to authenticate. The benefits this brings to security are huge, and we can't emphasize enough the importance of enabling MFA wherever possible.

Azure AD has MFA capabilities built in and will integrate with other third-party MFA providers. MFA should be used for users in the Global Administrator role in Azure AD, because these are highly sensitive accounts. All other accounts can also have MFA enabled.

For Contoso Shipping, you decide to enable MFA any time a user is signing in from a non-domain-connected computer — which includes the mobile apps your drivers use.

#### Providing identities to services
It's usually valuable for services to have identities. Often, and against best practices, credential information is embedded in configuration files. With no security around these configuration files, anyone with access to the systems or repositories can access these credentials and risk exposure.

Azure AD addresses this problem through two methods: service principals and managed identities for Azure services.
##### Service principals
To understand service principals, it's useful to first understand the words __identity__ and __principal__, because of how they are used in the identity management world.

An __identity__ is just a thing that can be authenticated. Obviously, this includes users with a user name and password, but it can also include applications or other servers, which might authenticate with secret keys or certificates.

A __principal__ is an identity acting with certain roles or claims. Usually, it is not useful to consider identity and principal separately, but think of using 'sudo' on a Bash prompt in Linux or on Windows using "run as Administrator." In both those cases, you are still logged in as the same identity as before, but you've changed the role under which you are executing. Groups are often also considered principals because they can have rights assigned.

A __service principal__ is an identity that is used by a service or application. And like other identities, it can be assigned roles.

##### Managed identities for Azure services
The creation of service principals can be a tedious(tẻ nhạt) process, and there are a lot of touch points that can make maintaining them difficult. Managed identities for Azure services are much easier and will do most of the work for you.

A managed identity can be instantly created for any Azure service that supports it—and the list is constantly growing. When you create a managed identity for a service, you are creating an account on your organization's Active Directory (a specific organization's Active Directory instance is known as an "Active Directory Tenant"). The Azure infrastructure will automatically take care of authenticating the service and managing the account. You can then use that account like any other Azure AD account, including allowing the authenticated service secure access of other Azure resources.

#### Role-based access control
Roles are sets of permissions, like "Read-only" or "Contributor", that users can be granted to access an Azure service instance.

Identities are mapped to roles directly or through group membership. Separating security principals, access permissions, and resources provides simple access management and fine-grained control. Administrators are able to ensure the minimum necessary permissions are granted.

Roles can be granted at the individual service instance level, but they also flow down the Azure Resource Manager hierarchy.

Here's a diagram that shows this relationship. Roles assigned at a higher scope, like an entire subscription, are inherited by child scopes, like service instances.

#### Privileged Identity Management

In addition to managing Azure resource access with role-based access control (RBAC), a comprehensive approach to infrastructure protection should consider including the ongoing auditing of role members as their organization changes and evolves. Azure AD Privileged Identity Management (PIM) is an additional, paid-for offering that provides oversight of role assignments, self-service, and just-in-time role activation and Azure AD and Azure resource access reviews.

#### Summary
Identity allows us to maintain a security perimeter, even outside our physical control. With single sign-on and appropriate role-based access configuration, we can always be sure who has the ability to see and manipulate our data and infrastructure.

### Encryption

For most organizations, data is the most valuable and irreplaceable asset. Encryption serves as the last and strongest line of defense in a layered security strategy.

Contoso Shipping knows that encryption is the only protection its data has once it leaves the data center and is stored on mobile devices that could potentially be hacked or stolen.

#### What is encryption?
Encryption is the process of making data unreadable and unusable to unauthorized viewers. To use or read the encrypted data, it must be _decrypted_, which requires the use of a secret key. There are two top-level types of encryption: __symmetric__ and __asymmetric__.

__Symmetric encryption__ uses the same key to encrypt and decrypt the data. Consider a desktop password manager application. You enter your passwords and they are encrypted with your own personal key (your key is often derived from your master password). When the data needs to be retrieved, the same key is used, and the data is decrypted.

__Asymmetric encryption__ uses a public key and private key pair. Either key can encrypt but a single key can't decrypt its own encrypted data. To decrypt, you need the paired key. Asymmetric encryption is used for things like Transport Layer Security (TLS) (used in HTTPS) and data signing.

Both symmetric and asymmetric encryption play a role in properly securing your data. Encryption is typically approached in two ways:

1. Encryption at rest
2. Encryption in transit

#### Encryption at rest

Data at rest is the data that has been stored on a physical medium. This data could be stored on the disk of a server, data stored in a database, or data stored in a storage account. Regardless of the storage mechanism, encryption of data at rest ensures that the stored data is unreadable without the keys and secrets needed to decrypt it. If an attacker was to obtain a hard drive with encrypted data and did not have access to the encryption keys, the attacker would not compromise the data without great difficulty.

The actual data that is encrypted could vary in its content, usage, and importance to the organization. This financial information could be critical to the business, intellectual property that has been developed by the business, personal data about customers or employees that the business stores, and even the keys and secrets used for the encryption of the data itself.

Here's a diagram that shows what encrypted customer data might look like as it sits in a database.

#### Encryption in transit

Data in transit is the data actively moving from one location to another, such as across the internet or through a private network. Secure transfer can be handled by several different layers. It could be done by encrypting the data at the application layer prior to sending it over a network. HTTPS is an example of application layer in transit encryption.

You can also set up a secure channel, like a virtual private network (VPN), at a network layer, to transmit data between two systems.

Encrypting data in transit protects the data from outside observers and provides a mechanism to transmit data while limiting risk of exposure.

This diagram shows the process. Here, customer data is encrypted as it's sent over the network. Only the receiver has the secret key that can decrypt the data to a usable form.

#### Encryption on Azure

Let's take a look at some ways that Azure enables you to encrypt data across services.
1. Encrypt raw storage

    __Azure Storage Service Encryption__ for data at rest helps you protect your data to meet your organizational security and compliance commitments. With this feature, the Azure storage platform automatically encrypts your data before persisting it to Azure Managed Disks, Azure Blob storage, Azure Files, or Azure Queue storage, and decrypts the data before retrieval. The handling of encryption, encryption at rest, decryption, and key management in Storage Service Encryption is transparent to applications using the services.
2. Encrypt virtual machine disks

    Storage Service Encryption provides low-level encryption protection for data written to physical disk, but how do you protect the virtual hard disks (VHDs) of virtual machines? If malicious attackers gained access to your Azure subscription and got the VHDs of your virtual machines, how would you ensure they would be unable to access the stored data?

    Azure Disk Encryption is a capability that helps you encrypt your Windows and Linux IaaS virtual machine disks. Azure Disk Encryption leverages the industry-standard BitLocker feature of Windows and the dm-crypt feature of Linux to provide volume encryption for the OS and data disks. The solution is integrated with Azure Key Vault to help you control and manage the disk encryption keys and secrets (and you can use managed service identities for accessing Key Vault).

    For Contoso Shipping, using VMs was one of the first moves toward the cloud. Having all the VHDs encrypted is an easy, low-impact way to ensure that you are doing all you can to secure your company's data.
3. Encrypt databases
    __Transparent data encryption (TDE)__ helps protect Azure SQL Database and Azure Data Warehouse against the threat of malicious activity. It performs real-time encryption and decryption of the database, associated backups, and transaction log files at rest without requiring changes to the application. By default, TDE is enabled for all newly deployed Azure SQL Database instances.

    TDE encrypts the storage of an entire database by using a symmetric key called the database encryption key. By default, Azure provides a unique encryption key per logical SQL Server instance and handles all the details. Bring your own key (BYOK) is also supported with keys stored in Azure Key Vault (see below).

    Because TDE is enabled by default, you are confident that Contoso Shipping has the proper protections in place for data stored in the company's databases.
4. Encrypt secrets
    We've seen that the encryption services all use keys to encrypt and decrypt data, so how do we ensure that the keys themselves are secure? Corporations may also have passwords, connection strings, or other sensitive pieces of information that they need to securely store. In Azure, we can use __Azure Key Vault__ to protect our secrets.

    Azure Key Vault is a centralized cloud service for storing your application secrets. Key Vault helps you control your applications' secrets by keeping them in a single, central location and by providing secure access, permissions control, and access logging capabilities. It is useful for a variety of scenarios:
    
    * Secrets management. You can use Key Vault to securely store and tightly control access to tokens, passwords, certificates, Application Programming Interface (API) keys, and other secrets.
    * Key management. You also can use Key Vault as a key management solution. Key Vault makes it easier to create and control the encryption keys used to encrypt your data.
    * Certificate management. Key Vault lets you provision, manage, and deploy your public and private Secure Sockets Layer/ Transport Layer Security (SSL/ TLS) certificates for your Azure, and internally connected, resources more easily.
    * Store secrets backed by hardware security modules (HSMs). The secrets and keys can be protected either by software, or by FIPS 140-2 Level 2 validated HSMs.

    The benefits of using Key Vault include:

    * Centralized application secrets. Centralizing storage for application secrets allows you to control their distribution, and reduces the chances that secrets may be accidentally leaked.
    * Securely stored secrets and keys. Azure uses industry-standard algorithms, key lengths, and HSMs, and access requires proper authentication and authorization.
    * Monitor access and use. Using Key Vault, you can monitor and control access to company secrets.
    * Simplified administration of application secrets. Key Vault makes it easier to enroll and renew certificates from public Certificate Authorities (CAs). You can also scale up and replicate content within regions, and use standard certificate management tools.
    * Integrate with other Azure services. You can integrate Key Vault with storage accounts, container registries, event hubs, and many more Azure services.

Because Azure AD identities can be granted access to use Azure Key Vault secrets, applications with managed service identities enabled can automatically and seamlessly acquire the secrets they need.

#### Summary
As you may know, encryption is often the last layer of defense from attackers and is an important piece of a layered approach to securing your systems. Azure provides built-in capabilities and services to encrypt and protect data from unintended exposure. Protection of customer data stored within Azure services is of paramount importance to Microsoft and should be included in any design. Foundational services such as Azure Storage, Azure Virtual Machines, Azure SQL Database, and Azure Key Vault can help secure your environment through encryption.

### Overview of Azure certificates

As mentioned previously, Transport Layer Security (TLS) is the basis for encryption of website data in transit. TLS uses _certificates_ to encrypt and decrypt data. However, these certificates have a lifecycle that requires administrator management. A common security problem with websites is having expired TLS certificates that open security vulnerabilities.

Certificates used in Azure are __x.509__ v3 and can be signed by a trusted certificate authority, or they can be self-signed. A self-signed certificate is signed by its own creator; therefore, it is not trusted by default. Most browsers can ignore this problem. However, you should only use self-signed certificates when developing and testing your cloud services. These certificates can contain a private or a public key and have a thumbprint that provides a means to identify a certificate in an unambiguous way. This thumbprint is used in the Azure configuration file to identify which certificate a cloud service should use.

#### Types of certificates
Certificates are used in Azure for two primary purposes and are given a specific designation based on their intended use.

1. __Service certificates__ are used for cloud services
2. __Management certificates__ are used for authenticating with the management API

#### Service certificates
Service certificates are attached to cloud services and enable secure communication to and from the service. For example, if you deploy a web site, you would want to supply a certificate that can authenticate an exposed HTTPS endpoint. Service certificates, which are defined in your service definition, are automatically deployed to the VM that is running an instance of your role.

You can upload service certificates to Azure either using the Azure portal or by using the classic deployment model. Service certificates are associated with a specific cloud service. They are assigned to a deployment in the service definition file.

You can manage service certificates separately from your services, and you can have different people managing them. For example, a developer could upload a service package that refers to a certificate that an IT manager has previously uploaded to Azure. An IT manager can manage and renew that certificate (changing the configuration of the service) without needing to upload a new service package. Updating without a new service package is possible because the logical name, store name, and location of the certificate is in the service definition file, while the certificate thumbprint is specified in the service configuration file. To update the certificate, it's only necessary to upload a new certificate and change the thumbprint value in the service configuration file.

#### Management certificates

Management certificates allow you to authenticate with the classic deployment model. Many programs and tools (such as Visual Studio or the Azure SDK) use these certificates to automate configuration and deployment of various Azure services. However, these types of certificates are not related to cloud services.

#### Using Azure Key Vault with certificates

You can store your certificates in Azure Key Vault - much like any other secret. However, Key Vault provides additional features above and beyond the typical certificate management.

* You can create certificates in Key Vault, or import existing certificates
* You can securely store and manage certificates without interaction with private key material.
* You can create a policy that directs Key Vault to manage the life cycle of a certificate.
* You can provide contact information for notification about life-cycle events of expiration and renewal of certificate.
* You can automatically renew certificates with selected issuers - Key Vault partner x509 certificate providers / certificate authorities.

Automating certificate management helps to reduce or eliminate the error prone task of manual certificate management.

### Protect your network
Securing your network from attacks and unauthorized access is an important part of any architecture. Here, we'll take a look at what network security looks like, how to integrate a layered approach into your architecture, and how Azure can help you provide network security for your environment.

#### A layered approach to network security

You've probably noticed that a common theme throughout this module is the emphasis of a layered approach to security. This approach is also recommended at the network layer. It's not enough to just focus on securing the network perimeter, or focusing on the network security between services inside a network. A layered approach provides multiple levels of protection, so that if an attacker gets through one layer, there are further protections in place to limit further attack.

Let's take a look at how Azure can provide the tools for a layered approach to securing your network footprint.

__Internet protection__

If we start on the perimeter of the network, we're focused on limiting and eliminating attacks from the internet. We suggest first assessing the resources that are internet-facing, and to only allow inbound and outbound communication where necessary. Make sure you identify all resources that are allowing inbound network traffic of any type, and then ensure they are restricted to only the ports and protocols required. Azure Security Center is a great place to look for this information, because it will identify internet-facing resources that don't have network security groups associated with them, as well as resources that are not secured behind a _firewall_.

#### What is a Firewall?

A firewall is a service that grants server access based on the originating IP address of each request. You create firewall rules that specify ranges of IP addresses. Only clients from these granted IP addresses will be allowed to access the server. Firewall rules, generally speaking, also include specific network protocol and port information.

To provide inbound protection at the perimeter, you have several choices.

* __Azure Firewall__ is a managed, cloud-based, network security service that protects your Azure Virtual Network resources. It is a fully stateful firewall as a service with built-in high availability and unrestricted cloud scalability. Azure Firewall provides inbound protection for non-HTTP/S protocols. Examples of non-HTTP/S protocols include: Remote Desktop Protocol (RDP), Secure Shell (SSH), and File Transfer Protocol (FTP). It also provides outbound, network-level protection for all ports and protocols, and application-level protection for outbound HTTP/S.

* __Azure Application Gateway__ is a load balancer that includes a Web Application Firewall (WAF) that provides protection from common, known vulnerabilities in websites. It is designed to protect HTTP traffic.

* __Network virtual appliances (NVAs)__ are ideal options for non-HTTP services or advanced configurations, and are similar to hardware firewall appliances.

#### Stopping Distributed Denial of Service (DDoS) attacks

Any resource exposed on the internet is at risk of being attacked by a denial of service attack. These types of attacks attempt to overwhelm a network resource by sending so many requests that the resource becomes slow or unresponsive.

When you combine __Azure DDoS Protection__ with application design best practices, you help provide defense against DDoS attacks. DDoS Protection leverages the scale and elasticity of Microsoft's global network to bring DDoS mitigation capacity to every Azure region. The Azure DDoS Protection service protects your Azure applications by monitoring traffic at the Azure network edge before it can impact your service's availability. Within a few minutes of attack detection, you are notified using Azure Monitor metrics.

This diagram shows network traffic flowing into Azure from both customers and an attacker. Azure DDoS protection identifies the attacker's attempt to overwhelm the network and blocks further traffic from reaching Azure services. Legitimate traffic from customers still flows into Azure without any interruption of service.

Azure DDoS Protection provides the following service tiers:

* Basic - The Basic service tier is automatically enabled as part of the Azure platform. Always-on traffic monitoring and real-time mitigation of common network-level attacks provide the same defenses that Microsoft's online services use. Azure's global network is used to distribute and mitigate attack traffic across regions.
* Standard - The Standard service tier provides additional mitigation capabilities that are tuned specifically to Microsoft Azure Virtual Network resources. DDoS Protection Standard is simple to enable and requires no application changes. Protection policies are tuned through dedicated traffic monitoring and machine learning algorithms. Policies are applied to public IP addresses associated with resources deployed in virtual networks, such as Azure Load Balancer and Application Gateway. DDoS standard protection can mitigate the following types of attacks:

    * Volumetric attacks. The attackers goal is to flood the network layer with a substantial amount of seemingly legitimate traffic.
    * Protocol attacks. These attacks render a target inaccessible, by exploiting a weakness in the layer 3 and layer 4 protocol stack.
    * Resource (application) layer attacks. These attacks target web application packets to disrupt the transmission of data between hosts.

#### Controlling the traffic inside your virtual network

__Virtual network security__

Once inside a virtual network (VNet), it's crucial that you limit communication between resources to only what is required.

For communication between virtual machines, _Network Security Groups _(NSGs) are a critical piece to restrict unnecessary communication.

Network Security Groups allow you to filter network traffic to and from Azure resources in an Azure virtual network. An NSG can contain multiple inbound and outbound security rules that enable you to filter traffic to and from resources by source and destination IP address, port, and protocol. They provide a list of allowed and denied communication to and from network interfaces and subnets, and are fully customizable.

You can completely remove public internet access to your services by restricting access to service endpoints. With service endpoints, Azure service access can be limited to your virtual network.

__Network integration__
It's common to have existing network infrastructure that needs to be integrated to provide communication from on-premises networks or to provide improved communication between services in Azure. There are a few key ways to handle this integration and improve the security of your network.

Virtual private network (VPN) connections are a common way of establishing secure communication channels between networks. Connections between Azure Virtual Network and an on-premises VPN device are a great way to provide secure communication between your network and your VNet on Azure.

To provide a dedicated, private connection between your network and Azure, you can use Azure ExpressRoute. ExpressRoute lets you extend your on-premises networks into the Microsoft cloud over a private connection facilitated by a connectivity provider. With ExpressRoute, you can establish connections to Microsoft cloud services, such as Microsoft Azure, Microsoft 365, and Dynamics 365. ExpressRoute connections improve the security of your on-premises communication by sending this traffic over the private circuit instead of over the public internet. You don't need to allow access to these services for your end users over the public internet, and you can send this traffic through appliances for further traffic inspection.

#### Summary
A layered approach to network security helps reduce your risk of exposure through network-based attacks. Azure provides several services and capabilities to secure your internet-facing resource, internal resources, and communication between on-premises networks. These features make it possible to create secure solutions on Azure.

You can also combine multiple Azure networking and security services to manage your network security and provide increased layered protection. For example, you can use Azure Firewall to protect inbound and outbound traffic to the Internet, and Network Security Groups to limit traffic to resources inside your virtual networks.

### Protect your shared documents

__Microsoft Azure Information Protection__ (sometimes referred to as AIP) is a cloud-based solution that helps organizations classify and optionally protect documents and emails by applying labels.

Labels can be applied automatically based on rules and conditions. Labels can also be applied manually. You can also guide users to choose recommended labels with a combination of automatic and manual steps.

The following screen capture is an example of AIP in action on a user's computer. In this example, the administrator has configured a label with rules that detect sensitive data. When a user saves a Microsoft Word document containing a credit card number, a custom tooltip is displayed. The tooltip recommends labeling the file as Confidential \ All Employees. This label is configured by the administrator. Using this label classifies the document and protects it.

After your content is classified, you can track and control how the content is used. For example, you can:

* Analyze data flows to gain insight into your business
* Detect risky behaviors and take corrective measures
* Track access to documents
* Prevent data leakage or misuse of confidential information

### Azure Advanced Threat Protection

__Azure Advanced Threat Protection__ (Azure ATP) is a cloud-based security solution that identifies, detects, and helps you investigate advanced threats, compromised identities, and malicious insider actions directed at your organization.

Azure ATP is capable of detecting known malicious attacks and techniques, security issues, and risks against your network.

#### Azure ATP components
Azure ATP consists of several components.

#### Azure ATP portal

Azure ATP has its own portal, through which you can monitor and respond to suspicious activity. The Azure ATP portal allows you to create your Azure ATP instance, and view the data received from Azure ATP sensors. You can also use the portal to monitor, manage, and investigate threats in your network environment. You can sign in to the Azure ATP portal at https://portal.atp.azure.com . Your user accounts must be assigned to an Azure AD security group that has access to the Azure ATP portal to be able to sign in.

#### Azure ATP sensor
Azure ATP sensors are installed directly on your domain controllers. The sensor monitors domain controller traffic without requiring a dedicated server or configuring port mirroring.

#### Azure ATP cloud service
Azure ATP cloud service runs on Azure infrastructure and is currently deployed in the United States, Europe, and Asia. Azure ATP cloud service is connected to Microsoft's intelligent security graph.

#### Purchasing Azure Advanced Threat Protection
Azure ATP is available as part of the Enterprise Mobility + Security E5 suite (EMS E5) and as a standalone license. You can acquire a license directly from the Enterprise Mobility + Security Pricing Options page or through the Cloud Solution Provider (CSP) licensing model. It is not available to purchase via the Azure portal.

### Understand Security Considerations for Application Lifecycle Management Solutions
The Microsoft Security Development Lifecycle (SDL) introduces security and privacy considerations throughout all phases of the development process. It helps developers build highly secure software, address security compliance requirements, and reduce development costs. The guidance, best practices, tools, and processes in the SDL are practices used internally at Microsoft to build more secure products and services.

Since first sharing the SDL in 2008, the practices have been continuously updated to cover new scenarios such as cloud services, IoT, and AI.

#### Provide training

Security is everyone's job. Developers, service engineers, and program and project managers must understand security basics. They all must know how to build security into software and services to make products more secure, while still addressing business needs and delivering user value. Effective training will complement and reinforce security policies, SDL practices, standards, and requirements of software security, and be guided by insights derived through data or newly available technical capabilities.

Although security is everyone's job, it's important to remember that not everyone needs to be a security expert nor strive to become a proficient penetration tester. However, ensuring everyone understands the attacker's perspective, their goals, and the art of the possible will help capture the attention of everyone and raise the collective knowledge bar.

#### Define security requirements

Security and privacy is a fundamental aspect of developing highly secure applications and systems. Regardless of development methodology in use, security requirements must be updated continuously in order to address changes in required functionality and changes to the threat landscape. The optimal time to define the security requirements is during the initial design and planning stages. Early planning allows development teams to integrate security in ways that minimize disruption.

Factors that influence security requirements include, but are not limited to:

* Legal and industry requirements
* Internal standards and coding practices
* Review of previous incidents
* Known threats

These requirements should be tracked through a work-tracking system, or through telemetry that is derived from the engineering pipeline.

#### Define metrics and compliance reporting
It's essential for an organization to define the minimum acceptable levels of security quality, and to hold engineering teams accountable to meeting that criteria. Defining these expectations early helps a team understand the risks that are associated with security issues, identify and fix security defects during development, and apply the standards throughout the entire project. Setting a meaningful security bar involves clearly defining the severity thresholds of security vulnerabilities, and helps to establish a plan of action when vulnerabilities are encountered. For example, all known vulnerabilities discovered with a "critical" or "important" severity rating must be fixed with a specified time frame.

To track key performance indicators (KPIs) and ensure security tasks are completed, bug tracking and/or work tracking mechanisms used by an organization (such as Azure DevOps) should allow for security defects and security work items to be clearly labeled as security, and marked with their appropriate security severity. This tracking allows for accurate tracking and reporting of security work.

You can read more about defining metrics and compliance reporting at:

* SDL Privacy Bug Bar Sample
* Add or modify an Azure DevOps field to track work
* SDL Security Bug Bar Sample

#### Perform threat modeling
Threat modeling should be used in environments where there is a meaningful security risk. As a practice, it allows development teams to consider, document, and discuss the security implications of designs in the context of their planned operational environment, and in a structured fashion. Applying a structured approach to threat scenarios helps a team more effectively and less expensively identify security vulnerabilities, determine risks from those threats, and then make security feature selections and establish appropriate mitigations. You can apply threat modeling at the component, application, or system level.

More information is available at Threat Modeling.

#### Establish design requirements

The SDL is typically thought of as assurance activities that help engineers implement more secure features, meaning the features are well engineered for security. To achieve this assurance, engineers typically rely on security features such as cryptography, authentication, and logging. In many cases, selecting or implementing security features has proven to be so complicated that design or implementation choices are likely to result in vulnerabilities. Therefore, it's crucial that they are applied consistently and with a consistent understanding of the protection they provide.

#### Define and use cryptography standards

With the rise of mobile and cloud computing, it's important to ensure all data - including security-sensitive information and management and control data - are protected from unintended disclosure or alteration when it's being transmitted or stored. Encryption is typically used to achieve this protection. However, making an incorrect choice when using any aspect of cryptography can be catastrophic. Therefore, it's best to develop clear encryption standards that provide specifics on every element of the encryption implementation.

Encryption should be left to experts. A good general rule is to only use industry-vetted encryption libraries and ensure they're implemented in a way that allows them to be easily replaced if needed.

For more information on encryption, see the Microsoft SDL Cryptographic Recommendations whitepaper.

#### Manage security risks from using third-party components
The vast majority of software projects today are built using third-party components (both commercial and open source). When selecting which third-party components to use, it's important to understand the impact that a security vulnerability in them could have to the security of the larger system into which they are integrated. Having an accurate inventory of these components, and a plan to respond when new vulnerabilities are discovered, will go a long way toward mitigating risks. However, you should also consider additional validation, depending on your organization's risk tolerance, the type of component being used, and potential impact of a security vulnerability.

Learn more about managing the security risks of using third-party components at:

Managing Security Risks Inherent in the Use of Third-Party Components
Managing Security Risks Inherent in the Use of Open-Source Software

#### Use approved tools

Define and publish a list of approved tools and their associated security checks, such as compiler/linker options and warnings. Engineers should strive to use the latest version of approved tools (such as compiler versions), and to utilize new security analysis functionality and protections.

For more information, see:

Recommended Tools, Compilers and Options for x86, x64, and ARM processors (whitepaper)
SDL Resources

#### Perform Static Analysis Security Testing
Analyzing source code prior to compilation provides a highly scalable method of security code review, and helps ensure that secure coding policies are being followed. Static Analysis Security Testing (SAST) is typically integrated into the commit pipeline to identify vulnerabilities each time the software is built or packaged. However, some offerings integrate into the developer environment to spot certain flaws such as the existence of unsafe or other banned functions, and then replace those functions with safer alternatives while the developer is actively coding. There is no one-size-fits-all solution; development teams should decide the optimal frequency for performing SAST, and consider deploying multiple tactics to balance productivity with adequate security coverage.

More information is available at:

* Microsoft DevSkim on GitHub
* Roslyn Security Guard Rules
* Visual Studio Marketplace
* Analyzing C/C++ Code Quality by Using Code Analysis
* Microsoft BinSkim on GitHub

Perform Dynamic Analysis Security Testing
Performing run-time verification of your fully compiled or packaged software checks functionality that is only apparent when all components are integrated and running. This verification is typically achieved using a tool, a suite of pre-built attacks, or tools that specifically monitor application behavior for memory corruption, user privilege issues, and other critical security problems. Similar to SAST, there is no one-size-fits-all solution and while some tools (such as web app scanning tools) can be more readily integrated into the CI/CD pipeline, other Dynamic Application Security Testing (DAST) such as fuzzing requires a different approach.

More information is available at:

Visual Studio Marketplace
Automated Penetration Testing with White-Box Fuzzing

#### Perform penetration testing

Penetration testing is a security analysis of a software system that is performed by skilled security professionals who simulate the actions of a hacker. The objective of a penetration test is to uncover potential vulnerabilities resulting from coding errors, system configuration faults, or other operational deployment weaknesses. Penetration tests typically find the broadest variety of vulnerabilities, and are often performed in conjunction with automated and manual code reviews to provide a greater level of analysis than would ordinarily be possible.

More information is available at:

Attack Surface Analyzer
SDL Security Bug Bar Sample

#### Establish a standard incident response process
Preparing an incident response plan is crucial for addressing new threats that can emerge over time, and your plan should be created in coordination with your organization's dedicated Product Security Incident Response Team (PSIRT). Your incident response plan should:

* Include who to contact if a security emergency occurs
* Establish the protocol for security servicing (including plans for code inherited from other groups within the organization and for third-party code)
* Be tested before it is needed

For more information about incident responses, see:

* Using Azure Security Center for an incident response
* Microsoft Incident Response and shared responsibility for cloud computing
* Microsoft Security Response Center
By introducing standardized security and compliance considerations throughout all phases of the development process, developers can reduce the likelihood of vulnerabilities in products and services, and avoid repeating the same security mistakes. Similarly, security integration throughout the operations lifecycle will assist in maintaining the integrity of those products and services. Operational Security Assurance practices should align with your development processes; this arrangement will result in less time and cost spent on triage and response after the fact, and provide your customers with assurance that your products are highly secure.

#### Summary

In this module, we discussed the basic concepts for protecting your infrastructure and data when you work in the cloud.

Defense in depth is the overriding theme - think about security as a multi-layer, multi-vector concern. Threats come from places we don't expect, and they can come with strength that will surprise us.

Azure has out-of-the-box help for a great deal of the security issues we face. One of the first steps we should take is assessing how much help from Azure we can use based on whether we're leveraging IaaS, PaaS, or SaaS.

Azure Security Center centralizes much of the help Azure has to offer. It provides a single dashboard, with a view into many of your services, and helps make sure you are following best practices. Continuously updated machine learning algorithms help identify whether the latest threats are aimed at your resources. And it helps your organization mitigate threats.

This module is only introductory. Security is a deep and complex topic, so whatever your cloud approach, an ongoing security education is necessary. But this module should get you started in the right direction, so you know what you need to learn next.