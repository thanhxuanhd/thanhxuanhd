# Introduction

Durable Functions is an extension of Azure Functions that enables you to perform long-lasting, stateful operations in Azure. Azure provides the infrastructure for maintaining state information. You can use Durable Functions to orchestrate a long-running workflow. Using this approach, you get all the benefits of a serverless hosting model, while letting the Durable Functions framework take care of activity monitoring, synchronization, and runtime concerns.

Suppose your company has a manual approval process for project design proposals. You want to automate the process, but still involve humans. As a requirement, the workflow solution that you implement needs to be able to orchestrate activities of varying duration and should be cost effective. Since your business also runs on custom business logic, the solution must be flexible enough to run proprietary code.

By the end of this module, you'll learn how to orchestrate a long-running workflow as a set of activities using Durable Functions.

## Learning objectives

In this module, you will:

- Explore Durable Functions
- Design a long-running approval process workflow
- Implement a long-running approval process workflow using Durable Functions

## Prerequisites

- Experience with Azure Functions
- Experience with the Azure portal

# What is Durable Functions?

Durable Functions enables you to implement complex stateful functions in a serverless-environment.

In the example scenario, your company currently follows a manual approval process for project design proposals. The process has multiple steps, and each step in the process can vary in duration. Implementing an automated process in-house is complex and costly. Coordinating each step takes effort. In addition, you must be able to incorporate custom logic into the workflow.

In this unit, you'll learn about the benefits of Durable Functions. You'll learn about the different function types and key concepts associated with Durable Functions.

## Durable functions

Durable Functions is an extension of Azure Functions. Whereas Azure Functions operate in a stateless environment, Durable Functions can retain state between function calls. This approach enables you to simplify complex stateful executions in a serverless-environment.

Durable Functions scales as needed, and provides a cost effective means of implementing complex workflows in the cloud. Some benefits of using Durable Functions include:

- They enable you to write event driven code. A durable function can wait asynchronously for one or more external events, and then perform a series of tasks in response to these events.

- You can chain functions together. You can implement common patterns such as fan-out/fan-in, which uses one function to invoke others in parallel, and then accumulate the results.

- You can orchestrate and coordinate functions, and specify the order in which functions should execute.

- The state is managed for you. You don't have to write your own code to save state information for a long-running function.

Durable functions allows you to define stateful workflows using an orchestration function. An _orchestration function_ provides these extra benefits:

- You can define the workflows in code. You don't need to write a JSON description or use a workflow design tool.

- Functions can be called both synchronously and asynchronously. Output from the called functions is saved locally in variables and used in subsequent function calls.

- Azure checkpoints the progress of a function automatically when the function awaits. Azure may choose to dehydrate the function and save its state while the function waits, to preserve resources and reduce costs. When the function starts running again, Azure will rehydrate it and restore its state.

## Function types

You can use three durable function types: _Client_, _Orchestrator_, and _Activity_.

**Client** functions are the entry point for creating an instance of a Durable Functions orchestration. They can run in response to an event from many sources, such as a new HTTP request arriving, a message being posted to a message queue, an event arriving in an event stream. You can write them in any of the supported languages.

