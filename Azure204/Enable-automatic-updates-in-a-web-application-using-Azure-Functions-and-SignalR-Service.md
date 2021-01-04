# Introduction

Consider the following scenario. Your company is planning to launch a new website that provides stock price information to customers. Recently, an intern created a website prototype for the new application and the Lead Architect has now asked you to step in and improve the solution.

Your objective is to update the app to implement automatic updates of the stock price information, but ensure communication between the client and server happens only when data changes on the server.

## Learning objectives

In this module, you will:

* Implement a function in Azure Functions that runs only when data changes in an Azure Cosmos DB.
* Implement a function in Azure Functions to broadcast changes to connected clients using SignalR Service.
* Update the client web application to respond to SignalR messages.
* Modify a Vue.js and JavaScript web application to use SignalR

## Prerequisites

* Ability to read JavaScript.
* Familiarity with Azure Functions, Azure Cosmos DB, Azure Storage, and Visual Studio Code.

# Analyze the limitations of a polling-based web app

The application’s current architecture reports stock information by fetching changes from the server based on a timer. This design is often called a polling-based design.

Before we analyze any limitations, let's review the current architecture. The server is responsible for storing stock information and the client renders data in the browser.

We'll set up the current solution on your local machine in the next unit.

## Server 

The stock price information is stored on the server in an Azure Cosmos DB database. When triggered by an HTTP request, the function uses bindings to return content from the database.

The function named `getStocks` is responsible for reading the stock information from the database. As mentioned, the connection to the Azure Cosmos DB database is achieved by using an input binding. This binding is configured in the function.json file, as shown in the following snippet.

```JSON
{
  "bindings": [
    {
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "direction": "in",
      "name": "req",
      "methods": ["get"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    },
    {
      "type": "cosmosDB",
      "direction": "in",
      "name": "stocks",
      "ConnectionStringSetting": "AzureCosmosDBConnectionString",
      "databaseName": "stocksdb",
      "collectionName": "stocks"
    }
  ]
}
```

The first binding (httpTrigger) in the array defines how the function is triggered.

| The configuration...                                      | via the property:        |
| --------------------------------------------------------- | ------------------------ |
| defines the function as an HTTP-triggered function        | `type`                   |
| allows unauthenticated incoming requests                  | `authLevel`, `direction` |
| exposes the request context through a parameter named req | `name`                   |
| accepts GET requests                                      | `methods`                |

The second binding (http) defines what is returned from the function.

| The configuration...                                         | via the property:   |
| ------------------------------------------------------------ | ------------------- |
| allows the function to return an HTTP response               | `type`, `direction` |
| exposes the response context through a parameter named `res` | `name`              |

The third binding (cosmosDB) establishes a connection to Azure Cosmos DB.

| The configuration...                                              | via the property:         |
| ----------------------------------------------------------------- | ------------------------- |
| makes an Azure Cosmos DB data available as the function is called | `type`, `direction`       |
| exposes the data to the function through a parameter named stocks | `name`                    |
| connects to the Azure Cosmos DB data with a connection string     | `ConnectionStringSetting` |
| points to the stocksdb database                                   | `databaseName`            |
| points to the stocks data collection                              | `collectionName`          |

With these bindings, GET requests to `getStocks` make data available through the `stocks` parameter. As you can see in the following code snippet, the function code to retrieve stock information is trivial thanks to the power of Azure Functions bindings.

```JavaScript
module.exports = async function (context, req, stocks) {
    context.res.body = stocks;
};
```

## Client

The sample client uses Vue.js to compose the UI and the axios HTTP client to handle requests to the function.

The page uses a timer to send a request to the server every five seconds to request stocks. The response returns an array of stocks, which are then displayed to the user.

```JavaScript
const LOCAL_BASE_URL = 'http://localhost:7071';

const app = new Vue({
    el: '#app',
    interval: null,
    data() {
        return {
            stocks: []
        }
    },
    methods: {
        async update() {
            try {
                const apiUrl = `${LOCAL_BASE_URL}/api/getStocks`;
                const response = await axios.get(apiUrl);
                app.stocks = response.data;
            } catch (ex) {
                console.error(ex);
            }
        },
        startPoll() {
            this.interval = setInterval(this.update, 5000);
        }
    },
    created() {
        this.update();
        this.startPoll();
    }
});
```

