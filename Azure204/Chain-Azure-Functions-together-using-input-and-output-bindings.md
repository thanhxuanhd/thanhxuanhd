# Introduction

Suppose you run a social networking site for professionals. You're allowing your users to upload their headshot images to be posted on their profile. To reduce the workload on the web server, you want to create a serverless backend using Azure Functions to process this data. You want to create an image thumbnail and then save it to permanent storage.

The power of Azure Functions comes mainly from the integrations that it offers with a range of data sources and services, which are defined with _bindings_. With bindings, developers interact with other data sources and services without worrying about how the data flows to and from their function.

## Learning objectives

In this module, you will:

- Explore the types of data sources that can be accessed through bindings
- Read data from Azure Cosmos DB by using Azure Functions
- Store data in Azure Cosmos DB by using Azure Functions
- Send messages to Azure Queue Storage by using Azure Functions

# Explore input and output binding types

Accessing and processing data are key tasks in many software solutions. Consider some of these scenarios:

- You've been asked to implement a way to move incoming data from Blob storage to Azure Cosmos DB.
- You want to post incoming messages to a queue for processing by another component in your enterprise.
- Your service needs to grab gamer scores from a queue and update an online scoreboard.

All of these examples are about moving data. The data source and destinations differ from scenario to scenario, but the pattern is similar. You connect to a data source, and you read and write data. Azure Functions helps you integrate with data and services by using bindings.

## What is a binding?

In Azure Functions, bindings provide a declarative way to connect to data from within your code. They make it easier to integrate with data streams consistently in a function. You can have multiple bindings providing access to different data elements. This is powerful because you can connect to your data sources without having to code specific connection logic (like database connections or web API interfaces).

## Types of bindings

There are two kinds of bindings you can use with functions:

1. **Input binding** - An input binding is a connection to a data source. Our function can read data from these inputs.

2. **Output binding** - An output binding is a connection to a data destination. Our function can write data to these destinations.

## Types of supported bindings

The _type_ of binding defines where we are reading or sending data. There is a binding to respond to web requests and a large selection of bindings to interact directly with various Azure services as well as third-party services.

A binding type can be used as an input, an output or both. For example, a function can write to Azure Blob Storage output binding, but a blob storage update could trigger another function.

Some common binding types are listed below:

- Blob Storage
- Azure Service Bus Queues
- Azure Cosmos DB
- Azure Event Hubs
- External Files
- External Tables
- HTTP endpoints

These types are just a sample. There are more, plus functions have an extensibility model to add more bindings.

## Binding properties

Three properties are required in all bindings. You may have to supply additional properties based on the type of binding and storage you are using.

1. **Name** - Defines the function parameter through which you access the data. For example, in a queue input binding, this is the name of the function parameter that receives the queue message content.

2. **Type** - Identifies the type of binding, i.e., the type of data or service we want to interact with.

3. **Direction** - Indicates the direction data is flowing, i.e., is it an input or output binding?

Additionally, most binding types also need a fourth property:

4. **Connection** - Provides the name of an app setting key that contains the connection string. Bindings use connection strings stored in app settings to keep secrets out of the function code. This makes your code more configurable and secure.

## Create a binding

Bindings are defined in JSON. A binding is configured in your function's configuration file, which is named function.json and lives in the same folder as your function code.

Let's examine a sample input binding:

```JSON
    {
      "name": "headshotBlob",
      "type": "blob",
      "path": "thumbnail-images/{filename}",
      "connection": "HeadshotStorageConnection",
      "direction": "in"
    },
```

To create this binding, we:

1. Create a binding in our function.json file.
2. Provide the value for the `name` variable. In this example, the variable holds the blob data.
3. Provide the storage type. In the preceding example, we are using Blob storage.
4. Provide the `path`, which specifies the container and the item name that goes in it. The `path` property is required when using the Blob trigger, and should be provided in the style shown here, with curly braces around the filename portion of the path. This creates a _binding expression_ that allows you to reference the blob's name in other bindings and in your function's code. In this example, a parameter on the function named filename would be populated with the _filename_ of the blob that triggered the function.
5. Provide the _connection_ string setting name defined in the application's settings file. It's used as a key to find the connection string to connect to your storage account.
6. Define the direction as in. It reads data from the blob.

Bindings are used to connect to data in your function. In this example, we used an input binding to connect user images to be processed by our function as thumbnails.

# Exercise - Explore input and output binding types

The following is a high-level illustration of what we're going to build in this exercise.

We'll create a function that will start when it receives an HTTP request and will respond to each request by sending back a message. The parameters req and res are the trigger binding and output binding, respectively.

## Create a function app

Let's create a function app that we'll use throughout this entire module. A function app lets you group functions as a logical unit for easier management, deployment, and sharing of resources.

1. Sign into the Azure portal using the same account you activated the sandbox with.

