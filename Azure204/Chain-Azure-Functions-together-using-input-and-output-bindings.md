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