The `update` method is called every five seconds once polling is started by the `startPoll` method. Inside the update method, a GET request is sent to the `getStocks` function and the result is set to `app.stocks` which updates the UI.

The server and client code is relatively straightforward but, as we'll see, this simplicity brings with it some limitations.

## Supporting CORS

In the `local.settings.json` file, the Host section includes the following settings.

```JSON
"Host" : {
    "LocalHttpPort": 7071,
    "CORS": "http://localhost:8080",
    "CORSCredentials": true
  }
```

This configuration allows a web application running at localhost:8080 to make requests to the function app running at localhost:7071. The property CORSCredentials tells function app to accept credential cookies from the request.

CORS is an HTTP feature that enables a web application running under one domain to access resources in another domain. Web browsers implement a security restriction known as same-origin policy that prevents a web page from calling APIs in a different domain; CORS provides a secure way to allow one domain (the origin domain) to call APIs in another domain.

You can set CORS rules individually for each of the Azure Storage services, by calling Set Blob Service Properties, Set File Service Properties, Set Queue Service Properties, and Set Table Service Properties. Once you set the CORS rules for the service, then a properly authorized request made against the service from a different domain will be evaluated to determine whether it is allowed according to the rules you have specified.

## Analysis of current solution

Let's think about some of the drawbacks of this timer-based polling approach.

In the timer-based polling prototype, the client application contacts the server whether or not changes exist to the underlying data. Once data is returned from the server the entire list of stocks is updated on the web page - again - regardless of any changes in the data. This polling mechanism is an inefficient solution.

Selecting the best polling interval for your scenario is also a challenging. Polling forces you to make a choice between how much each call to the backend costs and how quickly you want your app to respond to new data. Delays also often exist between when new data becomes available and when it's detected by the app. The following illustration shows the issue.

In the worst case, the potential delay for detecting new data is equal to the polling interval. So why not use a smaller interval?

As the application scales, the amount of data exchanged between the client and server will become a problem. Each HTTP request header includes hundreds of bytes of data along with the session's cookie. All this overhead, especially when under heavy load, creates wasted resources and unnecessarily taxes the server.

Now that you're more familiar with the starting point of the application, it's time to get the application running on your machine.

# Exercise - Analyze the limitations of a polling-based web app

## Download sample app code

1. Run the following command on your local machine to clone the app from GitHub.

```Bash
git clone https://github.com/MicrosoftDocs/mslearn-advocates.azure-functions-and-signalr.git serverless-demo
```
>Do not run `npm install` until you have completed the steps that update the local.settings.json. A post-install script is included to set up your database and add some data to the database.

2. Run the following command to navigate to the new folder into which you cloned the repo:

```Bash
cd serverless-demo
```

3. The beginning state of the app is located in the start folder. Make sure you are in that folder for the rest of this module. Use the following command to open the start folder in Visual Studio Code:

```Bash
code start
```

For your reference, the finished solution is available in the folder called end.

## Create a Storage account

Azure Functions requires a storage account, and you'll need it when you deploy the web app to the cloud later in the module.

1. Execute the following command in the Cloud Shell to define a name for your Azure Storage account.

```Bash
export STORAGE_ACCOUNT_NAME=mslsigrstorage$(openssl rand -hex 5)
echo "Storage Account Name: $STORAGE_ACCOUNT_NAME"
```

Keep note of this account name for the remainder of the module.

2. Run the following `az storage account create` command in the Cloud Shell to create a storage account for your function and static website.

```Bash
az storage account create \
  --name $STORAGE_ACCOUNT_NAME \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
  --kind StorageV2 \
  --sku Standard_LRS
```

It can take a few minutes to create a storage account. Wait for this step to finish before proceeding.

## Create an Azure Cosmos DB account

