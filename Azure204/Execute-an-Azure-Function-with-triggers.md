# Introduction

Imagine a scenario where a busy hair salon has a recurring problem: their customers commonly miss their appointments. The appointments are reserved time slots. If a customer misses an appointment, the salon loses money. To fix this problem, the salon reaches out to you, a software developer. To improve the problem, you decide to send reminder text messages. These could be sent as soon as the appointment is scheduled or changed, and every morning, you'll send a text message to every customer with an appointment that day.

You need to create a service that can be easily scheduled, updated and scaled. You decide to solve this problem using a function app. You already know how to implement the logic to send a text message. Now you need to learn how to send the message at a specific time or when a specific event occurs. Luckily, Azure Functions supports a feature called triggers. Triggers are used to determine how an Azure function is executed.

## Learning objectives

In this module, you will:

* Determine which trigger works best for your business needs
* Create a timer trigger to invoke a function on a consistent schedule
* Create an HTTP trigger to invoke a function when an HTTP request is received
* Create a blob trigger to invoke a function when a blob is created or updated in Azure Storage

# Determine the best trigger for your Azure function

An Azure function app doesn't do work until something tells it to execute. For example, we could create an Azure function to send out a reminder text message to our customers before an appointment. If we don't tell the function when it should run, our customers will never receive a message.

Here, you'll examine triggers at a high level and explore the most common types of triggers.

## What is a trigger?


A trigger is an object that defines how an Azure function is invoked. For example, if you want a function to execute every 10 minutes, you could use a timer trigger.

Every function must have exactly one trigger associated with it. If you want to execute a piece of logic that runs under multiple conditions, you need to create multiple functions that share the same core function code.

In this module, we're going to focus on the timer, HTTP, and blob trigger types.

## Types of triggers

Azure Functions support a wide range of trigger types. Here are some of the most common types:

| Type | Purpose |
| ---- | ------- |
| Timer | Execute a function at a set interval. |
| HTTP	| Execute a function when an HTTP request is received. |
| Blob	| Execute a function when a file is uploaded or updated in Azure Blob storage.|
| Queue	| Execute a function when a message is added to an Azure Storage queue.|
| Azure Cosmos DB |	Execute a function when a document changes in a collection.|
| Event Hub | Execute a function when an event hub receives a new event.|

## What is a binding?

A binding is a connection to data within your function. Bindings are optional and come in the form of input and output bindings. An input binding is the data that your function receives. An output binding is the data that your function sends.

Unlike a trigger, a function can have multiple input and output bindings.

In the next exercise, we'll run a function on a schedule using a Timer trigger.

# Run an Azure Function on a schedule

It's common to execute a piece of logic at a set interval. Imagine you're a blog owner and you notice that your subscribers aren't reading your most recent posts. You decide that the best action is to send an email once a week to remind them to check your blog. You implement this logic using an Azure function app with a timer trigger to invoke your function weekly.

## What is a timer trigger?

A timer trigger is a trigger that executes a function at a consistent interval. To create a timer trigger, you need to supply two pieces of information.

1. A Timestamp parameter name, which is simply an identifier to access the trigger in code.
2. A Schedule, which is a CRON expression that sets the interval for the timer.

## What is a CRON expression?

A CRON expression is a string that consists of six fields that represent a set of times.

The order of the six fields in Azure is: `{second} {minute} {hour} {day} {month} {day of the week}`.

For example, a CRON expression to create a trigger that executes every five minutes looks like:

```Log
0 */5 * * * *
```

At first, this string may look confusing. We'll come back and break down these concepts when we have a deeper look at CRON expressions.

To build a CRON expression, you need to have a basic understanding of some of the special characters.

| Special character	 | Meaning | Example |
| -----------------  | ------- | ------- |
| *	                 | Selects every value in a field | An asterisk "*" in the day of the week field means every day.
| ,	                 | Separates items in a list	| A comma "1,3" in the day of the week field means just Mondays (day 1) and Wednesdays (day 3).
| -	                 | Specifies a range	| A hyphen "10-12" in the hour field means a range that includes the hours 10, 11, and 12.
| /	                 | Specifies an increment |	A slash "*/10" in the minutes field means an increment of every 10 minutes.

