# Introduction
Imagine you work for an escalator company that has invested in IoT technology to monitor its product in the field. You oversee the processing of temperature sensor data from the drive gears of the escalators. You monitor the temperature data and add a data flag to indicate when the gears are too hot. In downstream systems, this data helps determine when maintenance is required.

Your company receives sensor data from several locations and from different escalator models. The data arrives in different formats, including batch file uploads, scheduled database pulls, messages on a queue, and incoming data from an event hub. You want to develop a reusable service that can process your temperature data from all these sources.

When designing a service such as this with traditional enterprise architecture strategies, you would need to consider server infrastructure and maintenance up front: scope out necessary hardware, plan to install it, coordinate with IT to manage it, etc. An alternative to all that work is __serverless computing__. With serverless computing, your cloud provider manages the provisioning and maintenance of the infrastructure letting you focus completely on building the app logic. Azure Functions is a key component of the serverless computing offering from Azure and enables you to run pieces of code or _functions_, written in the programming language of your choice, in the cloud.

## Learning objectives

In this module, you will:

* Decide if serverless computing is right for your business need.
* Create an Azure function app in the Azure portal.
* Execute a function using triggers.
* Monitor and test your Azure function from the Azure portal.

# Decide if serverless computing is right for your business needs

To help decide whether serverless computing is right for you, let's first learn what serverless is all about.

## What is serverless compute?

Serverless compute can be thought of as a function as a service (FaaS), or a microservice that is hosted on a cloud platform. Your business logic runs as functions and you don't have to manually provision or scale infrastructure. The cloud provider manages infrastructure. Your app is automatically scaled out or down depending on load. Azure has several ways to build this sort of architecture. The two most common approaches are Azure Logic Apps and Azure Functions, which we focus on in this module.

## What is Azure Functions?

_Azure Functions is a serverless application platform_. It allows developers to host business logic that can be executed without provisioning infrastructure. Functions provides intrinsic scalability and you are charged only for the resources used. You can write your function code in the language of your choice, including C#, F#, JavaScript, Python, and PowerShell Core. Support for package managers like NuGet and NPM is also included, so you can use popular libraries in your business logic.

## Benefits of a serverless compute solution

Serverless compute is a great option for hosting business logic code in the cloud. With serverless offerings such as Azure Functions, you can write your business logic in the language of your choice. You get automatic scaling, you have no servers to manage, and you are charged based on what is used — not on reserved time. Here are some additional characteristics of a serverless solution for you to consider.

## Avoids over-allocation of infrastructure

Suppose you've provisioned VM servers and configured them with enough resources to handle your peak load times. When the load is light, you are potentially paying for infrastructure you're not using. Serverless computing helps solve the allocation problem by scaling up or down automatically, and you're only billed when your function is processing work.

## Stateless logic

Stateless functions are great candidates for serverless compute; function instances are created and destroyed on demand. If state is required, it can be stored in an associated storage service.

## Event driven

Functions are _event driven_. This means they run only in response to an event (called a "trigger"), such as receiving an HTTP request, or a message being added to a queue. You configure a trigger as part of the function definition. This approach simplifies your code by allowing you to declare where the data comes from (trigger/input binding) and where it goes (output binding). You don't need to write code to watch queues, blobs, hubs, etc. You can focus purely on the business logic.

## Functions can be used in traditional compute environments

Functions are a key component of serverless computing, but they are also a general compute platform for executing any type of code. Should the needs of your app change, you can take your project and deploy it in a non-serverless environment, which gives you the flexibility to manage scaling, run on virtual networks, and even completely isolate your functions.

## Drawbacks of a serverless compute solution

Serverless compute will not always be the appropriate solution to hosting your business logic. Here are a few characteristics of functions that may affect your decision to host your services in serverless compute.

## Execution time
By default, functions have a timeout of 5 minutes. This timeout is configurable to a maximum of 10 minutes. If your function requires more than 10 minutes to execute, you can host it on a VM. Additionally, if your service is initiated through an HTTP request and you expect that value as an HTTP response, the timeout is further restricted to 2.5 minutes. Finally, there's also an option called __Durable Functions__ that allows you to orchestrate the executions of multiple functions without any timeout.