You store stock prices in an Azure Cosmos DB database, so you'll set that up in the sandbox account.

1. Run the following `az cosmosdb create` command in the Cloud Shell to create a new Azure Cosmos DB account in your sandbox resource group.

```Bash
az cosmosdb create  \
  --name msl-sigr-cosmos-$(openssl rand -hex 5) \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81
```

It can take a few minutes to create an Azure Cosmos DB account. Wait for this step to finish before proceeding.

## Update local settings
For the app to run, you need to add the connection settings for your cloud services to the local settings file.

1. Run the following commands in the Cloud Shell to get the connection strings for the resources we created in this exercise.

```Bash
STORAGE_CONNECTION_STRING=$(az storage account show-connection-string \
--name $(az storage account list \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
  --query [0].name -o tsv) \
--resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
--query "connectionString" -o tsv)

COSMOSDB_ACCOUNT_NAME=$(az cosmosdb list \
    --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
    --query [0].name -o tsv)

COSMOSDB_CONNECTION_STRING=$(az cosmosdb list-connection-strings  \
  --name $COSMOSDB_ACCOUNT_NAME \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
  --query "connectionStrings[?description=='Primary SQL Connection String'].connectionString" -o tsv)

COSMOSDB_MASTER_KEY=$(az cosmosdb list-keys \
--name $COSMOSDB_ACCOUNT_NAME \
--resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
--query primaryMasterKey -o tsv)

printf "\n\nReplace <STORAGE_CONNECTION_STRING> with:\n$STORAGE_CONNECTION_STRING\n\nReplace <COSMOSDB_CONNECTION_STRING> with:\n$COSMOSDB_CONNECTION_STRING\n\nReplace <COSMOSDB_MASTER_KEY> with:\n$COSMOSDB_MASTER_KEY\n\n"
```

2. Navigate to where you cloned the application and open the start folder in Visual Studio Code. Open local.settings.json in the editor so you can update the file.

3. In `local.settings.json`, update the variables `AzureWebJobsStorage`, AzureCosmosDBConnectionString, and AzureCosmosDBMasterKey with the values listed in the Cloud Shell and save the file. The local.settings.json file should only exist on your local computer.

## Run the application
1. In the Visual Studio Code terminal window, run the following command to install dependencies and set up the database

```Bash
npm install
```
> If a problem arises during the install process and the database is not correctly setup, you can run `npm run setup` to manually seed the database.

2. Press F5 to start debugging the function app. The function app startup is shown in a terminal window..
   
3. To run the web application on your machine, open a second integrated terminal instance and run the following command to start the web app.

```Bash
npm start
```

4. The script automatically opens the browser and navigates to http://localhost:8080. If the browser fails to open automatically, you can navigate to http://localhost:8080 manually.
5. Return to Visual Studio Code, open a third terminal instance, and enter the following command to update the stock prices. Immediately return to the browser and observe that the values for stock ABC change within a few seconds.

```Bash
npm run update-data
```

When you're done, stop the running processes.

- To stop the web server, click the kill process (trash can icon) on the terminal window that is running the web server.

- To stop the functions app, click the Stop button or press Shift + F5.

# Enable automatic updates in a web application using SignalR Service

Next, we'll move away from polling and toward an app that pushes data updates (as they occur) to connected clients. This new design reduces traffic and makes a more efficient UI by only updating as data changes. The three technologies that we'll use to deliver this updated solution are **Azure Cosmos DB**, **Azure Functions**, and **SignalR**.

As data changes in the database, Azure Cosmos DB exposes a "change feed". The change feed support in Azure Cosmos DB works by listening to a database container for changes. It then outputs the sorted list of changed documents in the order in which they were modified. By listening to the change feed, your application can automatically respond to data changes.

As data changes in the database, Azure Cosmos DB exposes a "change feed". The change feed support in Azure Cosmos DB works by listening to a database container for changes. It then outputs the sorted list of changed documents in the order in which they were modified. By listening to the change feed, your application can automatically respond to data changes.

