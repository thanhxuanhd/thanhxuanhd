# Introduction

Modern businesses run on multiple applications and services. How well your business runs can often be impacted by how efficiently(hiệu quả) you can distribute(phân tán) the right data to the right task. Automating this flow of data can streamline your business even further. Choosing the right technology for these critical data integrations(sự tích hợp) and process automation is also an important consideration.

Suppose you're a developer at a company that operates a bike rental system at a large state university. Your company recently acquired the rights to operate at another university in the next state.

The other university also has a rental system in place. You'll need to integrate with those existing systems too. The systems include bike tracking, inventory management, rental scheme registration, and more.

It's June and the university would like to have the rental system in place before students arrive on campus in late August. Your business runs on Azure and a requirement of this integration is that you don't take on any infrastructure maintenance or heavy upfront costs. As you expand into other universities across the country, you need to be able to scale efficiently in order to keep costs down and your low margins viable.

In this module, we’ll describe the Azure technologies that are available to you and teach you how to evaluate each one for your business needs.

## Learning objectives
Upon completion of this module, you'll be able to:

* Evaluate Azure services for integration and process automation scenarios

## Prerequisites
* Familiarity with Azure
* Basic knowledge of REST and APIs

# Identify the technology options

You don't have much time to get business processes properly integrated between your existing bike rental system and the system in use in the second campus. You'd like to make the most of your existing Azure expertise(kinh nghiệm) and you've read that Azure includes several different technologies that you can use to solve problems like this.

In this unit, we'll explore the Azure technology options that are available to automate and integrate your business processes.

## Common Business Problems
In business, one way to guarantee a high-quality service to users and high-quality products is to design and implement strict business processes. Such processes may involve multiple steps, multiple people, and multiple software packages. Each process may run in a simple line of activities that workers perform one after the other or they may branch or loop. Each process may also run quickly or take days or weeks to complete.

Frequently, a business runs into issues when it merges with a second business or integrates with a partner organization. How can administrators integrate the separate processes used in the two organizations, which may have been implemented using different software?

Business processes modeled in software are often called __workflows__. Azure includes four different technologies that you can use to build and implement workflows that integrate multiple systems:

* Logic Apps
* Microsoft Power Automate
* WebJobs
* Azure Functions

These four technologies have some similarities. For example:

* They can all accept __inputs__. An input is a piece of data or a file that is supplied to the workflow.
* They can all run __actions__. An action is a simple operation that the workflow executes and may often modify data or cause another action to be performed.
* They can all include __conditions__. A condition is a test, often run against an input, that may decide which action to execute next.
* They can all produce __outputs__. An output is a piece of data or a file that is created by the workflow.* 

In addition, workflows created with these technologies can either start based on a schedule or they can be triggered by some external event.

## Design-first technologies

When business analysts discuss and plan a business process, they may draw a flow diagram on paper. With Logic Apps and Microsoft Power Automate, you can take a similar approach to designing a workflow. They both include user interfaces in which you can draw out the workflow. We call this approach a _design-first approach_.

### Logic Apps

Logic Apps is a service within Azure that you can use to automate, orchestrate(dàn dựng), and integrate disparate(khác nhaue) components of a distributed application. By using the design-first approach in Logic Apps, you can draw out complex workflows that model complex business processes. The following screenshot shows the Logic Apps Designer and design canvas that you use to define your workflow.

Alternatively(ngoài ra), if you prefer to work in code, you can create or edit a workflow in JSON notation by using the code view, as illustrated in the following screenshot.

One reason why Logic Apps is so good at integration is that over 200 connectors are included. A _connector_ is a Logic Apps component that provides an interface to an external service. For example, the Twitter connector allows you to send and retrieve tweets, while the Office 365 Outlook connector lets you manage your email, calendar, and contacts. Logic Apps provides hundreds of pre-built connectors that you can use to create your apps. If you have an unusual or unique system that you want to call from a Logic Apps, you can create your own connector if your system exposes a REST API.

### Microsoft Power Automate