## Execution frequency

The second characteristic is execution frequency. If you expect your function to be executed continuously by multiple clients, it would be prudent to estimate the usage and calculate the cost of using functions accordingly. It might be cheaper to host your service on a VM.

While scaling, only one function app instance can be created every 10 seconds, for up to 200 total instances. Keep in mind, each instance can service multiple concurrent executions, so there is no set limit on how much traffic a single instance can handle. Different types of triggers have different scaling requirements, so research your choice of trigger and investigate its limits.

#  Exercise - Create a function app in the Azure portal

You are now ready to start implementing the temperature service. In the previous unit, you determined that a serverless solution would best fit your needs. Let's start by creating a function app to hold our Azure Function.

## What is a function app?

Functions are hosted in an execution context called __a function app__. You define function apps to logically group and structure your functions and a compute resource in Azure. In our elevator example, you would create a function app to host the escalator drive gear temperature service. There are a few decisions that need to be made to create the function app; you need to choose a service plan and select a compatible storage account.

## Choosing a service plan

Function apps may use one of two types of service plans. The first service plan is the __Consumption service plan__. This is the plan that you choose when using the Azure serverless application platform. The Consumption service plan provides automatic scaling and bills you when your functions are running. The Consumption plan comes with a configurable timeout period for the execution of a function. By default, it is 5 minutes, but may be configured to have a timeout as long as 10 minutes.

The second plan is called the Azure App Service plan. This plan allows you to avoid timeout periods by having your function run continuously on a VM that you define. When using an App Service plan, you are responsible for managing the app resources the function runs on, so this is technically not a serverless plan. However, it may be a better choice if your functions are used continuously or if your functions require more processing power or execution time than the Consumption plan can provide.

## Storage account requirements

When you create a function app, it must be linked to a storage account. You can select an existing account or create a new one. The function app uses this storage account for internal operations such as logging function executions and managing execution triggers. On the Consumption service plan, this is also where the function code and configuration file are stored.

## Create a function app

Let's create a function app in the Azure portal.

1. Sign into the Azure portal  using the same account you activated the sandbox with.

2. From the portal menu, select Create a resource.

> Important <br>
> We are currently working to update our sandbox to support the new workflow for creating an Azure Function in the portal. The instructions will be updated when that is available. Until then, you can use the version of the create experience that matches the current instructions by clicking the notification bar labeled Looking for the classic Function App create experience? at the top of the Function App page. This experience can also be reached by choosing the Function App (Classic) option from the Azure Marketplace.

3. Select Compute > Function App.

4. Choose a globally unique app name. This will serve as the base URL of your service. For example, you can name it escalator-functions-xxxxxxx, where the x's can be replaced with your initials and your birth year. If this isn't globally unique, you can try any other combination. Valid characters are a-z, 0-9 and -.

5. Select the Azure sandbox subscription __Concierge Subscription__.

6. Select the existing resource group called "learn-6cbe788c-f7ba-403d-8178-5c5a8f574ac1".

7. Select Windows for OS.

8. For Hosting Plan, select Consumption Plan, which is the serverless hosting option.

9. Select a geographical location close to you. In a production system, you would want to select a location near your customers or consumers of the function.

10. For Runtime Stack, select Node.js from the dropdown, which is the language in which we implement the function examples in this exercise.

11. Create a new storage account. Azure will give it a name based on the app name. You can change it if you like, but it must also be unique.

12. Make sure that Azure Application Insights is On and select the region closest to you (or your customers).

13. Select Create; deployment will take a few minutes. You'll receive a notification once it's complete.

## Verify your Azure function app

1. From the Azure portal left-hand menu, select Resource groups. You should then see a resource group named learn-6cbe788c-f7ba-403d-8178-5c5a8f574ac1 in the list of available groups.

2. Select the resource group learn-6cbe788c-f7ba-403d-8178-5c5a8f574ac1. You should then see a resource list like the following list.

The item with the lightning bolt Function icon, listed as an App Service, is your new function app. You can click on it to open the details about the new function - it has a public URL assigned to it, if you open that in a browser, you should get a default web page that indicates your Function App is running.