The key difference between this function and the original `getStocks` function is that the function is now triggered based on changes to our data. In the preceding exercise, we triggered our function based on requests from the client and pulled back all data through an Azure Cosmos DB input binding. Using the Azure Cosmos DB trigger automatically makes our data retrieval more efficient

Azure Functions features a binding that runs code anytime data is updated in an Azure Cosmos DB change feed. Once a function is listening to the change feed, then you can work with a subset of your data that just represents data changes. When paired with a persistent connection to the client, the function can contact individual clients on-demand, which is the foundation for a real-time application architecture.

## SignalR and persistent connections

In contrast to polling, a more favorable design features persistent connections between the client and server. Establishing a persistent connection allows the server to push data to the client at will. The on-demand nature of the connection reduces network traffic and load on the server. SignalR allows you to easily add this type of architecture to your application.

**SignalR** is an abstraction for a series of technologies that allows your app to enjoy two-way communication between the client and server. SignalR handles connection management automatically, and lets you broadcast messages to all connected clients simultaneously, like a chat room. You can also send messages to specific clients. The connection between the client and server is persistent, unlike a classic HTTP connection, which is re-established for each communication.

A key benefit of the abstraction provided by SignalR is the way it supports "transport" fallbacks. A transport is method of communicating between the client and server. SignalR connections begin with a standard HTTP request. As the server evaluates the connection, the most appropriate communication method (transport) is selected. Transports are chosen depending on the APIs available on the client.

For clients that support HTML 5, the WebSockets API transport is used by default. If the client doesn't support WebSockets, then SignalR falls back to Server Sent Events (also known as EventSource). For older clients, Ajax long polling or Forever Frame (IE only) is used to mimic a two-way connection.

For clients that support HTML 5, the WebSockets API transport is used by default. If the client doesn't support WebSockets, then SignalR falls back to Server Sent Events (also known as EventSource). For older clients, Ajax long polling or Forever Frame (IE only) is used to mimic a two-way connection.

The second benefit is that SignalR allows your application to gracefully degrade depending on supported technologies of the client. If it doesn't support WebSockets, then Server Sent Events are used. If the client can't handle Server Sent Events, then it uses Ajax long polling, and so on.

Let's look at how to use SignalR to broadcast information from function that reads the Azure Cosmos DB change feed.

#  Exercise – Enable automatic updates in a web application using SignalR Service

## Create a SignalR account

You'll need to add a SignalR account to your sandbox subscription.

1. The first step is to run the following command in the Cloud Shell to create a new SignalR account in the sandbox resource group. This command can take a couple of minutes to complete, so please wait for it to finish before proceeding to the next step.

```Bash
SIGNALR_SERVICE_NAME=msl-sigr-signalr$(openssl rand -hex 5)
az signalr create \
  --name $SIGNALR_SERVICE_NAME \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
  --sku Free_DS2 \
  --unit-count 1
```
2. For SignalR Service to work properly with Azure Functions, you need to set its service mode to Serverless. Configure the service mode using the following command.

```Bash
az resource update \
  --resource-type Microsoft.SignalRService/SignalR \
  --name $SIGNALR_SERVICE_NAME \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
  --set properties.features[flag=ServiceMode].value=Serverless
```

## Update local settings

For the app to run, you need to add the SignalR connection string saved to your local settings.

1. Run the following commands in the Cloud Shell to get the connection strings for the resources we created in this exercise.

```Bash
SIGNALR_CONNECTION_STRING=$(az signalr key list \
  --name $(az signalr list \
    --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
    --query [0].name -o tsv) \
  --resource-group learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81 \
  --query primaryConnectionString -o tsv)

printf "\n\nReplace <SIGNALR_CONNECTION_STRING> with:\n$SIGNALR_CONNECTION_STRING\n\n"
```

2. Navigate to where you cloned the application and open the start folder in Visual Studio Code. Open local.settings.json in the editor so you can update the file.
3. In local.settings.json, update the variable AzureSignalRConnectionString with the value listed in the Cloud Shell and save the file.

## Manage client connections