2. On the Azure portal menu or from the Home page, select Create a resource.

3. Select Compute > Function App.

4. Set the function app properties as follows:

| Property          | Suggested value               | Description                                                                       |
| ----------------- | ----------------------------- | --------------------------------------------------------------------------------- |
| Subscription      | Concierge Subscription        | The Azure subscription that you want to use for this Azure Cosmos DB account.     |
| Resource Group    | [sandbox resource group name] | This field is pre-populated with the resource group from your sandbox.            |
| Function App name | Globally unique name          | Name that identifies your new function app. Valid characters are a-z, 0-9, and -. |
| Publish           | Code                          | Option to publish code files or a Docker container.                               |
| Runtime Stack     | Node.js                       | The sample code in this module is written in JavaScript.                          |
| Region            | Central US                    | Choose the region nearest you.                                                    |

5. Select Review + create and then Create to provision and deploy the function app.

6. Select the Notification icon in the upper-right corner of the portal and watch for a Deployment in progress message similar to the following message.

7. Deployment can take some time. So, stay in the notification hub and watch for a Deployment succeeded message similar to the following message.

8. Once the function app is deployed, go to All resources in the portal. The function app will be listed with type App Service and has the name you gave it. Select the function app from the list to open it.

> Tip <br>If you are having trouble finding your function apps in the portal, find out how to add function apps to your favorites in the portal.

## Create a function

Now that we have a function app, it's time to create a function. A function is activated through a trigger. In this module, we'll use an HTTP trigger.

1. Select the Add (+) button next to **Functions**. This action starts the function creation process.

2. On the **Azure Functions for JavaScript** - getting started page, select In-portal and then select continue.

3. In the **Create a function** step, select More templates... and then select Finish and view templates.

4. In the list of all templates available to this function app, select **HTTP Trigger** .

5. On the **New Function** screen, you can change the name if you want, but leave the **Authorization level** as Function, and then select **Create**. The Authorization level option determines what kind of key is used to securely access your function. Choosing _Function_ requires callers of your function to provide a function-specific key with their requests.

6. In your new function, click the </> Get function URL link at the top right, select default (Function key), and then select Copy.

7. Paste the function URL you copied into the address bar of a new tab in your browser.

8. Add the query string value &name=Azure to the end of this URL, and then press Enter on your keyboard to execute the request. You should see a response similar to the following response returned by the function displayed in your browser.

```Output
Hello Azure
```

As you can see from this exercise so far, you have to select a trigger type when you create a function. Every function has a single trigger. In this example, we're using an HTTP trigger, which means that our function starts when it receives an HTTP request. The default implementation, shown in the following screenshot in JavaScript, responds with the value of the parameter name it received in the query string or body of the request. If no string was provided, the function responds with a message that asks whomever is calling to supply a name value.

All of this code is in the index.js file in this function's folder.

Let's look briefly at the function's other file, the function.json config file. This configuration data is shown in the following JSON listing.