Now we'll go back to the original CRON expression example. Let’s try to understand it better by breaking it down field by field.

```Log
0 */5 * * * *
```
The __first field__ represents seconds. This field supports the values 0-59. Because the field contains a zero, it selects the first possible value, which is one second.

The second field represents minutes. The value `"*/5" contains two special characters. First, the asterisk (*)` means "select every value within the field." Because this field represents minutes, the possible values are 0-59. The second special character is the slash (/), which represents an increment. When you combine these characters together, it means for all values 0-59, select every fifth value. An easier way to say that is simply "every five minutes."

The remaining four fields represent the hour, day, month, and weekday of the week. An asterisk for these fields means to select every possible value. In this example, we select "every hour of every day of every month."

When you put all the fields together, the expression is read as "on the first second, of every fifth minute of every hour, of every day, of every month".

## How to create a timer trigger

You can create a timer trigger in the Azure portal. In your Azure function app, select `timer trigger` from the list of trigger templates. Enter the logic that you want to execute. Supply a __Timestamp parameter name and the CRON expression__.

In this module we'll focus on creating triggers in the portal but you can also create triggers programmatically using Core Tools, Visual Studio or Visual Studio Code.

A timer trigger invokes an Azure function app on a consistent schedule. To define the schedule for a timer trigger, we build a CRON expression, which is a string that represents a set of times.

# Exercise - Create a timer trigger

## Create an Azure function app

Let’s start by creating an Azure Function app in the portal.

1. Sign into the Azure portal  using the same account you activated the sandbox with.

2. On the Azure portal menu or from the Home page, select Create a resource.

3. Select Compute, and then select Function App. You can also optionally use the search bar to locate and create the new resource.

4. Enter a globally unique App name.

5. Select the Concierge Subscription.

6. Select the existing Resource group learn-a12ca57c-e345-4e29-bcda-411a21bdeece.

7. Choose Windows as your OS.

8. Choose Consumption Plan for your Hosting Plan. When using the Consumption Plan type you're charged for each execution of your function and resources are automatically allocated based on your application workload.

9. Select a Location close to you.

10. For Runtime Stack, leave as default `.NET`, which is the language in which we implement the function examples in this exercise.

## Create a timer-triggered function

Now we're going to create a timer trigger inside our function.

1. Select the Add (+) button next to Functions. This action starts the function creation process.

2. On the Azure Functions for PowerShell - getting started page, select In-portal and then select Continue.

3/ In the list of quick start templates, select Timer and then select Create at the bottom of the screen.

## Configure the timer trigger

We have an Azure function app with logic to print a message to the log window. We're going to set the schedule of the timer to execute every 20 seconds.

1. Select Integrate.

2. Enter the following value into the Schedule box:

```Log
*/20 * * * * *
```

# Execute an Azure function with an HTTP request

An HTTP request is a common operation on most platforms and devices. Whether it's a request to look up a word in a dictionary or to get the local weather, we send HTTP requests all the time. Azure Functions allows us to quickly create a piece of logic to execute when an HTTP request is received.

Here, you'll learn how to create and invoke an Azure function using an HTTP trigger. You'll also explore some of the customization options that are available.

## What is an HTTP trigger?

An HTTP trigger is a trigger that executes a function when it receives an HTTP request. HTTP triggers have many capabilities and customizations, including:

* Provide authorized access by supplying keys.
* Restrict which HTTP verbs are supported.
* Return data back to the caller.
* Receive data through query string parameters or through the request body.
* Support URL route templates to modify the function URL.

When you create an HTTP trigger, select a programming language, provide a trigger name, and select an Authorization level.

## What is an HTTP trigger Authorization level?

An HTTP trigger Authorization level is a flag that indicates if an incoming HTTP request needs an API key for authentication reasons.