# Run your code on-demand with Azure Functions

Now that we have a function app created, let's look at how to build, configure, and execute a function.

Functions are event driven, which means they run in response to an event.

The type of event that starts the function is called a trigger. You must configure a function with exactly one trigger.

Azure supports triggers for the following services.

| Service | Trigger description |
| ------- | ------------------- |
| Blob storage | Start a function when a new or updated blob is detected. |
| Azure Cosmos DB |	Start a function when inserts and updates are detected. |
| Event Grid | Start a function when an event is received from Event Grid. |
| HTTP | Start a function with an HTTP request. |
| Microsoft Graph Events | Start a function in response to an incoming webhook from the Microsoft Graph. Each instance of this trigger can react to one Microsoft Graph resource type. |
| Queue storage	| Start a function when a new item is received on a queue. The queue message is provided as input to the function. |
| Service Bus |	Start a function in response to messages from a Service Bus queue. |
| Timer	| Start a function on a schedule.|

## Bindings

Bindings are a declarative way to connect data and services to your function. Bindings know how to talk to different services, which means you don't have to write code in your function to connect to data sources and manage connections. The platform takes care of that complexity for you as part of the binding code. Each binding has a direction - your code reads data from _input_ bindings and writes data to _output_ bindings. Each function can have zero or more bindings to manage the input and output data processed by the function.

A trigger is a special type of input binding that has the additional capability of initiating execution.

Azure provides a large number of bindings to connect to different storage and messaging services.

## A sample binding definition

Let's look at an example of configuring a function with an input binding (trigger) and an output binding. Let's say we want to write a new row to Azure Table storage whenever a new message appears in Azure Queue storage. This scenario can be implemented using an Azure Queue storage _trigger_ and an Azure Table storage _output binding_.

The following snippet is the function.json file for this scenario.