```JSON
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

As you can see, this function has a trigger binding named req of type `httpTrigger` and an output binding named res of type HTTP. In the preceding code for our function, we saw how we accessed the payload of the incoming HTTP request through our req parameter. Similarly, we sent an HTTP response simply by setting our res parameter. Bindings really do take care of some of the heavy lifting for us.

## Explore binding types

1. Notice under the function entry there is a set of menu items as shown in the following screenshot.

2. Select the Integrate menu item to open the integration tab for our function. If you've been following along with this unit, the integrate tab should look very similar to the following screenshot.

3. Select + New Input under the Inputs column. A list of all possible input binding types is displayed as shown in the following screenshot.

   Take a moment to consider each of these input bindings and how you might use them in a solution. There are a lot to choose from. This list might even have changed by the time you read this module, as we continue to support more data sources.

4. We'll get back to adding input bindings later in the module but, for now, select Cancel to dismiss this list

5. Select + New Output under the Outputs column. A list of all possible output binding types is displayed as shown in the following screenshot.

As you can see, there are several output binding types at your disposal. We'll get back to adding output bindings later in the module but, for now, select Cancel to dismiss this list.

So far, we've learned how to create a function app and add a function to it. We've seen a simple function in action, one that runs when an HTTP request is made to it. We've also explored the Azure portal UI and types of input and output binding that are available to our functions. In the next unit, we'll use an input binding to read text from a database.

# Read data with input bindings

To connect to a data source, you have to configure an _input binding_. This binding makes it possible to write minimal code to create a message. You don't have to write code for tasks such as opening a storage connection. The Azure Functions runtime and binding take care of those tasks for you.

## Input binding types

There are multiple types of input. However, not all types support both input and output. You'll use them whenever you want to ingest data of that type. Here, we'll look at the types that support input bindings and when to use them.

- Blob storage - Blob storage bindings allow you to read from a blob.

- Azure Cosmos DB - The Azure Cosmos DB input binding uses the SQL API to retrieve one or more Azure Cosmos DB documents and passes them to the input parameter of the function. The document ID or query parameters can be determined based on the trigger that invokes the function.

- Microsoft Graph - Microsoft Graph input bindings allow you to read files from OneDrive, read data from Excel, and get auth tokens so you can interact with any Microsoft Graph API.

- Mobile Apps - The Mobile Apps input binding loads a record from a mobile table endpoint and passes it into your function.

- Table storage - You can read data and work with Azure Table storage.

To create a binding as an input, you must define the `direction` as `in`. The parameters for each type of binding may vary.

## What is a binding expression?

A binding expression is specialized text in `function.json`, function parameters, or code that is evaluated when the function is invoked to yield a value. For example, if you have a Service Bus Queue binding, you could use a binding expression to get the name of the queue from App Settings.

## Types of binding expressions

- App settings
- Trigger file name
- Trigger metadata
- JSON payloads
- New GUID
- Current date and time

Most expressions are identified by wrapping them in curly braces. However, app setting binding expressions are wrapped in percent signs rather than curly braces. For example if the blob output binding path is `{Environment}/newblob.txt` and the Environment app setting value is Development, a blob will be created in the Development container.

## Summary

Input bindings allow you to connect your function to a data source. You can connect to several types of data sources, and the parameters for each vary. To resolve values from various sources, you can use binding expressions in the function.json file, function parameters, or code.

# Exercise - Read data with input bindings

Imagine that you want to create a simple bookmark lookup service. Your service is read-only initially. If users want to find an entry, they send a request with the ID of the entry and you return the URL. The following flowchart explains the flow:

When users send you a request with some text, you try to find an entry in your back-end database that contains this text as a key or ID. You return a result that indicates whether you found the entry.

When the Azure function receives a request with the bookmark ID, it first checks whether the request is valid, if not an error response is generated. For valid requests, the function checks if the bookmark ID is present in the Azure Cosmos DB database, if not present an error response is generated. If the bookmark ID is found, a success response is generated.

You need to store the data somewhere. In this flowchart, the data store is an Azure Cosmos DB instance. But how do you connect to a database from a function and read data? In the world of functions, you configure an input binding for that job. Configuring a binding through the Azure portal is straightforward. As you'll see shortly, you don't have to write code for such tasks as opening a storage connection. The Azure Functions runtime and bindings take care of those tasks for you.

## Create an Azure Cosmos DB account

### Create a database account

A database account is a container for managing one or more databases. Before we can create a database, we need to create a database account.

1. Make sure you are signed into the Azure portal using the same account you activated the sandbox with.

2. On the Azure portal menu or from the Home page, select Create a resource.

3. Select Databases > Azure Cosmos DB. You may need to search for the Cosmos.

4. In the Create Azure Cosmos DB Account page, enter the settings for the new Azure Cosmos DB account.

| Property       | Suggested value                            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| -------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Subscription   | Concierge                                  | Subscription The Azure subscription that you want to use for this Azure Cosmos DB account.                                                                                                                                                                                                                                                                                                                                                        |
| Resource Group | learn-9274146a-d90d-4959-8daf-f99199afa5dc | This field is pre-populated with the resource group from your sandbox.                                                                                                                                                                                                                                                                                                                                                                            |
| Account Name   | Globally unique name                       | Enter a unique name to identify this Azure Cosmos DB account. Because documents.azure.com is appended to the name that you provide to create your URI, use a unique but identifiable name. <br>The account name can contain only lowercase letters, numbers, and the hyphen (-) character, and it must contain 3 to 50 characters.                                                                                                                |
| API            | Core (SQL)                                 | The API determines the type of account to create. Azure Cosmos DB provides five APIs to suit the needs of your application: SQL (document database), Gremlin (graph database), MongoDB (document database), Azure Table, and Cassandra, each of which currently require a separate account. <br> Select Core (SQL). At this time, the Azure Cosmos DB trigger, input bindings, and output bindings only work with SQL API and Graph API accounts. |
| Location       | Central US                                 | Choose the region nearest you.                                                                                                                                                                                                                                                                                                                                                                                                                    |

Leave all of the other fields in the New account pane at their default values.

5. Select Review + create to review and validate the configuration.

6. On the next screen, select Create to provision and deploy the database account.

7. Deployment can take some time. So, wait for a Deployment succeeded message in the Notification Hub before proceeding.

### Add a container

In Azure Cosmos DB, a `container` holds arbitrary user-generated entities. Inside a container, we store documents.

Let's use the Data Explorer tool in the Azure portal to create a database and collection.

1. Select Data Explorer from the left hand side menue, and then New Container.

| Property      | Suggested value  | Description                                                                                                                                                                                                                                                                                                                                                                           |
| ------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Database ID   | func-io-learn-db | Database names must contain from 1 through 255 characters, and they cannot contain /, \, #, ?, or a trailing space.<br> You are free to enter whatever you want here, but we suggest func-io-learn-db as the name for the new database, and that's what we'll refer to in this unit.                                                                                                  |
| Container ID  | Bookmarks        | Enter Bookmarks as the name for our new collection. Container IDs have the same character requirements as database names.                                                                                                                                                                                                                                                             |
| Partition key | /id              | The partition key specifies how the documents in Azure Cosmos DB collections are distributed across logical data partitions. We will use the id field as a convenience, as we are not concerned with database performance in this module. If you would like to learn more about Azure Cosmos DB partition key strategies, please explore the Microsoft Learn Azure Cosmos DB modules. |
| Throughput    | 1000 RU          | Change the throughput to 1000 request units per second (RU/s). If you want to reduce latency, you can scale up the performance later.                                                                                                                                                                                                                                                 |

2. Select OK. The Data Explorer displays the new database and container. Inside the database, we've defined a container. Next, we'll add some data, also known as items.

### Add test data

We've defined a container in our database called Bookmarks. We want to store a URL and ID in each item, like a list of web page bookmarks.

You'll add data to the new container using Data Explorer.

1. In Data Explorer, the new database appears in the Containers pane. Expand the func-io-learn-db database, expand the Bookmarks collection, select Items, and then select New Item.

2. Replace the default content of the new item with the following JSON.

```JSON
{
    "id": "docs",
    "url": "https://docs.microsoft.com/azure"
}
```

3. After you've added the JSON to the Items tab, select Save.

Notice that there are more properties than the ones we added. They all begin with an underline (\_rid, \_self, \_etag, \_attachments, \_ts). These are properties generated by the system to help manage the document.

| Property      | Description                                                                                                                                                                             |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| \_rid         | The resource ID is a unique identifier that is also hierarchical per the resource stack on the resource model. It is used internally for placement and navigation of the item resource. |
| \_self        | The unique addressable URI for the resource.                                                                                                                                            |
| \_etag        | Required for optimistic concurrency control.                                                                                                                                            |
| \_attachments | The addressable path for the attachments resource.                                                                                                                                      |
| \_ts          | The time stamp of the last update of this resource.                                                                                                                                     |

4. Let's add a few more items into the container by clicking New Item at the top. Create four more items with the following content. Remember to save your work.

```JSON
{
    "id": "portal",
    "url": "https://portal.azure.com"
}
```

```JSON
{
    "id": "learn",
    "url": "https://docs.microsoft.com/learn"
}
```

```JSON
{
    "id": "marketplace",
    "url": "https://azuremarketplace.microsoft.com/marketplace/apps"
}
```

```JSON
{
    "id": "blog",
    "url": "https://azure.microsoft.com/blog"
}
```

You now have a few entries in your Bookmarks container. Our scenario will work as follows. If a request arrives with, for example, "id=docs", you'll look up that ID in your bookmarks container and return the URL https://docs.microsoft.com/azure. Let's make an Azure function that looks up values in this container.

### Create your function

1. Navigate to the function app that you created in the preceding unit. Click Home > (in recent) Fuction App.
2. Click Fuctions on the left side menue.

3. Select the Add (+) button next to Functions to start the function creation process. The page displays the complete set of supported triggers.

4. Select HTTP trigger

5. Fill out the New Function dialog that appears to the right using the following values.

| Field               | Value         |
| ------------------- | ------------- |
| Name                | find-bookmark |
| Authorization level | Function      |

Select Create to create your function. This action opens the index.js file in the code editor and displays a default implementation of the HTTP-triggered function.

### Verify the function

You can verify what we have done so far by testing our new function as follows:

1. In your new function, click Get function URL at the top right, select default (Function key), and then click Copy.

2. Paste the function URL you copied into your browser's address bar. Add the query string value `&name=<your fuction name>` to the end of the URL and press Enter to execute the request. You should get a response from the Azure Function right in the browser.

Now that we have our bare-bones function working, let's turn our attention to reading data from our Azure Cosmos DB, or in our scenario, our Bookmarks container.

## Add an Azure Cosmos DB input binding

To read data from the database, you need to define an input binding. As you'll see, you can configure a binding that can talk to your database in just a few steps.

1. Select Integrate in the left pane to open the integration tab. The template you used created an HTTP trigger and an HTTP output binding. Now add your new Azure Cosmos DB input binding.

2. Select +Add Input. A list of all possible input binding types is displayed in the drop down menue on the right.

3. In the list, select Azure Cosmos DB, and then select New under the Cosmos DB account connection. This action opens the Azure Cosmos DB input configuration page. Next, you'll set up a connection to your database.

4. If the following message appears in the Azure Cosmos DB input configuration UI telling you that you must install an extension, select Install.

5. Next to the Azure Cosmos DB account connection box, select new. This action opens the Connection window, where you will enter the settings for the new connection.

| Property         | Suggested value         | Description                                                                               |
| ---------------- | ----------------------- | ----------------------------------------------------------------------------------------- |
| Connection       | Azure Cosmos DB account | This field is selected by default.                                                        |
| Subscription     | Concierge Subscription  | The Azure subscription that you want to use for this Azure Cosmos DB account.             |
| Database account | Cosmos DB account       | Select the Account Name that you specified when you created your Azure Cosmos DB account. |

6. Select Select to create your connection.

A new connection to the database is configured and is shown in the Azure Cosmos DB account connection field.

> Note <br>If you're curious about the connection endpoint, click show value to reveal the connection string.

You want to look up a bookmark with a specific ID, so let's tie an ID that we receive in the query string to the binding.

7. In the Document ID (optional) field, enter {id}.

This syntax is known as a binding expression. The function is triggered by an HTTP request that uses a query string to specify the ID to look up. Since IDs are unique in our collection, the binding will return either 0 (not found) or 1 (found) documents.

8. Carefully fill out the remaining fields on this page using the values in the following table. At any time, you can click on the information icon to the right of each field name to learn more about the purpose of each field.

| Property                 | Suggested value  | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------ | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Document parameter name  | bookmark         | The name used to identify this binding in your code.                                                                                                                                                                                                                                                                                                                                                                                                                             |
| Database name            | func-io-learn-db | The database to work with. This value is the database name we set earlier in this lesson.                                                                                                                                                                                                                                                                                                                                                                                        |
| Collection Name          | Bookmarks        | The collection from which we'll read data. This setting was defined earlier in the lesson.                                                                                                                                                                                                                                                                                                                                                                                       |
| SQL Query (optional)     | Leave blank      | We are only retrieving one document at a time based on the ID. So, filtering with the Document ID field is a better than using a SQL Query in this instance. We could craft a SQL Query to return one entry `(SELECT * from b where b.ID = {id})`. That query would indeed return a document, but it would return it in a document collection. Our code would have to manipulate a collection unnecessarily. Use the SQL Query approach when you want to get multiple documents. |
| Partition key (optional) | {id}             | Add the partition key that we defined when we created the Bookmarks Azure Cosmos DB collection earlier. The key entered here (specified in input binding format {<key>}) must match the one in the collection.                                                                                                                                                                                                                                                                   |

9. Select Save to save all changes to this binding configuration.

Now that you have your binding defined, it's time to use it in your function.

### Update function implementation

1. Select your function, find-bookmark, to open index.js in the code editor. You've added an input binding to read from your database, so update the logic to use this binding.

2. Replace all code in index.js with the code from the following snippet and hit Save.

```Javascript
module.exports = function (context, req) {

    var bookmark = context.bindings.bookmark

    if(bookmark){
            context.res = {
            body: { "url": bookmark.url },
            headers: {
            'Content-Type': 'application/json'
            }
        };
    }
    else {

        context.res = {
            status: 404,
            body : "No bookmarks found",
            headers: {
            'Content-Type': 'application/json'
            }
        };
    }

    context.done();
};
```

An incoming HTTP request triggers the function, and an id query parameter is passed to the Azure Cosmos DB input binding. If the database finds a document that matches this ID, then the bookmark parameter will be set to the located document. In that case, we construct a response that contains the URL value found in the bookmarked document. If no document is found matching this key, we would respond with a payload and status code that tells the user the bad news.

### Try it out

1. Select Get function URL at the top right, select default (Function key), and then select Copy to copy the function's URL.

2. Paste the function URL you copied into your browser's address bar. Add the query string value &id=docs to the end of this URL and press the Enter key on your keyboard to execute the request. You should see a response that includes a URL to that resource.

3. Replace &id=docs with &id=missing, and observe the response.

In this unit, we created our first input binding manually to read from an Azure Cosmos DB database. The amount of code we wrote to search our database and read data was minimal, thanks to bindings. We did most of our work configuring the binding declaratively, and the platform took care of the rest.

In the next unit, we'll add more data to our bookmarks collection through an Azure Cosmos DB output binding.

# Write data with output bindings

As with input bindings, there are multiple types of output bindings. However not all types support both input and output. You'll use them anytime you want to send or store data. Here, we'll look at the types that support output bindings and when to use them.

## Output binding types

- **Blob Storage** - You can use the blob output binding to write blobs.

- **Azure Cosmos DB** - The Azure Cosmos DB output binding lets you write a new document to an Azure Cosmos DB database using the SQL API.

- **Event Hubs** - Use the Event Hubs output binding to write events to an event stream. You must have send permission to an event hub to write events to it.

- **HTTP** - Use the HTTP output binding to respond to the HTTP request sender. This binding requires an HTTP trigger and allows you to customize the response associated with the trigger's request. This can also be used to connect to web hooks.

- **Microsoft Graph** - Microsoft Graph output bindings allow you to write to files in OneDrive, modify Excel data, and send email through Outlook.

- **Mobile Apps** - The Mobile Apps output binding writes a new record to a Mobile Apps table.

- **Notification Hubs** - You can send push notifications with Notification Hubs output bindings.

- **Queue Storage** - Use the Azure Queue storage output binding to write messages to a queue.

- **Send Grid** - Send emails using SendGrid bindings.

- **Service Bus** - Use Azure Service Bus output binding to send queue or topic messages.

- **Table storage** - Use an Azure Table storage output binding to write to a table in an Azure Storage account.

- **Twilio** - Send text messages with Twilio.

To create a binding as an output, you must define the direction as out. The parameters for each type of binding may vary.

## Combining input and output bindings

It's possible to apply multiple bindings to a single function. This allows you to define both input and output bindings, and the input and output can even be the same binding type.

# Exercise - Write data with output bindings

In our last exercise, we implemented a scenario to look up bookmarks in an Azure Cosmos DB database. We configured an input binding to read data from our bookmarks collection. But, we can do more. Let's expand the scenario to include writing. Consider the following flowchart:

In this scenario, we'll receive requests to add bookmarks to our collection. The requests pass in the desired key, or ID, along with the bookmark URL. As you can see in the flowchart, we'll respond with an error if the key already exists in our back end.

If the key that was passed to us is _not found_, we'll add the new bookmark to our database. We could stop there, but let's do a little more.

Notice another step in the flowchart? So far we haven't done much with the data that we receive in terms of processing. We move what we receive into a database. However, in a real solution, it is possible that we'd probably process the data in some fashion. We can decide to do all processing in the same function, but in this lab we'll show a pattern that offloads further processing to another component or piece of business logic.

What might be a good example of this offloading of work in our bookmarks scenario? Well, what if we send the new bookmark to a QR code generation service? That service would, in turn, generate a QR code for the URL, store the image in blob storage, and add the address of the QR image back into the entry in our bookmarks collection. Calling a service to generate a QR image is time consuming so, rather than wait for the result, we hand it off to a function and let it take care of this asynchronously.

Just as Azure Functions supports input bindings for various integration sources, it also has a set of output bindings templates to make it easy for you to write data to data sources. Output bindings are also configured in the `function.json` file. As you'll see in this exercise, we can configure our function to work with multiple data sources and services.

## Create an HTTP-triggered function

1. Make sure you are signed into the Azure portal using the same account you activated the sandbox with.

2. In the portal, navigate to the function app that you created in this module.

3. Select the Add (+) button next to Functions. This action starts the function creation process.

4. The page shows us the current set of supported triggers. Select HTTP trigger.

5. Select Create to create your function. This action opens the index.js file in the code editor and displays a default implementation of the HTTP-triggered function.

## Add an Azure Cosmos DB input binding

Let's repeat what we did in the preceding unit to add an Azure Cosmos DB input binding.

1. Make sure our new function, HttpTrigger, is selected in the Functions list.

2. Select Integrate in the left pane to open the integration tab.

3. Select Add input in the Inputs column to display the list of all possible input binding types.

4. Select Azure Cosmos DB in the list.

5. If a message appears asking you to install the Microsoft.Azure.WebJobs.Extensions.CosmosDB extension, select install and wait for it to finish.

6. The Azure Cosmos DB account connection field should be pre-populated with the connection you created in the previous exercise. If you do not see your connection listed, use the following steps to create a new connection.
   Click new. This action opens the Connection window, where you will enter the settings for the new connection.

You want to look up a bookmark with a specific ID, so let's tie an ID that we receive in the query string to the binding.

1. In the Document ID (optional) field, enter `{id}`.

   This syntax is known as a binding expression. The function is triggered by an HTTP request that uses a query string to specify the ID to look up. Since IDs are unique in our collection, the binding will return either 0 (not found) or 1 (found) documents.

2. Carefully fill out the remaining fields on this page using the values in the following table. At any time, you can click on the information icon to the right of each field name to learn more about the purpose of each field.

| Setting                  | Value            | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ------------------------ | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Document parameter name  | bookmark         | The name used to identify this binding in your code.                                                                                                                                                                                                                                                                                                                                                                                                                |
| Database name            | func-io-learn-db | The database to work with. This value is the database name we set earlier in this lesson.                                                                                                                                                                                                                                                                                                                                                                           |
| Collection Name          | Bookmarks        | The container from which we'll read data. We defined this setting was earlier in the lesson.                                                                                                                                                                                                                                                                                                                                                                        |
| SQL Query (optional)     | Leave blank      | We are only retrieving one item at a time based on the ID. So, filtering with the Document ID field is better than using a SQL Query in this instance. We could craft a SQL Query to return one entry (SELECT \* from b where b.ID = {id}). That query would indeed return an item, but it would return it in a items collection. Our code would have to manipulate a collection unnecessarily. Use the SQL Query approach when you want to get multiple documents. |
| Partition key (optional) | {id}             | Add the partition key that we defined when we created the Bookmarks Azure Cosmos DB container earlier. The key entered here (specified in input binding format {<key>}) must match the one in the container.                                                                                                                                                                                                                                                        |

3. Select Save to save all changes to this binding configuration.

We now have an Azure Cosmos DB input binding. It's time to add an output binding so we can write new entries to our collection.

## Add an Azure Cosmos DB output binding

1. Make sure our function, add-bookmark, is still selected in the Functions list.

2. Select **Integrate** in the left pane to open the integration tab.

3. Select Add Output in the Outputs column to display the list of all possible output binding types.

4. Select Azure Cosmos DB in the list.

5. The Azure Cosmos DB account connection field should be pre-populated with the connection you created when you added the Azure Cosmos DB input binding.

6. Carefully fill out the remaining fields on this page using the values in the following table. At any time, you can click on the information icon to the right of each field name to learn more about the purpose of each field.

| Setting                          | Value            | Description                                                                                                                                                                                                  |
| -------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Document parameter name          | newbookmark      | The name used to identify this binding in your code. This parameter is used to write a new bookmark entry.                                                                                                   |
| Database name                    | func-io-learn-db | The database to work with. This value is the database name we set earlier in this lesson.                                                                                                                    |
| Collection Name                  | Bookmarks        | The container from which we'll read data. We defined the container earlier in the lesson.                                                                                                                    |
| Partition key (optional)         | {id}             | Add the partition key that we defined when we created the Bookmarks Azure Cosmos DB container earlier. The key entered here (specified in input binding format {<key>}) must match the one in the container. |
| Collection throughput (optional) | Leave blank      | We can accept the default here.                                                                                                                                                                              |

7. Select Save to save all changes to this binding configuration.

Now we have a binding to read from our collection, and one to write to it.

## Add an Azure Queue Storage output binding

Azure Queue storage is a service for storing messages that can be accessed from anywhere in the world. The size of a single message can be as much as 64 KB, and a queue can contain millions of messages - up to the total capacity of the storage account in which it is defined. The following diagram shows at a high level how a queue is used in our scenario:

Here you can see that the new function, add-bookmark, adds messages to a queue. Another function - for example, a fictitious function called `gen-qr-code` - will pop messages from the same queue and process the request. Since we write, or push, messages to the queue from **add-bookmark**, we'll add a new output binding to your solution. Let's create the binding through the portal UI this time.

1. Once again, select Integrate in the left function menu to open the integration tab.

2. Select Add Output in the Outputs column. A list of all possible output binding types is displayed.

3. In the list, select Azure Queue Storage.

   If a message appears asking you to install the Microsoft.Azure.WebJobs.Extensions.Storage extension, select install and wait for it to finish.

Next, we'll set up a storage account connection. This is where our queue will be hosted.

1. Under Storage account connection field, select new. The Storage Account selection pane opens.

2. When we started this module and you created your function app, a storage account was also created at that time. It's listed in this pane, you must select this storage account. The Storage account connection field is populated with the name of a connection.

3. Although we could keep the default values in all the other fields, let's change the following to lend more meaning to the properties:

| Property               | Old value       | New value              | Description                                                                                                    |
| ---------------------- | --------------- | ---------------------- | -------------------------------------------------------------------------------------------------------------- |
| Message parameter name | outputQueueItem | newmessage             | The binding property we'll use in code.                                                                        |
| Queue name             | outqueue        | bookmarks-post-process | The name of the queue where we're placing bookmarks so that they can be processed further by another function. |

4. Remember to select Save to save your changes.

## Update function implementation

We now have all our bindings set up for the add-bookmark function. It's time to use them in our function.

1. Select your function, HttpTrigger, to open the index.js file in the code editor and click Test/Run.

2. Replace all the code in the index.js file with the code from the following snippet and then Save:

```javascript
module.exports = function (context, req) {
  var bookmark = context.bindings.bookmark;
  if (bookmark) {
    context.res = {
      status: 422,
      body: "Bookmark already exists.",
      headers: {
        "Content-Type": "application/json",
      },
    };
  } else {
    // Create a JSON string of our bookmark.
    var bookmarkString = JSON.stringify({
      id: req.body.id,
      url: req.body.url,
    });

    // Write this bookmark to our database.
    context.bindings.newbookmark = bookmarkString;

    // Push this bookmark onto our queue for further processing.
    context.bindings.newmessage = bookmarkString;

    // Tell the user all is well.
    context.res = {
      status: 200,
      body: "bookmark added!",
      headers: {
        "Content-Type": "application/json",
      },
    };
  }
  context.done();
};
```

Let's break down what this code does:

- Because this function changes our data, we expect the HTTP request to be a POST and the bookmark data to be part of the request body.
- Our Azure Cosmos DB input binding attempts to retrieve a document, or bookmark, by using the id that we receive. If it finds an entry, the bookmark object will be set. The if(bookmark) condition checks to see whether an entry was found.
- Adding to the database is as simple as setting the context.bindings.newbookmark binding parameter to the new bookmark entry, which we have created as a JSON string.
- Posting a message to our queue is as simple as setting the context.bindings.newmessage parameter.

So, that's it. Let's see our work in action in the next section.

## Try it out

Now that we have multiple output bindings, testing becomes a little trickier. In previous labs we were content to test by sending an HTTP request and a query string, but we'll want to perform an HTTP post this time. We also need to check to see whether messages are making it into a queue.

1. With our function, add-bookmark, selected in the Function Apps portal, select the Test menu item at the far right to expand it.

2. Select the Test/Run menu item, and verify that you have the test pane open. The following screenshot shows what it should look like:

3. Replace the content of the request body with the following JSON payload:

```Json
{
    "id": "docs",
    "url": "https://docs.microsoft.com/azure"
}
```

4. Select Run at the bottom of the test pane.

5. Verify that the Output window displays the "Bookmark already exists" message, as shown in the following diagram:

6. Replace the request body with the following payload:

```Json
{
    "id": "github",
    "url": "https://www.github.com"
}
```

7. Select Run at the bottom of the test pane.

8. Verify the that Output box displays the "bookmark added" message as shown in the following diagram.

Congratulations! The add-bookmark works as designed, but what about that queue operation we had in the code? Well, let's go see whether something was written to a queue.

## Verify that a message is written to the queue

Azure Queue Storage queues are hosted in a storage account. You already selected the storage account in this exercise when you created the output binding.

1. In the main search box in the Azure portal, type storage accounts, and in the results list, under Services, select Storage accounts.

2. In the list of storage accounts that are returned, select the storage account that you used to create the newmessage output binding. The storage account settings are displayed in the main window of the portal.

3. In the Services list in the middle of the page, select the Queues item. A list of queues hosted by this storage account is displayed. Verify that the bookmarks-post-process queue exists, as shown in the following screenshot:

4. Select bookmarks-post-process to open the queue. The messages that are in the queue are displayed in a list. If all went according to plan, the queue includes the message that you posted when you added a bookmark to the database. It should look like the following:

In this example, you can see that the message was given a unique ID, and the MESSAGE TEXT field displays your bookmark in JSON string format.

5. You can test the function further by changing the request body in the test pane with new id/url sets and running the function. Watch this queue to see more messages arrive. You can also look at the database to verify that new entries have been added.

In this exercise, we expanded your knowledge of bindings to output bindings, writing data to your Azure Cosmos DB. We went further and added another output binding to post messages to an Azure queue. This demonstrates the true power of bindings to help you shape and move data from incoming sources to a variety of destinations. We haven't written any database code or had to manage connection strings ourselves. Instead, we configured bindings declaratively and let the platform take care of securing connections, scaling our function, and scaling our connections.

# Summary

This module was about integrating data and services into your functions. We started with a quick tour of the binding types that show up when you add them to a function, then we looked at reading data from an Azure Cosmos DB by using an input binding. Azure Functions takes care of managing connection strings, and we saw how easy it is to read data in our code by using the binding. Finally, we focused our attention on writing data to different sources with the help of output bindings.

This journey is summarized in the following table, which shows the different bindings that you used in each of the units listed:

| Learning Unit                          | Triggers | Input bindings                | Output bindings                                  |
| -------------------------------------- | -------- | ----------------------------- | ------------------------------------------------ |
| Explore input and output binding types | HTTP     | HTTP                          | HTTP                                             |
| Read data with input bindings          | HTTP     | HTTP <br>Azure Cosmos DB HTTP |
| Write data with output bindings        | HTTP     | HTTP <br> Azure Cosmos DB     | HTTP<br>Azure Cosmos DB <br> Azure Queue Storage |


You can apply the approaches you have learned here to add and test bindings in your functions. Here are a few interesting ideas to get more practice with bindings and to build on what you have learned here.

* Create another function to read from Blob storage and other input bindings that we haven't used in this module.

* Create another function to write to more destinations by using other supported output binding types.

* In the preceding unit, we introduced a queue and posted messages to it with an output binding. As a next step, consider adding another function that reads the messages in the queue and prints out the MESSAGE TEXT to the console with `console.log()`.