Microsoft Power Automate is a service that you can use to create workflows even when you have no development or IT Pro experience. You can create workflows that integrate and orchestrate many different components by using the website or the Microsoft Power Automate mobile app.

There are four different types of flow that you can create:

* __Automated__: A flow that is started by a trigger from some event. For example, the event could be the arrival of a new tweet or a new file being uploaded.
* __Button__: Use a button flow to run a repetitive task with a single click from your mobile device.
* __Scheduled__: A flow that executes on a regular basis such as once a week, on a specific date, or after 10 hours.
* __Business process__: A flow that models a business process such as the stock ordering process or the complaints procedure.

Microsoft Power Automate provides an easy-to-use design surface that anyone can use to create flows of the above types. As the following screenshot illustrates, the designer makes it easy to design and layout your process.

Under the hood, Microsoft Power Automate is built on Logic Apps. This fact means that Power Automate supports the same range of connectors and actions. You can also use custom connectors in Microsoft Power Automate.

Design-first technologies compared

As you can see from the following table, Microsoft Power Automate is more appropriate for use by non-technical staff. If your workflow designers are IT professionals, developers, or DevOps practitioners, Logic Apps are usually a better fit:

|                   | Microsoft Power Automate | Logic Apps |
| ----------------  | ------------------------ | ---------- |
| Intended users    |	Office workers and business analysts | Developers and IT pros |
| Intended scenarios |	Self-service workflow creation | Advanced integration projects|
| Design tools	    | GUI only. Browser and mobile app	| Browser and Visual Studio designer. Code editing is possible |
| Application Lifecycle Management | Power Automate includes testing and production environments |	Logic Apps source code can be included in Azure DevOps and source code management systems |

## Code-first technologies

The developers on your team will likely prefer to write code when they want to orchestrate and integrate different business applications into a single workflow. This is the case when you need more control over the performance of your workflow or need to write custom code as part of the business process. For such people, Azure includes WebJobs and Functions.

### WebJobs and the WebJobs SDK

The Azure App Service is a cloud-based hosting service for web applications, mobile back-ends, and RESTful APIs. These applications often need to perform some kind of background task. For example, in your bike rental system, when a user uploads a photo of a bike, you may need to generate a smaller thumbnail photograph.

WebJobs are a part of the Azure App Service that you can use to run a program or script automatically. There are two kinds of WebJob:

* __Continuous__. These WebJobs run in a continuous loop. For example, you could use a continuous WebJob to check a shared folder for a new photo.
* __Triggered__. These WebJobs run when you manually start them or on a schedule.

To determine what actions your WebJobs takes, you can write code in several different languages. For example, you can script the WebJob by writing code in a Shell Script (Windows, PowerShell, Bash). Alternatively, you can write a program in PHP, Python, Node.js, or Java.

You can also program a WebJob by using the .NET Framework or the .NET Core Framework and a .NET language such as C# or VB.NET. In this case, you can also use the WebJobs SDK to make the task easier. The SDK includes a range of classes, such as `JobHostConfiguration` and `HostBuilder`, which reduce the amount of code required to interact with the Azure App Service.

The WebJobs SDK only supports C# and the NuGet package manager.

### Azure Functions

An Azure Function is a simple way for you to run small pieces of code in the cloud, without having to worry about the infrastructure required to host that code. You can write the Function in C#, Java, JavaScript, PowerShell, Python, or any of the languages that are listed in the Supported languages in Azure Functions article. In addition, with the consumption plan option, you only pay for the time when the code runs. Azure automatically scales your function in response to the demand from users.

When you create an Azure Function, you can start by writing the code for it in the portal. Alternatively, if you need source code management, you can use GitHub or Azure DevOps Services.

To create an Azure Function, choose from the range of templates. The following list is an example of some of the templates available to you.

* __HTTPTrigger__. Use this template when you want the code to execute in response to a request sent through the HTTP protocol.
* __TimerTrigger__. Use this template when you want the code to execute according to a schedule.
* __BlobTrigger__. Use this template when you want the code to execute when a new blob is added to an Azure Storage account.
* __CosmosDBTrigger__. Use this template when you want the code to execute in response to new or updated documents in a NoSQL database.