**Orchestrator** functions describe how actions are executed, and the order in which they are run. You write the orchestration logic in code (C# or JavaScript).

**Activity** functions are the basic units of work in a durable function orchestration. An activity function contains the actual work performed by the tasks being orchestrated.

## Application patterns

You can use Durable Functions to implement many common workflow patterns. These patterns include:

- **Function chaining** - In this pattern, the workflow executes a sequence of functions in a specified order. The output of one function is applied to the input of the next function in the sequence. The output of the final function is used to generate a result.

- **Fan out/fan in** - This pattern runs multiple functions in parallel and then waits for all the functions to finish. The results of the parallel executions can be aggregated or used to compute a final result.

- **Async HTTP APIs** - This pattern addresses the problem of coordinating state of long-running operations with external clients. An HTTP call can trigger the long-running action. Then, it can redirect the client to a status endpoint. The client can learn when the operation is finished by polling this endpoint.

- **Monitor** - This pattern implements a recurring process in a workflow, possibly looking for a change in state. For example, you could use this pattern to poll until specific conditions are met.

- **Human interaction** - This pattern combines automated processes that also involve some human interaction. A manual process within an automated process is tricky because people aren't as highly available and as responsive as most computers. Human interaction can be incorporated using timeouts and compensation logic that runs if the human fails to interact correctly within a specified response time. An approval process is an example of a process that involves human interaction.

## Comparison with Logic Apps

Durable Functions and Logic Apps are both Azure services that enable serverless workload. Azure Durable Functions is intended as a powerful serverless compute option to run custom logic. Azure Logic Apps is better suited for integrating Azure services and components. You can use either technology to create complex orchestrations. With Azure Durable Functions, you develop orchestrations by writing code and using the Durable Functions extension. With Logic Apps, you create orchestrations by using the design surface or editing configuration files.

he following table lists some of the key differences between Azure Durable Functions and Azure Logic Apps:

|                   | Azure Durable Functions                                                        | Azure Logic Apps                                                                                          |
| ----------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| Development       | Code-first (imperative)                                                        | Design-first (declarative)                                                                                |
| Connectivity      | About a dozen built-in binding types. You can write code for custom bindings.  | Large collection of connectors. Enterprise Integration Pack for B2B.You can also build custom connectors. |
| Actions           | Each activity is an Azure Function. You write the code for activity functions. | Large collection of ready-made actions. You integrate custom logic through custom connectors.             |
| Monitoring        | Azure Application Insights                                                     | Azure portal, Azure Monitor logs                                                                          |
| Management        | REST API, Visual Studio                                                        | Azure portal, REST API, PowerShell, Visual Studio                                                         |
| Execution context | Can run locally or in the cloud                                                | Runs only in the cloud                                                                                    |

# Design a workflow based on Durable Functions

You can use Durable Functions to orchestrate a long-running workflow as a set of activities. You can map each step in the process to a function type, and each task to an activity. Having an automated process means you don't have to worry about manually monitoring or escalating a task if it isn't done.

As we continue to think about automating our proposal approval process, we need to consider the cases when a step in the process does not complete in time and needs to be escalated. For example: what if we need a manager's approval for a particular size of proposal, but the manager is late with a response?

Escalation steps are useful to the business, as they move along a task when a deadline has been reached. They ensure tasks are completed, and not forgotten. An escalation step could involve sending out reminders or even reassigning a task to someone higher up the managerial hierarchy.

In this unit, you'll design an approval process workflow based on Durable Functions. In the next exercise, you'll apply this knowledge to create an app with Azure Durable Functions.

## Description of the design approval process

Our workflow begins when a project design is submitted for approval. The proposal is assigned as an approval task to a manager. The manager will either approve or reject the proposal. In the real world, this event would probably generate and send a notification to the proposal author, to let them know the outcome of the approve/reject request. In this example, you'll just change the status of the task to either `approved` or `rejected`.

The workflow steps are as follows:

1. A project design is submitted.
2. An approval task is allocated to a manager, so they can review the project design proposal.
3. The project design proposal is rejected or approved.
4. An escalation task is allocated if the approval task isn't completed within a pre-defined time limit.

The following table shows how the workflow steps can be mapped to the function types we use in a Durable Functions workflow.

| Workflow function                                   | Durable Function Type    |
| --------------------------------------------------- | ------------------------ |
| Submitting a project design proposal for approval   | _Client_ Function        |
| Assign an Approval task to relevant member of staff | _Orchestration_ Function |
| Approval task                                       | _Activity_ Function      |
| Escalation task                                     | _Activity_ Function      |

The Orchestration function will manage a rule in the workflow that starts the escalation activity if the approval activity doesn't return within a specified time.

Now that we understand what's needed for our workflow, let's write it in code in the next unit!

# Exercise - Create a workflow using Durable Functions

## Create a Function App

1. Sign in to the Azure portal using the same account that you used to activate the sandbox.

2. On the Azure portal menu or from the Home page, select Create a resource.

3. Select Compute, and then select Function App.

4. Configure the following function app properties on the tabs listed below.

   a. On the Basics tab, specify the following options:

   | Property          | Suggested value                            | Description                                                                                                                                                                                                                                     |
   | ----------------- | ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
   | Subscription      | Concierge subscription                     | Specifies the subscription under which this new function app is created.                                                                                                                                                                        |
   | Resource Group    | learn-2d1f38fe-f4c4-4a22-b667-4eb48c84579e | Specifies the name of the resource group in which to create your function app. We'll create the function app in the sandbox resource group that was assigned when we activated the sandbox, namely, learn-2d1f38fe-f4c4-4a22-b667-4eb48c84579e. |
   | Function App name | [Globally unique name]                     | Specifies the name that identifies your new function app. Valid characters are a-z, 0-9, and -.                                                                                                                                                 |
   | Publish           | Code                                       | Specifies that the function will use code instead of a container.                                                                                                                                                                               |
   | Runtime Stack     | Node.js                                    | Specifies that the sample code in this module is written in JavaScript.                                                                                                                                                                         |
   | Version           | 12 LTS                                     | Specifies the version of the runtime stack.                                                                                                                                                                                                     |
   | Region            | [Select from the list below]               | Choose the region closest to you that is also one of the allowed Sandbox regions listed below.                                                                                                                                                  |

Sandbox regions

The free sandbox allows you to create resources in a subset of the Azure global regions. Select a region from the following list when you create resources:

- West US 2
- South Central US
- Central US
- East US
- West Europe
- Southeast Asia
- Japan East
- Brazil South
- Australia Southeast
- Central India

b. On the Hosting tab, specify the following options:

| Property         | Suggested value          | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| ---------------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Storage account  | [Globally unique name]   | Specifies the name of the new storage account used by your function app (which does not need to match the globally unique name that you specified for your function). Storage account names must be between 3 and 24 characters in length and may contain numbers and lowercase letters only. This dialog automatically populates the field with a unique name that is dynamically generated. However, feel free to use a different name or even an existing account. |
| Operating system | Windows                  | Specifies the operating system that hosts the function app.                                                                                                                                                                                                                                                                                                                                                                                                           |
| Plan type        | Consumption (Serverless) | Specifies the hosting plan that defines how resources are allocated to your function app. In the default Consumption plan, resources are added dynamically as required by your functions. In this serverless hosting model, you only pay for the time your functions run.                                                                                                                                                                                             |

c. On the Monitoring tab, specify the following option:

| Property                    | Suggested value | Description                                                           |
| --------------------------- | --------------- | --------------------------------------------------------------------- |
| Enable Application Insights | No              | Specifies that Application Insights will be disabled for this module. |

5. Click Review + create and review the options that you configured. If you're satisfied with your options, click Create to provision and deploy the function app.

6. Wait for the deployment to complete before continuing. This might take a few minutes.

## Install the durable-functions npm package

Since we are creating JavaScript Durable Functions, we need to install the durable-functions npm package. To do so, use the following steps.

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. Under Development Tools, click App Service Editor.

3. When the App Service Editor opens in a new browser window or tab, highlight the wwwroot folder.

4. Click Open Console on left menu.
   This action starts console. You can use this console to access the web server that hosts your functions, and write the code for your functions.

5. Create a new package.json file:

a. Enter the following commands in the console to create the new JSON file and open it in the editor:

```Bash
touch package.json
open package.json
```

b. Add the following code:

```JSON
{
  "name": "example",
  "version": "1.0.0"
}
```

Where example should be replaced with the name of your package. For example, you could use the globally unique name that you specified for your function earlier.

c. Select Ctrl+S to save the file, then Ctrl+Q to close the document.

6. In the console window, enter the following command:

```Bash
npm install durable-functions
```

This command instructs the node package manager to install the durable-functions package and any dependencies that are required. This may take a few minutes to complete, and the node package manager may display some warnings, which you can ignore.

7. Wait until all packages have finished installing, then switch back to your browser window with the Azure portal.

8. Click Overview, then click Restart, and then click Yes when prompted to restart. Wait for the restart to complete before continuing.

## Create the client function for submitting a design proposal

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. In the Azure portal, under Functions, click Functions, and then click + Add.

3. Select the Durable Functions HTTP starter template. This template creates a durable function that runs in response to an HTTP request.

4. Name the function HttpStart, select Function authorization level, and then click Create Function.

5. When the function is created, click Code + Test, and the code for the index.js file appears in the editor. Your file should resemble the following example:

```JavaScript
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);
    const instanceId = await client.startNew(req.params.functionName, undefined, req.body);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(context.bindingData.req, instanceId);
};
```

6. In the drop-down menu for the files in your function, select function.json to view the bindings associated with your new function. This information specifies any authentication requirements, together with the HTTP methods that can trigger the function. This file also specifies that the function is a client that starts the orchestration process. Your file should resemble the following example:

```JSON
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}",
      "methods": [
        "post",
        "get"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    }
  ]
}
```

## Create the orchestrator function

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. In the Azure portal, under Functions, click Functions, and then click + Add.

3. Select the Durable Functions orchestrator template. This template creates a durable function that orchestrates the execution of functions.

4. Name the new function OrchFunction, and then select Create Function.

5. When the function is created, click Code + Test, and the code for the index.js file appears in the editor. Replace the existing code with the following code:

```Javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function* (context) {
    const outputs = [];

    /*
    * We will call the approval activity with a reject and an approved to simulate both
    */

    outputs.push(yield context.df.callActivity("Approval", "Approved"));
    outputs.push(yield context.df.callActivity("Approval", "Rejected"));

    return outputs;
});
```

This code calls an Activity function named Approval, which you'll create shortly. The code in the orchestrator function invokes the Approval function twice. The first time simulates accepting the proposal, and the second time tests the proposal rejection logic.

The value returned by each call is combined together, and passed back to the client function. In a production environment, your orchestration function would call a series of activity functions that make the accept/reject decision, and return the result of these activities.

6. Select Save to save your new function.

## Create the activity function

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. In the Azure portal, under Functions, click Functions, and then click + Add.

3. Select the Durable Functions activity template. This template creates a durable function that is run when an Activity is called by an orchestrator function.

4. Name the function Approval, and then select Create Function.

5. When the function is created, click Code + Test, and the code for the index.js file appears in the editor. Replace the existing code with the following code:

```Javascript
module.exports = async function (context) {
    return `Your project design proposal has been -  ${context.bindings.name}!`;
};
```

This function returns a message indicating the status of the proposal. The expression context.bindings.name will either be Accepted or Rejected, depending on the parameter passed to the function from the orchestrator. In a real world scenario, you would add the logic that handles the accept or reject operations in this function.

6. Select Save to save your new function.

## Verify that the durable functions workflow starts

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. In the Azure portal, under Functions, click Functions, and then click your HttpStart function.

3. Click Get Function URL, and copy the URL. Your URL should resemble the following example:

```
https://example.azurewebsites.net/api/orchestrators/{functionName}?code=AbCdEfGhIjKlMnOpQrStUvWxYz==
```

You'll use this URL to run your functions.

4. Open a new browser window and navigate to the URL that you copied. In the URL, replace the {functionName} placeholder with OrchFunction, which should resemble the following example:

```
https://example.azurewebsites.net/api/orchestrators/OrchFunction?code=AbCdEfGhIjKlMnOpQrStUvWxYz==
```

The response message contains a set of URI endpoints that you can use to monitor and manage the execution, which should resemble the following example:

```JSON
{
  "id": "f0e1d2c3b4a5968778695a4b3c2d1e0f",
  "statusQueryGetUri": "https://example.azurewebsites.net/...",
  "sendEventPostUri": "https://example.azurewebsites.net/...",
  "terminatePostUri": "https://example.azurewebsites.net/...",
  "rewindPostUri": "https://example.azurewebsites.net/...",
  "purgeHistoryDeleteUri": "https://example.azurewebsites.net/..."
}
```

5.  Copy the statusQueryGetUri value, and use your web browser to navigate to this URL. You should see a response message that resembles the following example:

```JSON
{
  "name": "OrchFunction",
  "instanceId": "f0e1d2c3b4a5968778695a4b3c2d1e0f",
  "runtimeStatus": "Completed",
  "input": null,
  "customStatus": null,
  "output": [
    "Your project design proposal has been -  Approved!",
    "Your project design proposal has been -  Rejected!"
  ],
  "createdTime": "2019-04-16T15:23:03Z",
  "lastUpdatedTime": "2019-04-16T15:23:35Z"
}
```

Recall that the orchestration function runs the activity function twice. The first time, the activity function indicates that the project proposal has been accepted. The second time, the proposal is rejected. The messages from both function calls are combined by the orchestration function and returned to the client function.

# Control long-running tasks using timers

When you are working with a long-running workflow, it is important to consider some additional scenarios. For example, what should happen if a task isn't completed within an acceptable period of time? How can you check the status of a task? You can address these concerns with timeouts and escalation paths.

In the example scenario, you've been asked to amend your new workflow to incorporate an escalation step to take action if a project design proposal isn't approved in a timely fashion.

In this unit you'll learn how to control long running tasks using durable timers, and how to add an escalation path based on the timer.

## Timers in Durable Functions

Durable Functions provides timers for use in the orchestrator functions, which you can use to implement delays or set up timeouts for asynchronous actions. You should use durable timers in orchestrator functions instead of the setTimeout() and setInterval() functions.

You create a durable timer by calling the createTimer() method of the DurableOrchestrationContext. This method returns a task that resumes on a specified date and time.

## Using timers for delay

The following example illustrates how to use durable timers for delay, which sends a reminder every day for 10 days.

```Javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    for (let i = 0; i < 10; i++) {
        const deadline = moment.utc(context.df.currentUtcDateTime).add(i, 'd');
        yield context.df.createTimer(deadline.toDate());
        yield context.df.callActivity("SendReminder");
    }
});
```

You should always use currentUtcDateTime to obtain the current date and time, instead of Date.now or Date.UTC.

## Using timers for timeout

The following example illustrates how to use durable timers for a timeout, which will execute a different path if a timeout occurs. In this example, the function waits until either the GetQuote activity function completes or the deadline timer expires. If the activity function completes, the code follows the success case, otherwise it follows the timeout case.

```Javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("GetQuote");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    }
    else
    {
        // timeout case
        return false;
    }
});
```

In the following exercise, you'll use this information to add an escalation path to our sample scenario in the orchestrator function.

# Exercise - Add a durable timer to manage a long-running task

## Add moment npm package to your function app

Before changing our workflow, we'll add the moment npm package to our function app through the console.

1. Sign in to the Azure portal using the same account that you used to activate the sandbox.

2. On the Azure portal menu or from the Home page, select All resources, and then select the function app that you created in the previous exercise.

3. Under Development Tools, click Console.

4. When the console window opens, verify that you are in the D:\home\site\wwwroot folder, then run the following command:

```Bash
npm install typescript
npm install moment
```

These commands install the typescript and moment libraries. The moment library contains date/time functions that you can use with durable functions, and the typescript library is a dependency. These commands may take a few seconds to complete, and the node package manager may display some warnings that you can ignore.

5. Wait until all packages have finished installing, then close the console window.

## Add an escalation activity to your function app

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. In the Azure portal, under Functions, click Functions, and then click + Add.

3. Select the Durable Functions activity template. This template creates a durable function that is run when an Activity is called by an orchestrator function.

4. Name the function Escalation, and then select Create Function.

5. When the function is created, click Code + Test, and the code for the index.js file appears in the editor. Replace the existing code with the following code:

6. When the function has been created, replace the code in index.js for this function with the following example:

```Javascript
module.exports = async function (context) {
    return `ESCALATION : You have not approved the project design proposal - reassigning to your Manager!  ${context.bindings.name}!`;
};
```

7. Select Save to save your new function.

## Update the orchestration function to use the escalation function

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. In the Azure portal, under Functions, click Functions, and then click your OrchFunction function that you created in the previous exercise.

3. When the function is created, click Code + Test, and the code for the index.js file appears in the editor.

4. Add a reference to the moment library:

```Javascript
const moment = require("moment");
```

5. Replace the body of the function with the following code, which will test whether the deadline for approval has passed:

```Javascript
module.exports = df.orchestrator(function* (context) {
    const outputs = [];
    const deadline = moment.utc(context.df.currentUtcDateTime).add(20, "s");
    const activityTask = context.df.waitForExternalEvent("Approval");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        outputs.push(yield context.df.callActivity("Approval", "Approved"));
    }
    else
    {
        outputs.push(yield context.df.callActivity("Escalation", "Head of department"));
    }

    if (!timeoutTask.isCompleted) {
        // All pending timers must be complete or canceled before the function exits.
        timeoutTask.cancel();
    }

    return outputs;
});
```
To keep things brief for the purposes of this exercise, if the Approval function doesn't respond within 20 seconds, the Escalation function is called. The code also changes the call to Approval to wait for an external input. This way we can control when the response comes back for testing purposes.

6. Select Save.

## Verify that the Durable Functions workflow starts

1. On the Azure portal menu or from the Home page, select All resources, and then select your function app.

2. Under Overview, click Restart, and then click Yes when prompted to restart. Wait for the restart to complete before continuing.

3. Under Functions, click Functions, and then click your HttpStart function.

4. Click Get Function URL, and copy the URL. Your URL should resemble the following example:

5. Open a new browser window and navigate to the URL that you copied. In the URL, replace the {functionName} placeholder with OrchFunction, which should resemble the following example:

```JSON
{
  "id": "f0e1d2c3b4a5968778695a4b3c2d1e0f",
  "statusQueryGetUri": "https://example.azurewebsites.net/...",
  "sendEventPostUri": "https://example.azurewebsites.net/...",
  "terminatePostUri": "https://example.azurewebsites.net/...",
  "rewindPostUri": "https://example.azurewebsites.net/...",
  "purgeHistoryDeleteUri": "https://example.azurewebsites.net/..."
}
```

6. Copy the statusQueryGetUri value, and use your web browser to navigate to this URL. You should see a response message that shows the status as Running while it is waiting for the timer to countdown to 20 seconds, which should resemble the following example:

```JSON
{
  "name": "OrchFunction",
  "instanceId": "f0e1d2c3b4a5968778695a4b3c2d1e0f",
  "runtimeStatus": "Running",
  "input": null,
  "customStatus": null,
  "output": null,
  "createdTime": "2019-04-14T13:17:26Z",
  "lastUpdatedTime": "2019-04-14T13:17:27Z"
}
```

7. If you wait for 20 seconds and refresh the browser window, the timeout should have been reached, and the workflow will call the Escalate activity. You'll see a response that should resemble the following example:


```JSON
{
    "name": "OrchFunction",
    "instanceId": "f0e1d2c3b4a5968778695a4b3c2d1e0f",
    "runtimeStatus": "Completed",
    "input": null,
    "customStatus": null,
    "output": [
        "ESCALATION : You have not approved the project design proposal - reassigning to your Manager!  Head of department!"
    ],
    "createdTime": "2019-04-14T13:43:09Z",
    "lastUpdatedTime": "2019-04-14T13:43:31Z"
}
```