There are three Authorization levels:

* Function
* Anonymous
* Admin

The Function and __Admin__ levels are "key" based. To send an HTTP request, you must supply a key for authentication. There are two types of keys: _function_ and _host_. The difference between the two keys is their scope. Function keys are specific to a function. Host keys apply to all functions inside the function app. If your Authorization level is set to __Function__, you can use either a function or a host key. If your Authorization level is set to Admin, you must supply a host key.

The Anonymous level means that there's no authentication required. We use this level in our exercise.

## How to create an HTTP trigger

Just like a timer trigger, you can create an HTTP trigger through the Azure portal. Inside your Azure function, you select __HTTP trigger__ from the list of predefined trigger types. Then you enter the logic that you want to execute and make any customizations like restricting the use of certain HTTP verbs.

One setting that's important to understand is _Request parameter name_. This setting is a string that represents the name of the parameter that contains the information about an incoming HTTP request. By default, the name of the parameter is _req_.

## How to invoke an HTTP trigger

To invoke an HTTP trigger, you send an HTTP request to the URL for your function. To get this URL, go to the code page for your function and select the Get function URL link.

After you have the URL for your function, you can send HTTP requests. If your function receives data, remember that you can use either query string parameters or supply the data through the request body.

An HTTP trigger invokes an Azure function when it receives an HTTP request to its function URL. HTTP triggers allow you to receive data and return data back to the caller.

# Exercise - Create an HTTP trigger

In this unit, we're going to create an Azure function that accepts an HTTP request with a single string. The function returns a string back to the caller to represent success or failure. We'll continue working on the function from the previous exercise.

## Create an HTTP trigger

Let's continue using our existing Azure Functions application and add an HTTP trigger.

1. Make sure you are signed into the Azure portal  using the same account you activated the sandbox with.

2. On the Azure portal menu or from the Home page, select All resources.

3. Select your function app.

4. Select the Add (+) button next to Functions. This action starts the function creation process.

5. In the list of all templates available to this function app, select HTTP trigger. 

6. In the New Function dialog, choose a name for the function and select Anonymous from the Authorization level dropdown.

7. Select Create to create the function.

8. Take a quick look at the auto-generated code to get an idea about what's going on. The req parameter represents the incoming request and contains a name parameter. We check to see if name has a value. If it does, we return a greeting. Otherwise, we return an error message.

## Get your function URL

Now that we've created the HTTP trigger, let's get the function URL so we can begin to make a request.

1. To the right of Run, select Get function URL.

2. Select Copy, then close the function URL popup.

## Issue a GET request to your HTTP trigger

We now have our function URL copied to our clipboard. Let's issue a GET request to see if we get a response.

1. Open a new tab in your web browser.

2. Paste the URL into the address bar.

3. Add a query string parameter called name with your name for example `.../api/HttpTriggerCSharp1?name=Jesse`

4. Press `ENTER` to submit the request.

# Execute an Azure function when a blob is created

Imagine you're a photographer and you have a website where you display your pictures of the day. Because you're busy, you don't have a consistent upload schedule, but you want to notify your fans when you upload a picture. You decide to create an Azure function to automatically send a tweet whenever you upload an image to your Azure Storage blob container.

Here, you learn how to create a blob trigger and instruct it to monitor a specific location in your Azure Storage blob container.

## What is Azure Storage?

Azure Storage is Microsoft's cloud storage solution that supports all types of data, including: blobs, queues, and NoSQL. The goal of Azure Storage is to provide data storage that's:

* Highly available
* Secure
* Scalable
* Managed

We're not going to focus on Azure Storage too much. Instead, we use it to create blobs that will trigger our function to run.

## What is Azure Blob storage?

Azure Blob storage is an object storage solution that's designed to store large amounts of unstructured data.

For example, Azure Blob storage is great at doing things like:

* Storing files
* Serving files
* Streaming video and audio
* Logging data

