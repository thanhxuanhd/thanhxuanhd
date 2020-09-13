### Introduction
Azure is a cloud platform that provides the compute, storage, and networking resources needed to build cloud-hosted applications. As a new user, the Azure portal is likely to be the primary way you will interact with Azure. The Azure portal lets you create and manage all your Azure resources. For example, you can set up a new database, increase the compute power of your virtual machines, and monitor your monthly costs. It's also a great learning tool since you can survey all available resources and use guided wizards to create the ones you need.

Here you will learn how to sign in to the portal and navigate the portal interface. You will also learn how to customize the dashboard, so it is convenient to locate and monitor your most essential services.

### Azure management options

You can configure and manage Azure using a broad range of tools and platforms. There are tools available for the command line, language-specific Software Development Kits (SDKs), developer tools, tools for migration, and many others.

Tools that are commonly used for day-to-day management and interaction include:
* __Azure portal__ for interacting with Azure via a Graphical User Interface (GUI)
* __Azure PowerShell__ and __Azure Command-Line Interface__ (CLI) for command line and automation-based interactions with Azure
* __Azure Cloud Shell__ for a web-based command-line interface
* __Azure mobile app__ for monitoring and managing your resources from your mobile device

#### Azure portal

The Azure portal is a public website that you can access with any web browser. Once you sign in with your Azure account, you can create, manage, and monitor any available Azure services. You can identify a service you're looking for, get links for help on a topic, and deploy, manage, and delete resources. It also guides you through complex administrative tasks using wizards and tooltips.

The dashboard view provides high-level details about your Azure environment. You can customize the dashboard by moving and resizing tiles, and displaying services you're interested in.

The portal doesn't provide any way to automate repetitive tasks. For example, to set up multiple VMs, you would need to create them one at a time by completing the wizard for each VM. This process makes the portal approach time-consuming and error-prone for complex tasks.

#### Azure PowerShell
Azure PowerShell is a module that you can install for Windows PowerShell or PowerShell Core, which is a cross-platform version of PowerShell that runs on Windows, Linux, or macOS. Azure PowerShell enables you to connect to your Azure subscription and manage resources. Windows PowerShell and PowerShell Core provide services such as the shell window and command parsing. Azure PowerShell then adds the Azure-specific commands.

For example, Azure PowerShell provides the `New-AzVM` command that creates a virtual machine for you inside your Azure subscription. To use it, you would launch PowerShell, install the Azure PowerShell module, sign in to your Azure account using the command `Connect-AzAccount`, and then issue a command such as:


```PowerShell
New-AzVM `
    -ResourceGroupName "MyResourceGroup" `
    -Name "TestVm" `
    -Image "UbuntuLTS" `
    ...
```

Creating administration scripts and using automation tools is a powerful way to optimize your workflow. You can automate repetitive tasks. Once a script is verified, it runs consistently, which can reduce errors. Another scripting environment is the Azure CLI.

#### Azure CLI

Azure CLI is a cross-platform command-line program that connects to Azure and executes administrative commands on Azure resources. _Cross-platform_ means that it can be run on Windows, Linux, or macOS. For example, to create a VM, you would open a command prompt window, sign in to Azure using the command `az login`, create a resource group, then use a command such as:

```AzureCLI
    az vm create \
    --resource-group MyResourceGroup \
    --name TestVm \
    --image UbuntuLTS \
    --generate-ssh-keys \
    ...
```