```JSON
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Our JSON configuration specifies that our function will be triggered when a message is added to a queue named myqueue-items. The return value of our function is then written to the outTable table in Azure Table storage. For PowerShell functions, output bindings are explicitly written to with the `Push-OutputBinding` cmdlet.

This example is a simple illustration of how we configure bindings for a function. We could change the output to be an email using a SendGrid binding, or put an event onto a Service Bus to notify some other component in our architecture, or even have multiple output bindings to push data to various services.

## Creating a function in the Azure portal

Azure provides several pre-made function templates for common scenarios.

## Quickstart templates

When adding your first function, you are presented with the Quickstart screen where you can choose the trigger for your function. Based on your selections, Azure will generate the function code and configuration for you with some sample code provided to display out the input data received in the log.

## Custom function templates

The selection of Quickstart templates provides easy access to the most common scenarios. However, Azure provides over 30 additional templates you can start with. These can be selected from the template list screen when creating subsequent functions or be selected by using the Custom function option on the Quickstart screen.

## Navigating to your function and files

When you create a function from a template, several files are created. For example, if you opted to use the Webhook + API Quickstart using JavaScript, the files generated would be a configuration file, function.json, and a source code file, index.js. The functions you create in a function app appear under the Functions menu item in the function app portal.

When you select a function in your function app, a code editor opens and displays the code for your function, as illustrated in the following screenshot.

As you can see in the preceding screenshot, there's a flyout menu on the right that includes a tab to View files. Selecting this tab shows the file structure that makes up your function.

## Testing your Azure function

Once you've created a function, you'll want to test it. There are a couple of approaches: manual execution and testing from within the Azure portal itself.

## Manual execution

You can start a function by manually triggering the configured trigger. For instance, if you are using an HTTP trigger - you can use a tool such as Postman or cURL to initiate an HTTP request to your function endpoint URL, which is available from the HTTP trigger definition (Get function URL).

## Testing in the Azure portal

The portal also provides a convenient way to test your functions. On the right side of the code window, there is a flyout tabbed navigation menu. This menu contains a Test item. Expanding the menu and selecting this tab gives you another way to execute your function and view the result. When you click Run in this test window, the results are displayed in the output window, along with a status code.

## Monitoring dashboard

The ability to monitor your functions is critical during development and in production. The Azure portal provides a monitoring dashboard available if you turn on the Application Insights integration. In the function app navigation menu, once you expand the function node you'll see a Monitor menu item. This monitor dashboard provides a quick way to view the history of function executions and displays the timestamp, result code, duration, and operation ID populated by Application Insights.

## Streaming log window
You're also able to add logging statements to your function for debugging in the Azure portal. The called methods for each language are passed a "logging" object, which may be used to log information to the log window located in a tabbed flyout menu located at the bottom of the code window.

The following JavaScript code snippet shows how to log a message using the `context.log` method (the context object is passed to the handler).

```Javascript
context.log('Enter your logging statement here');
```

We could do the same thing in C# using the `log.Info` method. In this case, the log object is passed to the C# method processing the function.

```C#
log.Info("Enter your logging statement here");
```

In PowerShell, use `Write-Host` to write to the log:

```PowerShell
Write-Host "Enter your logging statement here"
```

## Errors and warnings window
You can locate the errors and warnings window tab in the same flyout menu as the log window. This window will show compilation errors and warnings within your code.

# Exercise - Add logic to the function app

Let's continue with our gear drive example and add the logic for the temperature service. Specifically, we're going to receive data from an HTTP request.

## Function requirements

First, we need to define some requirements for our logic:

* Temperatures between 0-25 should be flagged as OK.
* Temperatures between 26-50 should be flagged as CAUTION.
* Temperatures above 50 should be flagged as DANGER.

## Add a function to our function app

As we discussed in the preceding unit, Azure provides templates that help you get started building functions. In this unit, we'll use the `HttpTrigger` template to implement the temperature service.

1. Sign in to the Azure portal .

2. Select the resource group from the first exercise by choosing All resources in the left-hand menu, and then selecting "learn-6cbe788c-f7ba-403d-8178-5c5a8f574ac1".

3. The resources for the group will then be displayed. Click the name of the function app that you created in the previous exercise by selecting the escalator-functions-xxxxxxx item (also indicated by the lightning bolt Function icon).

4. Select the Add (+) button next to Functions. This action starts the function creation process.

5. On the Azure Functions for JavaScript - getting started page, select In-portal and then select continue.

6. In the Create a function step, select More templates... and then select Finish and view templates.

7. In the list of all templates available to this function app, select HTTP trigger .

8. Enter `DriveGearTemperatureService` in the name field of the New Function dialog that appears. Leave the Authorization level as "Function" and press the Create button to create the function.

9 When your function creation completes, the code editor opens with the contents of the index.js code file. The default code that the template generated for us is listed in the following snippet.

```JavaScript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```

Our function expects a name to be passed in either through the HTTP request query string or as part of the request body. The function responds by returning the message Hello, {name}, echoing back the name that was sent in the request.

On the right-hand side of the source view, you'll find two tabs. The View files tab lists the code and config file for your function. Select function.json to view the configuration of the function, which should look like the following:

```JavaScript
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

This configuration declares that the function runs when it receives an HTTP request. The output binding declares that the response will be sent as an HTTP response.

## Test the function

> cURL is a command line tool that can be used to send or receive files. It's included with Linux, macOS, and Windows 10, and can be downloaded for most other operating systems. cURL supports numerous protocols like HTTP, HTTPS, FTP, FTPS, SFTP, LDAP, TELNET, SMTP, POP3, etc. For more information, refer to the links below:
>https://en.wikipedia.org/wiki/CURL <br>
> https://curl.haxx.se/docs/

To test the function, you can send an HTTP request to the function URL using cURL on the command line. To find the endpoint URL of the function, return to your function code and select the Get function URL link, as shown in the following screenshot. Save this link temporarily.

## Securing HTTP triggers

HTTP triggers let you use API keys to block unknown callers by requiring the key to be present on each request. When you create a function, you select the authorization level. By default, it's set to "Function", which requires a function-specific API key, but it can also be set to "Admin" to use a global "master" key, or "Anonymous" to indicate that no key is required. You can also change the authorization level through the function properties after creation.