The web client uses the SignalR client SDK to establish a connection to the server. The SDK retrieves the connection via a function named negotiate (by convention) to connect to the service.

1. Open the Visual Studio Code command palette by pressing F1.
2. Search for and select the Azure Functions: Create Function command.
3. When prompted, provide the following information.

| Name          | Value           |
| ------------- | --------------- |
| Template      | HTTP Trigger    |
| Name          | negotiate       |
| Authorization | level	Anonymous |

Refresh the Explorer window in Visual Studio Code to see the updates. A folder named negotiate is now available in your function app.

4. Open negotiate/function.json and add the following SignalR binding definition to the bindings array.

```JSON
{
    "type": "signalRConnectionInfo",
    "name": "connectionInfo",
    "hubName": "stocks",
    "direction": "in",
    "connectionStringSetting": "AzureSignalRConnectionString"
}
```

This configuration allows the function to return the connection information to the server, which is used to identify connected clients.

5. Next, open negotiate/index.js and replace the existing function code with the following code.

```JavaScript
module.exports = async function (context, req, connectionInfo) {
    context.res.body = connectionInfo;
};
```

As the function is called, the SignalR connection is returned as the response to the function.

Now that the function to return the SignalR connection info is implemented, you can create a function responsible for pushing changes to the client.

## Detect and broadcast database changes

First, you need to create a new function that listens for changes in the database. This function uses an Azure Cosmos DB trigger that connects to the change feed of the database.

1. Open the Visual Studio Code command palette by pressing F1.
2. Search for and select the Azure Functions: Create Function command.
3. When prompted, provide the following information.

| Name                                         | Value                         |
| -------------------------------------------- | ----------------------------- |
| Template                                     | Azure Cosmos DB Trigger       |
| Name                                         | stocksChanged                 |
| App setting for your Azure Cosmos DB account | AzureCosmosDBConnectionString |
| Database name                                | stocksdb                      |
| Collection name                              | stocks                        |
| ollection name for leases                    | leases                        |
| Create lease collection if not exists        | true                          |

4. Open stocksChanged/function.json in Visual Studio Code.
5. Append the property `"feedPollDelay": 500` to the existing trigger binding definition. This setting tells Azure Cosmos DB how long to wait before checking for changes in the database. The application you're building is built around a push-based architecture. However behind the scenes, Azure Cosmos DB is continually monitoring the change feed to detect changes. The `feedPollDelay` refers to how the internals of Azure Cosmos DB recognize changes, not how your web application exposes changes to the data.

The Azure Cosmos DB binding for your function should now look like the following code.

```JSON
{
  "type": "cosmosDBTrigger",
  "name": "documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "AzureCosmosDBConnectionString",
  "databaseName": "stocksdb",
  "collectionName": "stocks",
  "createLeaseCollectionIfNotExists": "true",
  "feedPollDelay": 500
}
```

6. Next, append the following SignalR output binding definition to the bindings array.

```JSON
{
  "type": "signalR",
  "name": "signalRMessages",
  "connectionString": "AzureSignalRConnectionString",
  "hubName": "stocks",
  "direction": "out"
}
```

This binding allows the function to broadcast changes to clients.

7. Update the stocksChanged/index.js file to reflect the following code. The beauty of all the configuration is that the function code is simple.

```JavaScript
module.exports = async function (context, documents) {
    const updates = documents.map(stock => ({
        target: 'updated',
        arguments: [stock]
    }));

    context.bindings.signalRMessages = updates;
    context.done();
}
```

An array of changes is prepared by creating an object formatted to be read by SignalR. Every updated stock is provided to the arguments array along with a target property set to updated.

The value of the target property is used on the client when listening for specific messages broadcast by SignalR.

## Update the web application
Open public/index.html and paste the following code in place of the current DIV with the ID of app.