#### Azure Cloud Shell
[Azure Cloud Shell](https://shell.azure.com/) is an interactive, authenticated, browser-accessible shell for managing Azure resources. It provides the flexibility of choosing the shell experience that best suits the way you work, either Bash or PowerShell.

You can switch between the two shells, and both support the Azure CLI and Azure PowerShell module. Bash defaults to the Azure CLI (with the `az` command pre-installed), but you can switch to PowerShell Core within Linux by typing `pwsh`. The PowerShell environment has both CLI tools pre-installed. In addition to these administrative tools, the Cloud Shell has a suite of developer tools, text editors, and other tools available, including:

| __Developer Tools__ |  __Editors__ | __Other tools__ |
| ------------------- | ------------ | --------------- |
|    .NET Core <br> Python <br> Java <br> Node.js <br>Go | code (Cloud Shell Editor) <br> vim<br> nano<br> emacs| git<br>maven<br>make<br>npm<br>and more...

You can create, build, and deploy apps right from this browser-based environment. It's all persistent as well - you're prompted to create an Azure Storage Account when you access the Azure Cloud Shell. This storage area is used as your $HOME folder and any scripts or data you place here is kept across sessions. Each subscription has a unique storage account associated with it, so you can keep the data and tools you need specific to each account you manage.

We'll use the Cloud Shell in Microsoft Learn for many of the interactive exercises to try out Azure features.

#### Azure mobile app

The [Microsoft Azure mobile app](https://azure.microsoft.com/en-us/features/azure-portal/mobile-app/) allows you to access, manage, and monitor all your Azure accounts and resources from your iOS or Android phone or tablet. Once installed, you can:

* Check the current status and important metrics of your services
* Stay informed with notifications and alerts about important health issues
* Quickly diagnose and fix issues anytime, anywhere
* Review the latest Azure alerts
* Start, stop, and restart virtual machines or web apps
* Connect to your virtual machines
* Manage permissions with role-based access control (RBAC)
* Use the Azure Cloud Shell to run saved scripts or perform ad hoc administrative tasks
* and more...

#### Other options
There are also Azure SDKs for a range of languages and frameworks, and REST APIs that you can use to manage and control Azure resources programmatically. For a full list of tools available, see the [Downloads](https://azure.microsoft.com/en-us/downloads/) page.

When starting with Azure, you'll most often use the Azure portal. Let's take a closer look at the portal approach.

### Navigate the portal

With an Azure account, we can sign into the __Azure portal__. The portal is a web-based administration site that lets you interact with all of your subscriptions and resources you have created. Almost everything you do with Azure can be done through this web interface.

#### Azure portal layout

The Azure portal is the primary graphical user interface (GUI) for controlling Microsoft Azure. You can carry out the majority of management actions in the portal, and it is typically the best interface for carrying out single tasks or where you want to look at the configuration options in detail.

#### Resource panel

In the left-hand sidebar of the portal is the resource panel, which lists the main resource types. Note that Azure has more resource types than just those shown. The resources listed are part of your _favorites_.

You can customize this with the specific resource types you tend to create or administer most often

The remainder of the portal view is for the specific elements you are working with. The default (main) page is Home but you can change your default view to the customizable Dashboard from Settings. We'll cover settings later in this unit.

#### What is the Azure Marketplace?

The _Azure Marketplace_ is often where you start when creating new resources in Azure. The Marketplace allows customers to find, try, purchase, and provision applications and services from hundreds of leading service providers, all certified to run on Azure.

The solution catalog spans several industry categories, including but not limited to open-source container platforms, virtual machine images, databases, application build and deployment software, developer tools, threat detection, and blockchain. Using Azure Marketplace, you can provision end-to-end solutions quickly and reliably, hosted in your own Azure environment. At the time of writing, this listing included over 8,000 listings.

While Azure Marketplace is designed for IT professionals and cloud developers interested in commercial and IT software, Microsoft Partners also use it as a launch point for all joint Go-To-Market activities.

#### Configuring settings in the Azure portal

The Azure portal displays several configuration options, mostly in the status bar at the top-right of the screen.

If you are viewing the Azure portal on a screen with reduced horizontal space, the following icons may be made available through an ellipsis (...) menu.

#### Cloud Shell

If you select the Cloud Shell icon (>_), you create a new Azure Cloud Shell session. Recall that Azure Cloud Shell is an interactive, browser-accessible shell for managing Azure resources. It provides the flexibility of choosing the shell experience that best suits the way you work. Linux users can opt for a Bash experience, while Windows users can opt for PowerShell. This browser-based terminal lets you control and administer all of your Azure resources in the current subscription through a command-line interface built right into the portal.

#### Directory and subscription

Select the __Book and Filter__ icon to show the __Directory + subscription__ pane.

Azure allows you to have more than one subscription associated with one directory. On the __Directory + subscription__ pane, you can change between subscriptions. Here, you can change your subscription or change to another directory.

#### Notifications
Selecting the bell icon displays the __Notifications__ pane. This pane lists the last actions that have been carried out, along with their status.

#### Settings
Select the gear icon to change the Azure portal settings. These settings include:

* Inactivity sign out delay
* Default view when you first sign in
* Flyout or docked option for the portal menu
* Color and contrast themes
* Toast notifications (to a mobile device)
* Language and regional format

#### Help panel

Select the __question mark__ icon to show the __Help__ pane. Here you choose from several options, including:

* Help + Support
* What's new
* Azure roadmap
* Launch guided tour
* Keyboard shortcuts
* Show diagnostics
* Privacy statement

#### Help and support options

__Help + support__ opens the main help and support area for the Azure portal and includes documentation options for a variety of common questions. One of the hidden areas here is the __New support request__ link, which is on this page. This link is how you can open a support ticket with the Azure team.

All Azure customers can access billing, quota, and subscription-management support. _The availability of support for other issues depends on the support plan you have._

When you open a support ticket, you will complete the form by using provided dropdown lists and text-entry fields.

Once you've filled out the form, select Create to submit your support request. The Azure support team will contact you after you submit your request.

You can then check the status and details of your support request, by going to __Help > Help +support > All support requests__.

#### Feedback panel

The __smiley face__ icon opens the Send us feedback pane. Here you can send feedback to Microsoft about Azure. You can decide as part of your feedback whether Microsoft can respond to your feedback by email.

#### Profile settings
If you select on your name in the top right-hand corner, a menu opens with a few options:

* Sign in with another account, or sign out entirely
* View your account profile, where you can change your password

Select the "..." button on the right-hand side for options to:

* Check your permissions
* View your bill
* Update your contact information

If you select "..." and then __View my bill__, Azure takes you to the __Cost Management + Billing - Invoices__ page, which helps you analyze where Azure is generating costs.

Azure is a large product, and the Azure portal user interface (UI) reflects this scope. The sliding pane approach allows you to navigate back and forth through the various administrative tasks with ease. Let's experiment a bit with this UI so you get some practice.

#### Azure Advisor
Finally, the Azure Advisor is a free service built into Azure that provides recommendations on high availability, security, performance, operational excellence, and cost. Advisor analyzes your deployed services and looks for ways to improve your environment across those areas. You can view recommendations in the portal or download them in PDF or CSV format.

With Azure Advisor, you can:

* Get proactive, actionable, and personalized best practices recommendations.
* Improve the performance, security, and high availability of your resources as you identify opportunities to reduce your overall Azure costs.
* Get recommendations with proposed actions inline.

You can access Azure Advisor by selecting __Advisor__ from the navigation menu, or search for it in the All Services menu.


###  Exercise - Work with panes

#### Activate the Azure sandbox
1. Start by activating the Azure sandbox above.

2. Once it's activated, sign into the Azure portal for sandbox . Make sure to use the same account you activated the sandbox with.

#### Explore the Marketplace

Once you are logged into the Azure portal, we can start exploring things. In these sections, you get a tour of the Azure portal, but won't actually create any Azure resources.

1. Let's start by touring how to create a resource. On the home page, select __Create a resource__.

2. A pane labeled New appears and displays a list of categories on the left-hand side, labeled Azure Marketplace. This initial view is somewhat like a `"Popular Categories"` menu, with some of the most common categories visible. If you'd like, you can expand this list to see everything in the Marketplace pane with the See all link next to the heading. If you do, you can get back to the New pane by selecting the X icon in the upper right of any panes you have opened.

3. Selecting any of items in the Azure Marketplace list will show popular services for that category on the right of the __New__ pane. This list is a subset of the entire range of computing resources available for that category. As with the Azure Marketplace, you can select on the __See all__ link for a more comprehensive list. We will talk more about this list in subsequent sections.

4. Returning to the New pane, select on Get started and you should see a list on the right side of the pane that includes services such as Windows Server 2016 Datacenter, Ubuntu Server VM, SQL Database, and so on. Most of these items include a Quickstarts + tutorials link directly below the name. This link opens a new browser tab with the quickstart Microsoft documentation for that item.

5. Optional: select Quickstarts + tutorials under Windows Server 2016 Datacenter and, in the new browser window, look through the Windows VM tutorials. When you are finished, close this new tab to return to the Azure portal

#### View resources

1. Under Azure Marketplace, select Compute to show more compute options on the right side of the pane, such as Red Hat Enterprise, Reserved VM Instances, Web app for Containers, and so on.

2. To the right of Featured, select See all. The full list of available VM services now appears.

3. Select __Windows Server__ under the Operating Systems section. On screens with limited horizontal space for the pane, you may have to scroll right and select the See More link to find the Windows Server option.

4. Select the drop-down list to see all of the Windows Server images available.

5.Select the __X__ at the top right-hand corner to close the __Windows Server__ window.

6. Select the __X__ on the previous Marketplace window. You should now see the __New__ pane again.

#### Filter results
Another way to locate services is to refine the list with filters and search terms. On the New pane, you may have noticed the search box at the top. Searching is the quickest way to filter what services you see.

This search defaults to checking every Azure service category to get its results. Next, you'll filter after selecting a category.

1. Type `virtual machine` into the search box and select `Enter`.

2. Select Compute. You see a filtered list of Compute services related to virtual machine images.

3. Select any of the results that interest you to learn more about that service, including how to get started. Select the X in the corner to explore a different service. When you're done, move to the next step.

4. Select the X in the right-hand end of the search box. The X button will erase your search term but does not reset any of the drop-down filters you've set. You can either reset those filters manually, or close the Marketplace pane with the X icon in the upper right corner and reopen it. When you are finished trying out the search and filtering options, move on to the next step.

5. Select the X at the top right-hand corner to close the Marketplace pane. Now you will see the New pane once again.

6. Select the X at the top right-hand corner to close the New pane.

Many of the principles you have learned in this exercise apply throughout the Azure portal UI experience. In the next unit, you continue your journey in the Azure portal and configure additional settings in Azure.

### Exercise - Use the Azure portal

The Azure portal has several features and services available; let's look at some of the more common areas you'll tend to use. First, take a moment to hover your mouse pointer over each of the icons in the top menu bar for a few seconds each. You should see a tooltip label pop-up for each one. This label is the name of the menu item. You will use these icons later.

#### All services

1. On the top left-hand side of the Azure portal, __select Show portal menu.__

2. Select __All services__. Take a couple of minutes to look through the list to see how many services Azure offers.

3. You can search for services through the _Search All box_.

4. Select __Virtual machines__. If you don't see it, use the search box. The Virtual Machines pane appears. You haven't created any virtual machines so there are no results.

5. Select __+ Add > Virtual machine__. The __Create a virtual machine__ pane appears.

6. Select the X in the top right-hand corner to close the Create a virtual machine pane.

7. Select the X in the top right-hand corner to close the Virtual machines pane.

8. Select on Microsoft Azure on the top left-hand side to get back to the home page.

#### Azure Cloud Shell

The Azure Cloud Shell allows you to use a command-line interface (CLI) to execute commands in your Azure subscription. You can access it by selecting the (`>_`) icon in the toolbar. You can also navigate to https://shell.azure.com to launch a Cloud Shell in the browser independent of the portal.

The Azure Cloud Shell is available in the Sandbox environment, but the Sandbox version of the shell has reduced functionality. To use all of the Azure Cloud Shell features, use your own Azure subscription.

When you launch the shell, you see a Welcome window. You can choose either a __Bash__ or __PowerShell__ environment, depending on your personal preferences. You can also change the shell at any time through the language drop-down on the left side of the shell.

Finally, there are a variety of management and programming tools included in the created environment.

* Azure command-line tools (Azure CLI, AzCopy, etc.)
* Languages / Frameworks including .NET Core, Python, and Java
* Container management support for Docker, Kubernetes, etc.
* Code editors such as vim, emacs, code, and nano
* Build tools (make, maven, npm, etc.)
* Database query tools such as sqlcmd

#### Directory and subscription

1. Select the __Directory + Subscription__ (book and filter) icon to show the __Directory + subscription__ pane.

This is where you can switch between multiple subscriptions or directories. You should see that you are in the Concierge Subscription of the Microsoft Learn Sandbox directory here. If you have other Azure directories tied to the same email address, those subscriptions will be available as well.

There is also a link to learn more about directories and subscriptions.

Select the __X__ in the top right-hand corner to close the __Directory + subscription__ pane.

#### Notifications pane

1. On the icon bar menu bar, select the Notifications (bell) icon. This window lists any pending notifications.

2. If any notifications appear, hover your mouse over one of them. Select the X that appears in that notification to dismiss it.

3. Select Dismiss all. You should have no notifications showing.

4. Select the X in the top right-hand corner to close the Notifications pane.

#### Settings
1. Select the Settings (cog) icon to open the Portal settings pane, showing the General settings by default.

2. Drop down the Sign me out when inactive setting, and select After one hour.

3. Under Choose a theme, select the different colored themes and observe the changes to the portal UI. Leave it set to the one you like the best.

4. Under High contrast theme, try the three different options.

5. Select Enable pop-up notifications. When this option is checked, notifications will appear as pop-up "toast"-style notifications. They will still show up in the Notifications (bell) icon as well.

6. Select the Language & region tab in the settings. Select Language and pick Español, and then select the Apply button. If a Translate this page dialog box appears, close the box. The whole portal is now in Spanish.

7. To revert back to English, select the Settings (cog) icon in the top menu bar and switch to the Idioma y región settings. Select Idioma and pick English. Select the Aplicar button. The portal returns to English.

#### Help pane

1. Select the Help (?) icon to show the Help pane.

2. Select the Help + support button.

3. In the Help + support pane, under Support, select New support request. To create a new support request, you would fill in the information in each of the following sections, and then select Create to lodge the issue.

* Basics: the issue type
* Problem: severity of the problem, a summary and description, and any additional information
* Contact information: preferred contact method and the information associated with this contact method
4. You can view the status of your support requests by selecting on All support requests.

Support requests can only be created using an active paid subscription. Creating support requests from a free Microsoft Learn sandbox is not supported.

#### What's new and other information
1. Select the Help icon and select What's new.

2. Review the features that have recently been released. Also note and explore the other Help menu options, such as:

* Azure roadmap
* Launch guided tour
* Keyboard shortcuts
* Show diagnostics
* Privacy statement

3. Select the X in the top right-hand corner to close the Help pane.

4. Close the What's new pane. You should now be back to the Dashboard.

#### Feedback pane

1.Select the __Feedback__ (smiley face) icon to open the Send us feedback pane.

2. Type your impressions of Azure in the Tell us about your experience box, select the box that says Microsoft can email you about your feedback, and select Submit Feedback.

3. A Feedback sent message will appear, and then close. You should now be back at the Dashboard.

#### Profile settings

1. Select on your name in the top right-hand corner of the portal. Options include:

* Sign in with another account, or sign out entirely
* View your account profile, where you can change your password
* Submit an idea
* Check your permissions
* View your bill
* Update your contact information

Some of these items do not appear unless you select the "..." icon.

1. Select "..." then __View my bill__ to navigate to the __Cost Management + Billing - Invoices__ page, which helps you analyze where Azure is generating costs.

If you're using your own account and not sandbox, you can select a subscription from the drop-down list.

Select a billing period.

Note the service costs and check them against what you expect for your current subscription.

Select the X in the top right-hand corner to close the __Costs by service__ pane.

Select the X in the top right-hand corner to close the __Cost Management + Billing - Invoices__ page.

You should now be back on the home page.

Now that we've explored all the main areas of the Azure portal, let's look at one of the most useful features - Dashboards.

### Azure Portal dashboards

Let's look at how to create and modify dashboards using the Azure portal, and by editing the underlying JSON file directly. In this unit, you'll learn to navigate around the portal. And in the next unit, you will try out the things you've learne

#### What is a dashboard?

A _dashboard_ is a customizable collection of UI tiles displayed in the Azure portal. You add, remove, and position tiles to create the exact view you want, and then save that view as a dashboard. Multiple dashboards are supported, and you can switch between them as needed. You can even share your dashboards with other team members.

Dashboards give you considerable flexibility regarding how you manage Azure. For example, you can create dashboards for specific roles within the organization, and then use role-based access control (RBAC) to control who can access that dashboard. Hence, your database administrator would have a dashboard that contains views of the SQL database service, whereas your Azure Active Directory administrator would have views of the users and groups within Azure AD. You can even customize the portal between your production and development environments within the portal - creating a specific dashboard for each environment you are managing.

Dashboards are stored as JavaScript Object Notation (JSON) files. This format means they can be uploaded and downloaded to other computers, or shared with members of the Azure directory. Azure stores dashboards within resource groups, just like virtual machines or storage accounts that you can manage within the portal.

Because dashboards are JSON files, you can also [customize them programmatically](https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards-create-programmatically), making them compelling administrative tools. Also, some tile types can be query-based, so they update automatically when the source data changes.

#### Explore the default dashboard

The default dashboard is named "Dashboard". When you log into the portal for the first time and select Dashboard from the portal menu, you are presented with this dashboard containing five tiles.

These default web parts are:

* Dashboard controls

* All resources tile

* Quickstarts + tutorials tile

* Service Health tile

* Marketplace tile

#### Creating and managing dashboards

At the top of the dashboard are the controls that enable you to create, upload, download, edit, and share a dashboard. You can also switch a dashboard to full screen, clone it, or delete it.

#### Select dashboard

To the far left of the toolbar is the __Select Dashboard__ drop-down control. Clicking this control enables you to select from dashboards that you have already defined for your account. This control makes it simple for you to define multiple dashboards for different purposes and then switch from one to another and back again, depending on what you are trying to do at the time.

Dashboards that you create will initially be private; that is, only you can see them. To make a dashboard available across your enterprise, you need to share it. We'll look at that option shortly.

#### Create a new dashboard

To create a new dashboard, click New dashboard. The dashboard workspace appears, with no tiles present. You can then add, remove, and adjust tiles however you like. When you are finished customizing the dashboard, click Done customizing to save and switch to that dashboard.

#### Upload and Download

The __Upload__ and __Download__ buttons enable you to download your current dashboard as a JSON file, customize it, and then distribute it and upload it or have someone else upload that file back to the Azure portal, thereby replacing their current dashboard.

If you click __Download__, the current dashboard downloads the JSON code as a file you can edit locally. You can then upload it back to Azure by clicking the Upload button. Downloading and uploading dashboards is discussed further below.

#### Edit a dashboard using the portal


Although you can edit a dashboard by downloading the JSON file, changing values in the file, and uploading the file back to Azure, you may prefer a graphical approach to designing the user interface. To use the GUI to configure your current dashboard, you can switch to editing mode in several ways:

* Click the Edit (pencil icon) button.
* Right-click on the dashboard background area and select Edit.
* Right-click on a tile and a menu will appear with edit options.
* Hover over a tile on the dashboard - a ... menu will appear on the top/right corner with a Customize option.

The dashboard will switch to edit mode.


On the left-hand side appears the Tile Gallery, with several possible tiles. You can filter the Tile Gallery by category and resource type.

Adding tiles is as easy as selecting the tile from the list on the left and then dragging it to the work area. You can then move each tile about, resize it, or change the data that it displays.

> __Tip__ <br>
> One cool feature is that you can take elements on child panes and put them on your dashboard. Just hover over the item and look for the ... tile edit menu - this will have a "Pin to Dashboard" option which lets you quickly grab a tile from a service and put it onto the dashboard.

The work area in edit mode is divided into squares. Each tile must occupy at least one square, and tiles will snap to the nearest largest set of tile dividers. Any overlapping tiles are moved out of the way. When you make a tile smaller, the surrounding tiles will move back up against it.

#### Change tile sizes

Some tiles have a set size, and you can edit their size only programmatically. However, you can edit tiles with a gray bottom right-hand corner by dragging the corner indicator.

Alternatively, right-click into the contextual menu and specify the size you want.

To create your dashboard, pull tiles from the Tile Gallery onto the workspace and then rearrange them.

#### Change tile settings

Some tiles have editable settings. For example, with the clock tile, when you drag it onto the workspace, it opens the __Edit clock__ tile. You can then set the time zone, which it displays, and also set whether it displays in 12- or 24-hour format.

For multi-national or transcontinental companies, you can add several clocks, each in a different time zone.

#### Accepting your edits

When you have arranged the tiles as you want them, either click Done customizing or right-click and then click __Done customizing__.

#### Edit a dashboard by changing the JSON file

You can also edit a dashboard by changing the JSON file. This approach provides more options for changing settings, but you cannot see the changes until you upload the file back into Azure. The easiest starting point is to download the dashboard JSON as previously described and edit that file.

As an example, in the JSON shown above, to change the size of the tile you would edit the colSpan and rowSpan variables, then save the file and upload it back to Azure.

> __Tip__ <br>
> You can also distribute the dashboard JSON file to other users.

#### Reset a dashboard

You can reset any dashboard to the default style. In edit mode, right-click the dashboard background and select Reset to default state. A dialog box will ask you to confirm that you want to reset that dashboard.

#### Share or unshare a dashboard

When you define a new dashboard, it is private and visible only to your account. To make it visible to others, you need to share a dashboard. However, as with any other Azure resource, you need to specify a new resource group (or use an existing resource group) in which to store shared dashboards. If you do not have an existing resource group, Azure will create a dashboards resource group in whichever location you specify. If you have existing resource groups, you can specify that resource group to store the dashboards.

When you have shared the template, you will see a second Sharing + access control pane.

You can then click Manage users to specify the users who have access to that dashboard.

#### Switching to a shared dashboard

To switch to a shared dashboard, you click on the list of dashboards, and then click Browse all dashboards.

You will now see the All dashboards pane, with the names of any shared dashboards displayed. Just click on a dashboard to apply it to the Azure portal.

#### Display a dashboard as a full screen

If you want the largest dashboard real estate, click the Full screen button to display your current dashboard without any browser menus. If you have any tiles outside the boundaries of your screen display, slider bars will appear at the right and bottom of your screen.

When you have finished working in full-screen mode, press the ESC key or click Exit Full Screen next to the Dashboard name at the top of the screen.

#### Clone a dashboard

Cloning a dashboard creates an instant copy called `"Clone of <dashboard name>"` and switches to that copy as the current dashboard. Cloning is also an easy way to create dashboards before sharing them. For example, if you have a dashboard that is almost as you want it, clone it, make the changes that you need, and then share it.

#### Delete a dashboard

Deleting a dashboard removes it from your list of available dashboards. You are prompted to confirm that you want to delete the dashboard, but there is no facility to recover a dashboard that has been deleted.

Let's try out some of these options by creating a new dashboard.


### Exercise - Customize the dashboard

Dashboards are a flexible tool for managing different aspects of Azure services through the Portal. They make it convenient to monitor the state of your services. Because they are shareable, they help ensure that everyone on your team sees the same data and stays aware of the state of your critical components. Let's create a new dashboard and add some tiles to it.

#### Create a new dashboard

1. In the Azure portal , from the top left-hand side, select Show portal menu > Dashboard.

2. Select the New Dashboard

3. In the center pane, change My Dashboard to Customer Dashboard.

#### Add and configure the Clock Tile
1. In the tile gallery, drag the clock onto the workspace. Place it on the top right of the available space.

2. On the Edit clock pane, change the Location to Pacific Time (US & Canada).

3. Under Time format, select 24 hour.

4. Select Done.

5. Repeat the preceding four steps, except select Eastern Time (US & Canada). You should now have two clocks, one showing the time on the West Coast, the other on the East Coast.

#### Resize a tile

1. Under Tile Gallery, drag an All resources tile and drop it onto the top left-hand side of the new dashboard workspace.

2. Hover over the new All resources tile and select the ellipsis icon (...); then select the 6x6 size.

3. Select the gray corner on the bottom right-hand side of the tile, and resize the tile to 3.5 squares vertically by six horizontally. When you finish resizing, the tile adjusts to 4x6.

4. In the Tile Gallery, drag the Resource Groups tile onto the workspace. Place it underneath the All resources tile.

5. In the Tile Gallery, select the Metrics chart tile, and drag it onto the workspace. Place it to the right of the All resources tile.

6. Continue to add the following tiles, rearranging them to fit:

* Help + support
* Quick Tasks
* Marketplace

7. When you have added these tiles, select Done customizing. The Customer Dashboard dashboard should appear.

#### Clone a dashboard

You now want to create a similar dashboard for some other customers.

1. Select the Clone button.

2. Rename the dashboard from Clone of Customer Dashboard to Azure AD Admin Dashboard.

3. On the Resource Groups tile, select the Remove from dashboard trash can icon to delete this tile.

4. From the Tile Gallery, add the following tiles:

* Organization Identity
* Users and Groups
* User Activity Summary

5. Reposition the tiles as necessary, and then select Done customizing.

#### Share a dashboard

You now want to make this dashboard available to other users. In the sandbox environment, you won't be able to publish a shared dashboard. But you can see how you'd share a dashboard by completing the following steps.

1. From the Azure AD Admin dashboard, select the Share button at the top. The Sharing and access control panel that appears.
2. To publish to a specific resource group, uncheck the Publish to the 'dashboards' resource group checkbox.
3. Select the resource group learn-4e3284c9-afbd-4c3b-a03f-e209e662a7dd from the Resource group dropdown.
4. Select Publish.
5. At this point in the sandbox environment, you'll receive an error. That's ok.
6. Close the Sharing + access control pane.

#### Edit a dashboard.json file

1. Select Download.

2. Open a file explorer on your computer and navigate to where your web browser downloaded the dashboard, typically a Downloads folder.

3. Find the Customer Dashboard.json file and open it in a text editor.

4. In your editor, look for the text ClockPart.

5. On the first occurrence of ClockPart, change the previous position > rowSpan value to 1.

6. On the second occurrence of ClockPart, also change the previous position > rowSpan value to 1.

7. On the second occurrence of ClockPart, change the position > y value from 2 to 1.

8. Save the Customer Dashboard.json file and close your code editor.

9. On the Azure dashboard, select Upload.

10. In the Open dialog box, browse to the Downloads folder, and double-click Customer Dashboard.json.

The clocks have resized to one row high, and the bottom clock has moved up one row.

#### Delete a dashboard

1. Ensure that the Azure AD Admin dashboard is selected.

2. Select the Delete button.

3. In the Confirmation message box, select the checkbox to confirm that this dashboard will no longer be visible, and then select OK.

#### Reset a dashboard
1. Ensure that Customer Dashboard is selected.

2. Select Edit.

3. Right-click on the workspace, and select Reset to default state.

4. In the Reset dashboard to default state message box, select Yes.

    The Customer Dashboard has reset to its default tiles.

5. Select Done customizing.

6. Select your name at the top right of the portal.

7. Select Sign out.

8. Close your browser.

Congratulations! You have now created and edited dashboards, shared them, altered them as JSON files, and finally, reset them to the default state. You should now be able to see what powerful tools dashboards can be and how you can use them to create efficient interfaces for differing roles within an organization.


### Access public and private preview features

Microsoft offers previews of Azure features for evaluation purposes. With _Azure Preview Features_, you can test beta and other pre-release features, products, services, software, and regions.

Some of the common areas you will see previews for include:

* New storage types
* New Azure services, such as Machine Learning enhancements
* New or enhanced integration with other platforms
* New APIs for services

Azure feature previews are available under certain terms and conditions that are specific to each particular Azure preview. Also, some previews are not covered by customer support.

Once a feature has been evaluated and tested successfully, it might be released to customers as part of Azure's default product set. This release is referred to as __General Availability (GA)__.

#### Feature preview categories

There are two types of previews available:

* __Private Preview__. An Azure feature marked "private preview" is available to specific Azure customers for evaluation purposes. This is typically by invite only and issued directly by the product team responsible for the feature or service.

* __Public Preview__. An Azure feature marked "public preview" is available to all Azure customers for evaluation purposes. These previews can be turned on through the preview features page as detailed below.

#### Finding preview features

You can learn about preview features through the preview features page . This page lists the preview features that are available for evaluation. To access a preview feature, select its entry on this page and learn more about how to evaluate it. You can also use the RSS Feed button on this page to subscribe to notifications and stay informed.

You can also find Azure preview features in the portal as follows:

1. Sign in to Azure portal.
2. Select Create a resource in the resources panel to open the New pane.
3. Enter the word preview into the search box at the top of the New pane.
4. A list of available preview features is displayed, with the word (preview) next to each one.

#### Azure portal preview features

Another preview area you can try is the next version of the Azure portal. Use the URL https://preview.portal.azure.com  (notice the preview prefix).

Typical portal preview features provide performance, navigation, and accessibility improvements to the Azure portal interface. It will be branded with Microsoft Azure (Preview) in the top bar, so you will know you are in the preview portal.

#### Provide feedback on preview features

If you utilize the preview portal or a preview feature, Microsoft wants to hear about your experience. You can provide feedback through the "smiley" face icon on the portal or by posting ideas and suggestions on the Azure portal Feedback Forum.

#### Get notified about GA releases

The Azure portal "What's New" link on the help menu (?) provides a list of recent updates you can periodically check to see what's changed in Azure.

Alternatively, you can use the Azure Updates page. This page provides additional information and features including:

* Which updates are in general availability, preview, or development.
* Browse updates by product category or update type, by using the provided dropdown lists.
* Search for updates by keyword by entering search terms into a text-entry field.
* Subscribe to get Azure update notifications by RSS.

### Summary

We have covered lots of ground in this module.

You have learned how to sign into Azure using an Azure account.
You reviewed the features of the Azure portal and its customization options.
You created, customized, and shared a dashboard.
However, this tour is just the beginning. Azure has so much to offer you, no matter what role you play in your organization. If you are a developer, Azure provides an easy way to test new platforms and build sophisticated apps. If you are an administrator, you will use the Azure portal, Azure CLI, or Azure PowerShell tools to administer your cloud-based infrastructure. If you are an architect, you can use Azure to test out new architecture ideas quickly.

Keep exploring Azure by selecting one or more paths through the content that's structured specifically for what you want to learn.