Since we specified "Function" when we created this function, we will need to supply the key when we send the HTTP request. You can send it as a query string parameter named code, or as an HTTP header (preferred) named x-functions-key.

The function and master keys are found in the Manage section when the function is expanded. By default, they are hidden, and you need to display them.

1. Expand your function and select the Manage section, show the default Function Key, and copy it to the clipboard

2. Next, from the command line where you installed the cURL tool, format a cURL command with the URL for your function, and the Function key.

* Use a POST request.
* Add a Content-Type header value of type application/json.
* Make sure to replace the URL below with your own.
* Pass the Function Key as the header value x-functions-key.

```Bash
curl --header "Content-Type: application/json" --header "x-functions-key: <your-function-key>" --request POST --data "{\"name\": \"Azure Function\"}" https://<your-url-here>/api/DriveGearTemperatureService
```

> Caution <br>
> If you are on Windows, please run `cURL` from the command prompt. PowerShell has a `curl` command, but it's an alias for Invoke-WebRequest and is not the same as `cURL`.

> Note <nr>
> You can also test from an individual function's section with the Test tab on the side of a selected function, though you won't be able to verify the function key system is working, as it is not required here. Add the appropriate header and parameter values in the Test interface and click the Run button to see the test output.

## Add business logic to the function 

Next, let's add the logic to the function that checks temperature readings that it receives and sets a status for each.

Our function is expecting an array of temperature readings. The following JSON snippet is an example of the request body that we'll send to our function. Each reading entry has an ID, timestamp, and temperature.

```JSON
{
    "readings": [
        {
            "driveGearId": 1,
            "timestamp": 1534263995,
            "temperature": 23
        },
        {
            "driveGearId": 3,
            "timestamp": 1534264048,
            "temperature": 45
        },
        {
            "driveGearId": 18,
            "timestamp": 1534264050,
            "temperature": 55
        }
    ]
}
```

Next, we'll replace the default code in our function with the following code that implements our business logic.

Open the index.js file and replace it with the following code. Make sure to save the file after updating it.

```JavaScript
module.exports = function (context, req) {
    context.log('Drive Gear Temperature Service triggered');
    if (req.body && req.body.readings) {
        req.body.readings.forEach(function(reading) {

            if(reading.temperature<=25) {
                reading.status = 'OK';
            } else if (reading.temperature<=50) {
                reading.status = 'CAUTION';
            } else {
                reading.status = 'DANGER'
            }
            context.log('Reading is ' + reading.status);
        });

        context.res = {
            // status: 200, /* Defaults to 200 */
            body: {
                "readings": req.body.readings
            }
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please send an array of readings in the request body"
        };
    }
    context.done();
};
```

The logic we added is straightforward. We iterate over the array of readings and check the temperature field. Depending on the value of that field, we set a status of OK, CAUTION, or DANGER. We then send back the array of readings with a status field added to each entry.

Notice the log statements. When the function runs, these statements will add messages in the log window.

## Test our business logic

In this case, we're going to use the Test pane in the portal to test our function.

1. Open the Test window from the right-hand side flyout menu.

2. Paste the sample request into the request body text box.
```JSON
{
    "readings": [
        {
            "driveGearId": 1,
            "timestamp": 1534263995,
            "temperature": 23
        },
        {
            "driveGearId": 3,
            "timestamp": 1534264048,
            "temperature": 45
        },
        {
            "driveGearId": 18,
            "timestamp": 1534264050,
            "temperature": 55
        }
    ]
}
```

3. Select Run and view the response in the output pane. To see log messages, open the Logs tab in the bottom flyout of the page. The following screenshot shows an example response in the output pane and messages in the Logs pane.

You can see in the output pane that our status field has been correctly added to each of the readings.

If you navigate to the Monitor dashboard, you'll see that the request has been logged to Application Insights.

# Summary

You've learned how to use Azure Functions to host business logic services in the cloud. It's a great way to add hosted services to your solution that can scale and grow with your business. You focus on the code using the language of your choice, and Azure manages the infrastructure. Functions can integrate with other services, like Event Grid, GitHub, Twilio, Microsoft Graph, and Logic Apps to create complex and robust serverless workflows quickly and easily.