```HTML
<div id="app" class="container">
    <h1 class="title">Stocks</h1>
    <div id="stocks">
        <div v-for="stock in stocks" class="stock">
            <transition name="fade" mode="out-in">
                <div class="list-item" :key="stock.price">
                    <div class="lead">{{ stock.symbol }}: ${{ stock.price }}</div>
                    <div class="change">Change:
                        <span
                            :class="{ 'is-up': stock.changeDirection === '+', 'is-down': stock.changeDirection === '-' }">
                            {{ stock.changeDirection }}{{ stock.change }}
                        </span>
                    </div>
                </div>
            </transition>
        </div>
    </div>
</div>
```

This markup adds a transition element, which allows Vue.js to run a subtle animation as stock data changes. When a stock is updated, the tile fades out and quickly back in to view. This way if the page is full of stock data, users can easily see which stocks have changed.

Next, add the following script block just above the reference to index.html.js.

```HTML
<script src="https://cdn.jsdelivr.net/npm/@aspnet/signalr@1.1.0/dist/browser/signalr.js"></script>
```

This script adds a reference to the SignalR SDK.

Now open public/index.html.js and replace the file with the following code.

```JavaScript
const LOCAL_BASE_URL = 'http://localhost:7071';
const REMOTE_BASE_URL = '<FUNCTION_APP_ENDPOINT>';

const getAPIBaseUrl = () => {
    const isLocal = /localhost/.test(window.location.href);
    return isLocal ? LOCAL_BASE_URL : REMOTE_BASE_URL;
}

const app = new Vue({
    el: '#app',
    data() {
        return {
            stocks: []
        }
    },
    methods: {
        async getStocks() {
            try {
                const apiUrl = `${getAPIBaseUrl()}/api/getStocks`;
                const response = await axios.get(apiUrl);
                app.stocks = response.data;
            } catch (ex) {
                console.error(ex);
            }
        }
    },
    created() {
        this.getStocks();
    }
});

const connect = () => {
    const connection = new signalR.HubConnectionBuilder()
                            .withUrl(`${getAPIBaseUrl()}/api`)
                            .build();

    connection.onclose(()  => {
        console.log('SignalR connection disconnected');
        setTimeout(() => connect(), 2000);
    });

    connection.on('updated', updatedStock => {
        const index = app.stocks.findIndex(s => s.id === updatedStock.id);
        app.stocks.splice(index, 1, updatedStock);
    });

    connection.start().then(() => {
        console.log("SignalR connection established");
    });
};

connect();
```

The changes you just made accomplished two goals: removed all polling logic from the client and added handlers to listen for messages coming from the server.

A new helper function is introduced which makes it easy for the application to work in local and deployed contexts.

```Javascript
const LOCAL_BASE_URL = 'http://localhost:7071';
const REMOTE_BASE_URL = '<FUNCTION_APP_ENDPOINT>';

const getAPIBaseUrl = () => {
    const isLocal = /localhost/.test(window.location.href);
    return isLocal ? LOCAL_BASE_URL : REMOTE_BASE_URL;
}
```

The `getAPIBaseUrl` function returns the appropriate URL depending on whether the app is running locally or deployed to Azure. The placeholder `<REMOTE_BASE_URL>` is replaced by the storage account endpoint in a coming exercise when you deploy this application to the cloud.

The Vue.js-related code is streamlined now that changes are pushed to the client. Consider this segment of the code you pasted in to the script file:

```Javascript
const app = new Vue({
    el: '#app',
    data() {
        return {
            stocks: []
        }
    },
    methods: {
        async getStocks() {
            try {
                const apiUrl = `${getAPIBaseUrl()}/api/getStocks`;
                const response = await axios.get(apiUrl);
                app.stocks = response.data;
            } catch (ex) {
                console.error(ex);
            }
        }
    },
    created() {
        this.getStocks();
    },
});
```

The same stocks array is used here as in the previous implementation, but all the polling code is removed and the logic for `getStocks` remains unchanged. The `getStocks` function is still called as the component is created.

Next, consider this segment of the client code:

```Javascript
const connect = () => {
    const connection = new signalR.HubConnectionBuilder()
                            .withUrl(`${getAPIBaseUrl()}/api`)
                            .build();

    connection.onclose(()  => {
        console.log('SignalR connection disconnected');
        setTimeout(() => connect(), 2000);
    });

    connection.on('updated', updatedStock => {
        const index = app.stocks.findIndex(s => s.id === updatedStock.id);
        app.stocks.splice(index, 1, updatedStock);
    });

    connection.start().then(() => {
        console.log("SignalR connection established");
    });
};

connect();
```

When the page loads, the `connect` function is called. In the body of the `connect` function, the first action is to use the SignalR SDK to create a connection by calling `HubConnectionBuilder`. The result is a SignalR connection to the server.

To gracefully recover after the server has timed out, the `onclose` handler reestablishes a connection two seconds after the connection has closed by calling `connect` again.

As the client receives messages from the server, it listens for messages via the on('updated',... syntax. Once an update is received, the following actions take place:

- The changed stock is located in the array.
- The old version is removed.
- The new version is inserted at the same index position in the array.
Manipulating the array this way allows Vue to detect changes in the data and trigger animation effects to notify users of changes.

## Run the application

Now you can see the new version of the app running locally.

Press F5 to start debugging the functions app.

Next, open a new terminal window in Visual Studio Code and run npm start:

```Bash
npm start
```
The script automatically opens the browser and navigates to http://localhost:8080. If the browser fails to open automatically, you can navigate to http://localhost:8080 manually.

## Observe automatic updates
Now you can change the application's data and observe how to the UI automatically updates.

1. Arrange Visual Studio Code on one side of the screen and the web browser on the other. This way you can see the UI update as changes are made to the database.

2. Return to Visual Studio Code and enter the following command in a new integrated terminal. Again, watch as the application automatically updates the stock ABC.
```Bash
npm run update-data
```

When you're done, stop the running processes:

To stop the web server, click the kill process (trash can icon) on the terminal window that is running the web server.

To stop the functions app, click the Stop button or press Shift + F5.

# Use a storage account to host a static website

Now that we've tested the application on your local machine, it's time to publish it to the cloud. There are two aspects of your application that require attention before publishing. First, you need to deploy the local functions into Azure and then you need to make the static HTML and JavaScript files available on the web.

Azure Storage includes a feature where you can place files in a specific storage container, which makes them available for HTTP requests. This feature, known as static website support makes hosting publicly available web pages a simple process.

When you copy files to a storage container named `$web`, those files are available to web browsers via a secure server using the `https://<ACCOUNT_NAME>.<ZONE_NAME>.web.core.windows.net/<FILE_NAME>` URI scheme.

#  Exercise - Use a storage account to host a static website

Customize Visual Studio Code

Before you begin, there are two changes you need to make to Visual Studio Code.

The first change grants Visual Studio Code access to the Azure subscription used by the sandbox. This subscription was created when you activate the sandbox and allows you to use Azure services without incurring any costs.

The second customization tells the Azure Functions extension to use the advanced creation process. If you skip this step, the extension uses default values and won't create your function to work with the sandbox.

## Add concierge tenant to Visual Studio Code

The following steps associate the free Azure subscription created for you with Visual Studio Code. At the end of the tutorial you'll follow steps to restore Visual Studio Code back to its original settings.

1. In the Cloud Shell, run the following command and copy the tenant ID to your clipboard.

```Bash
az account list --query "[?name=='Concierge Subscription'].tenantId" -o tsv
```

2. Open settings in Visual Studio Code. On Windows or Linux, select File > Preferences > Settings. On macOS, select Code > Preferences > Settings.

3. Navigate through User Settings > Extensions > Azure configuration

4. Enter the tenant in the Azure: Tenant textbox.

# Sign out and back in


1. Press F1 to open the Visual Studio Code command palette.

2. Search for and select Azure: Sign Out.

3. Press F1 again.

4. Search for and select Azure: Sign In and sign in with the same account you used to sign into the Learn sandbox.

## Select subscription
1. Click on the Azure extension icon.

2. Under the Functions heading, click on Select Subscriptions.

3. Next, the command palette appears. Select Concierge Subscription and click OK.

Now Visual Studio Code is configured to use the sandbox resources and avoid any billing against your account.

## Deploy the function app

1. Press F1 to open the Visual Studio Code command palette.

2. Search for and select the Azure Functions: Deploy to Function App command.

3. Follow the prompts to provide the following information.

| Name                                                                                  | Value                                               |
| ------------------------------------------------------------------------------------- | --------------------------------------------------- |
| unction app                                                                           | Select Create new Function App in Azure... Advanced |
| Function app name	Enter a globally unique name. Valid characters are a-z, 0-9, and -. |
| OS                                                                                    | Select Windows                                      |
| Plan                                                                                  | Select Consumption                                  |
| Language                                                                              | Select JavaScript                                   |
| Resource group                                                                        | Select learn-cd2a1e59-7618-4e7a-8a10-1d01e2d2de81   |
| Storage account                                                                       | Select the account you created earlier              |
| Application Insights resource                                                         | Select Skip for now                                 |

A new function app is created in Azure and the deployment begins. The Azure Functions Visual Studio Code extension first creates the Azure resources and then deploys the function app.

Once complete, the Azure Functions extension reports the primary endpoint of the function in a message box as shown by this screenshot.

The functions app name (labeled as 1 in the image) is the unique name you provided as you created the app. The app end point (labeled as 2) is the function app name followed by azurewebsites.net.

4. Open `public/index.html.js` and replace `<FUNCTION_APP_ENDPOINT>` with the function's endpoint.

5. Next, upload your local settings to Azure by opening the command palette via `F1` and select `Azure Functions: Upload local settings.` When prompted, choose the function app you just created, and choose to overwrite all settings.

## Configure static websites in Azure Storage
Use the following steps to configure the Azure Storage account to host a static website.

1. Open the Visual Studio Code command palette via F1.

2. Search for and select the Azure Storage: Configure Static Website command.

| Name            | Value                                                         |
| --------------- | ------------------------------------------------------------- |
| Storage account | Select the account you created earlier.                       |
| Default file    | Select index.html as the index document name for the account. |
| Error document  | Enter index.html for the default 404 error document path.     |

The error document path is the page the browser will load when a routing error occurs. This is important for JavaScript frameworks like Vue.js which have client-side routing.

## Deploy the web application to Azure Storage

1. Open the Visual Studio Code command palette via F1.

2. Search for and select the Azure Storage: Deploy to Static Website command.

| Name            | Value                                                                 |
| --------------- | --------------------------------------------------------------------- |
| Storage account | Select the Storage account you created earlier.                       |
| Select folder   | Select browse and choose the public subfolder containing the web app. |

After the extension is done deploying your application, a notification appears that the upload was successful. The upload can take several minutes.

## Determine the primary endpoint address of the static website

1. In the command palette, search for Azure Storage: Browse static website and choose your Storage account. The site opens in the browser. At this point, the app won't run because of CORS requirements of Azure Functions.

2. Copy the URL in the browser, which is the endpoint of the static site hosted in your Storage account. You use the endpoint value to set up CORS settings for the function app in the next section.

Keep this browser window open. You will return refresh this window once the CORS settings are updated in your function app.

## Set up CORS in the function app

1. In the command palette, search for and select the Azure Functions: Open in portal command.

2. Select the function app name.

3. Once the portal is open in the browser, select the Platform features tab and under API select CORS.

4. Check the checkbox next to Enable Access-Control-Allow-Credentials.

5. Add an entry with the static website primary endpoint as the value (make sure to remove the trailing /). You can paste this value in from your clipboard.

6. Click Save to persist the CORS settings.

## Run the deployed application

Now you can make change to the application's data and observe how to the data is automatically updated.

1. Arrange Visual Studio Code on one side of the screen and the web browser running the static site on the other. This way you can see the UI update as changes are made to the database.

2. Refresh the browser. It may take a moment for stocks to appear as the serverless functions are running for the first time.

3. In Visual Studio integrated terminal, enter the following command and watch as the UI is automatically updated.

```Bash
npm run update-data
```