There are three types of blobs: __block blobs__, __append blobs__, and __page blobs__. Block blobs are the most common type. They allow you to store text or binary data efficiently. Append blobs are like block blobs, but they're designed more for append operations like creating a log file that's being constantly updated. Finally, page blobs are made up of pages and are designed for frequent random read and write operations.

## What is a blob trigger?

A blob trigger is a trigger that executes a function when a file is uploaded or updated in Azure Blob storage. To create a blob trigger, you create an Azure Storage account and provide a location that the trigger monitors.

## How to create a blob trigger

Just like the other triggers we've seen so far, we create a blob trigger in the Azure portal. Inside your Azure function, select __Blob trigger__ from the list of predefined trigger types. Then enter the logic to execute when a blob is created or updated.

One setting that you'll want to look at is the __Path__. The __Path__ tells the blob trigger where to monitor to see if a blob is uploaded or updated. By default, the Path value is:

```Log
samples-workitems/{name}
```

Let's break down this concept into two pieces: _samples-workitems_ and _{name}_. The first part, samples-workitems, represents the blob container that the trigger monitors. The second part, {name} means that every type of file will cause the trigger to invoke the function. The function is invoked because there's no filter. For example, I could make the trigger invoke the function only when a PNG file is added by using syntax like:

```Log
samples-workitems/{name}.png
```

The last significant piece of information with this concept is the text name. The name represents a parameter in your Azure function that receives the name of the added file. For example, if I upload a file named resume.txt, my Azure function receives that value as a string through a parameter called name.

A blob trigger invokes an Azure function when it sees activity at a specific location in your Azure Storage blob account. You set the location to monitor by modifying the Path value in the Azure portal.

# Exercise - Create a Blob trigger

## Create a blob trigger

Again, let's continue using our existing Azure Functions application and add a blob trigger.

1. Make sure you are signed into the Azure portal  using the same account you activated the sandbox with.

2. On the Azure portal menu or from the Home page, select All resources.

3. Select your function app.

4. Point to Functions and select the plus (+) icon.

5. Select Azure Blob Storage trigger.

6. If you see a message saying Extensions not installed, select Install. Dependency installation can take a couple of minutes, so please be patient. Wait until the installation completes before continuing.

7. Leave the Name set to the default value.

8. Leave the Path set to the default value.

9. Select the new link next to the Storage account connection dropdown. In the Storage Account selection dialog that appears, select the storage account for this function app.

10. Once you have returned to the New Function screen, select Create to create the function.

## Create a blob container
Now that we've created a blob trigger, let's use the Storage Explorer to create a blob and trigger the function.
1. Open the storage account you used (or created) in a new tab.

2. In the Azure portal, search for and select Storage accounts and then filter by the name.

3. Click on the Storage Explorer (preview) section - this will open a new panel where you can work with blobs and files.

Remember that our blob trigger is monitoring only the location described in the Path field. By default, our path should be:

> samples-workitems/{name}

We need to create a container called samples-workitems.

1. Right-click BLOB CONTAINERS and select Create Blob Container.

2. Enter samples-workitems as the name, leave the access level at the default Private setting, and select OK.

## Turn on your blob trigger

Now that we've created our container to monitor, let's run our function so we can see output when a blob is created.

1. Switch back to the browser tab with your Azure Function (or reopen it).

2. Select your blob trigger to open the code screen.

3. Open the Logs tab at the bottom of the screen.

## Create a blob

Our blob trigger is now up and listening for activity. Let's create a blob to see if we get a log message.

1. Switch back to the browser tab with Storage Explorer.

2. In Storage Explorer, select the samples-workitems container from the BLOB CONTAINERS list.

3. Select Upload from the toolbar.

4. Select any file from your computer.

5. Select Upload.

6. Switch back to the Azure Function tab and check the output logs for a message that displays what file was uploaded. Your blob trigger should automatically execute. Note that if you click the Run button in the function window, it will likely error due to the default value that is specified in the Test request body. You will need to change the path in the request body window to a valid file in order for the test dialog to execute successfully.