Azure Functions can integrate with many different services both within Azure and from third parties. These services can trigger your function, or send data input to your function, or receive data output from your function

## Code-first technologies compared

In most cases, the simple administration and more flexible coding model provided by Azure Functions may lead you to choose them in preference to WebJobs. However, you may choose WebJobs for the following reasons:

You want the code to be a part of an existing App Service application and to be managed as part of that application, for example in the same Azure DevOps environment.
You need close control over the object that listens for events that trigger the code. This object in question is the `JobHost` class, and you have more flexibility to modify its behavior in WebJobs.

|                  | Azure WebJobs | Azure Functions |
| ---------------  | ------------- | --------------- |
| Supported languages |	C# if you are using the WebJobs SDK | C#, Java, JavaScript, PowerShell, etc. |
| Automatic scaling | No | Yes|
| Development and testing in a browser | No | Yes |
| Pay-per-use pricing | No | Yes |
| Integration with Logic Apps |	No | Yes |
| Package managers	| NuGet if you are using the WebJobs SDK | Nuget and NPM |
| Can be part of an App Service application | Yes |	No |
| Provides close control of JobHost | Yes |	No |
Now that you know what design-first and code-first technologies are available to you, how do you narrow your choices? We'll look at this question in the next unit.


# Analyze the decision criteria

There are several different business processes that run your bicycle rental business. For example, there is the bike rental process, a return process, a bike booking process, and processes that don't directly relate to bikes, such as holiday booking for the staff.

We've introduced an array of Azure technology that could be used to help build these processes. Let's try to be more concrete about how we make the decision for a given process.

## How to choose a service

The following diagram shows a simplified flow chart that you can use to choose the best technology to use for your business process:

The first question to ask is whether you prefer to design the workflow in a GUI designer tool or by writing code. The following list has some valid reasons for using a design-first tool:

* People who design the workflow have no coding experience.
* Later designers and users can consult the graphical design to clearly understand how the workflow proceeds.

Alternatively, you can choose to use a code-first tool because:

* People who design the workflow are developers and prefer to work entirely in code.
* You want the details of a workflow to be hidden from non-coders.

## Choosing a design-first technology

If you choose to use a design-first approach, you must also choose from Microsoft Power Automate and Azure Logic Apps.

The principal question here is who will design the workflow: will it be developers or users?

In Logic Apps, there is a GUI designer on which you draw out the workflow. It is intuitive and easy to use but you also have the opportunity to delve under the hood and edit the source code for a workflow. This tool is designed for people with development skills.

In Microsoft Power Automate, extra help and templates are provided for common types of workflow. There is no way to edit the source code that the tool creates. This tool is designed for users who have a good understanding of the business process but no coding skills.

## Choosing a code-first technology

If you choose to use a code-first approach, your next choice is between WebJobs and Azure Functions.

Because of the extra features that are included with Azure Functions, including wider ranges of trigger events and supported languages, the ability to develop test code in the browser, and the pay-per-use price model, consider Azure Functions to be your default choice. There are two situations in which WebJobs might be a better choice:

* You have an existing Azure App Service application, and you want to model the workflow within the application. This requirement means that the workflow can also be managed as part of the application, for example in an Azure DevOps environment.

* You have specific customizations that you want to make to the JobHost that are not supported by Azure Functions. For example, in a WebJob, you can create a custom retry policy for calls to external systems. This kind of policy can't be configured in an Azure Function.

## Mixing Technologies

Remember that there is no requirement for you to use the same technology for different workflows: if your requirements differ, you are likely to reach a different answer at the end of your decision-making process. Furthermore, you can also call one workflow from another. For example, a workflow implemented in Microsoft Power Automate can easily call another that is built as an Azure Function.

One reason to mix the technologies used in your business processes would be to give users control over a small section of a complete workflow. Do this by implementing that section in Microsoft Power Automate, then call that flow from a Logic App, Web Job, or Function.

# Choose the best design-first technology to automate your business process

You want to choose a technology to automate the booking process for your bike rental business.

You want to streamline and modernize this process as it is performed on your original campus. You also want to integrate a bike tracking technology that is used on the new campus where you've recently obtained the rights to operate the existing bike rental business.

In this exercise, we'll examine this scenario in detail and choose which technology to use.

## Scenario

On your original campus, you have five bike rental shops. Each shop has a list of bikes for rent and its own database that records the bikes and their features and whether they are already rented or in the shop.

Currently, each bike can only be rented from its home shop. When a customer returns a bike to another shop, your staff move it back to the shop where it is listed on the database. You'd like to change the process so that each bike can be rented from any shop. However, you want to ensure staff can find out quickly where each bike is.

At the university in the next state, the bike rental business invested in a third party system to track bike locations. When a bike arrives back at a store, a unique barcode on the crossbar is scanned. The bike tracking database is automatically updated with the name of the shop that scanned the barcode. When a bike leaves a shop with a customer, the location is changed to "On Hire" and the customer name is recorded in a separate column.

This system has proved helpful when a customer asks for a bike with specific frame size and/or specific features, such as an electric motor or all-terrain suspension. If a shop doesn't have a bike with the right equipment, the shop can quickly find out where such a bike is and get it or send the customer to the right shop. This bike location database has a REST API that you can call from other systems.

Your managing director wants to be able to understand clearly the workflow that you develop. There were problems in the past when documentation has not been kept in-sync with custom code and your director wants to see the process as it's implemented.

## Business Process
You want to update the bike reservation and rental process on both campuses to the following workflow:

The details are as follows:

1. A customer requests a bike on the phone, in person, or through the website.
2. Shop staff record the customer's details and frame size.
3. Does the customer need specific features such as an electric motor, suspension, or a child trailer? If so what are those features?
4. Where are all the bikes with that frame size and those features? This information is obtained from the bike location database and is kept up-to-date by the barcode scanning system.
5. Is there a bike with the right features and frame size in the right shop? If yes book that bike.

    a. If not, where is the nearest bike? Reserve that bike.

    b. Send an email to staff to move the bike to the customer.

    c. Scan barcode in new location.
6. Give the bike to the customer and update location to "On Hire".
7. Take payment from the customer.

This is a simplification of the entire process. For simplicity, we've omitted edge cases such as no bike with the desired frame size or features is available for rent. Perhaps you can think of other cases not covered by this simplified process.

## Choosing a technology
Let's look at the Azure technologies available to implement the business process and integrate with the bike location database:

* Microsoft Power Automate
* Azure Logic Apps
* Azure Functions
* Azure Service Apps WebJobs

You could use any of these technologies and others to build a workflow for this business process. Each technology can also integrate with any REST API, so you could also use any of these technologies to integrate with the bike location system. How then, can you choose?

## Design-first or code-first?

We know that your Managing Director and her staff want to understand the workflow at a higher level than examining the code and implementation. She also doesn't like separate documents describing a process, because it so easily becomes out-of-date when the process changes.

If you choose a design-first approach, the workflow is visualized in an easy-to-understand design surface. In addition, that diagram is not a separate document, but a picture of the process as it is implemented. The benefit is that there's no possibility that the diagram is not updated when the process is changed.

For this reason, choose a design-first approach.

## Microsoft Power Automate or Azure Logic Apps?

Now you must choose from the two design-first technologies:

* Microsoft Power Automate
* Azure Logic Apps

There's no suggestion in the scenario that shop staff should be able to modify the business process. In addition, to connect to the bike location database through its REST API, you will need to create a custom connector. This is a developer task.

It seems sensible that the development of the custom connector and the workflow should be done by the same person or team. Since these must be developers, it's best to use Azure Logic Apps.

As this exercise shows, we can narrow down the technology to use for a given solution by simply understanding the business process and the audience.

# When to choose Azure Functions to run your business logic