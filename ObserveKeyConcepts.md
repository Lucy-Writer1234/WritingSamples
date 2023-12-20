# Introducing Observe Concepts

Observe collects all of your data, system and application logs, metrics, tracing spans, and any other kind of data then ingests them into a *data lake*. Then, depending on your individual use case, Observe curates the relevant event data into *datasets* that provide more structure, faster queries, and make it easier to understand the information than the original raw event datastream. 

Datasets can be linked to other datasets to make it easy for you to access and correlate relevant context during an event investigation. You can then package up datasets, along with dashboards and alerting conditions(monitors), to produce Observe applications.

To use Observe effectively, understanding the key concepts of Observe provides you with the background to perform the following tasks:

* Ingesting data into the Observe data lake using datastreams.
* Installing Observe applications.
* Using the Dataset Graph feature to understand datasets and the relationships between them. 
* Using dashboards to view and filter datasets.
* Navigating between related datasets using GraphLink.
* Using worksheets and Observe Processing Analytics Language (OPAL) for manipulating event data.
* Visualizing metrics and presenting them on dashboards.
* Live incident debugging using **Live Mode**.

## Datastreams
Observe ingests event data using *datastreams*. Once ingested, all event data associated with a specific datastream populates a *dataset*.

A datastream ingests data from multiple different sources with each source identified by a unique token. 

Individual tokens may be enabled, disabled, or even deleted without impacting data associated with a different token on the same Datastream. This allows you to easily rotate datastream tokens. 

More about information about **Datastreams** can be found [here](https://docs.observeinc.com/en/latest/content/data-ingestion/datastreams.html).

## Datasets

*Datasets* are much like tables in a database. They consist of rows and columns of specific types. They can be linked, joined, and aggregated to derive insights. But unlike traditional tables, datasets automatically grow as Observe collects new data.

Datasets contain built-in support for time and history. This provides an easy way to track the state and relationships between objects you care about over time, whether the object is a server in a data center or a virtual object such as a shopping cart in an e-commerce application. 

Datasets accelerate the querying of large amounts of data by continuously transforming raw data into more structured information. The information becomes easier to understand and faster to query. 

A dataset has a name and lives within a project. Project names must be unique within a workspace, and Dataset names must be unique within that project. When you log into Observe, the **Explore** page lets you browse the different projects and Datasets that exist within your workspace.


```{image} images/explore-datasets-new.png
:align: center
:width: 650px
:alt: Explore datasets
```
*<p align="center">Figure 1 - Explore Datasets</p>*

Datasets may be installed by an Observe application with pre-built collections of datasets, dashboards, and alerts for observing specific environments such as AWS, Kubernetes, or Linux hosts.  Alternatively, you create them for custom use cases, for example, a mobile payments application.

Create links between pre-built datasets and custom datasets, combine information from any number of datasets in dashboards, and create monitors that combine pre-built and custom datasets. You can even look at the definitions of pre-built datasets, dashboards, and monitors, to learn from them and extend them.
 
When creating custom datasets, you must specify a set of named columns, the type of data stored in the columns, and the kind of custom dataset you want to create, either an *Event Dataset* or *Resource Dataset*.

### Event Datasets

All event data that Observe ingests on a datastream goes into a corresponding source dataset. This type of source dataset consists of nothing but timestamped events and is an *event dataset*.

This source dataset typically contains billions of events and as such is not very efficient to work through.  Rather than just providing you with a search bar to look for breadcrumbs, Observe allows you to further curate the source dataset into smaller, more useful, chunks.

For example, the Kubernetes source dataset may break out Container Logs  which can be further broken out into Web Logs and Application Logs thereby creating three discrete event datasets. The Web Logs dataset can feature an `HTTP Status Code` column which may derive from Container Logs using a regular expression. If you want to look for `404 (Not Found)' errors for a website, you can now go directly to the Web Logs and quickly find the data you need.
 
Observe identifies Event Datasets with purple icons. 

```{image} images/event-streams.png
:align: center
:width: 200px
:alt: Event Streams
```
*<p align="center">Figure 2 - Event datasets</p>* 


Kubernetes Container Logs provide an example of an event Dataset. 

```{image} images/kube-logs.png
:align: center
:width: 850px
:alt: Event Streams
```
*<p align="center">Figure 3 - Kubernetes Log datasets</p>*

### Resource Datasets
 
Resource datasets contain information about virtual or physical things such as servers, users, shopping carts, etc. Observe collectively calls these *resources*.

Each resource dataset contains a primary key, a combination of one or more columns, that uniquely identifies a resource. For a container resource dataset, this can be as simple as a *Container ID*. The primary key enables the resource dataset to link to other, related, datasets. The links are similar to foreign key relationships in traditional databases. Primary keys and links drive many of the advanced search and navigational capabilities in Observe.

Essentially, resource datasets behave like *temporal tables* and store the full history, state, and relationships of virtual or physical things such as servers, users, or shopping carts.

For example, Kubernetes regularly sends events describing the state of pods, containers, and more, at a particular point in time. Observe collects all of those events and derives an inventory of all pods and containers along with the associated state over time.

Observe derives the individual states and state transitions from event datasets. This means you can not only see the current state of all the pods, containers, and more, in the Kubernetes cluster. You can also see the state, and the state of related resources, as it was an hour ago, a day ago, or last month. And you can drill down into individual events that affected the state and state changes.

Resource datasets store objects with permanence over time, and the state changes over time. *State* may be composed of many attributes, each with a specific value for a specific time interval. Because a Resource Dataset keeps track of all attributes, you can ask questions such as “What was the state of Pod P at time T?”.

Each attribute forms a column in the respective resource dataset and the resource dataset represents the time intervals by a pair of designated `valid-from` and `valid-to` columns. 


Observe identifies **Resource** Datasets with blue icons.

```{image} images/resource-sets.png
:align: center
:width: 200px
:alt: Event Streams
```
*<p align="center">Figure 4 - Resource datasets</p>*

Kubernetes Pods is an example of a Resource dataset.

```{image} images/kube-pod.png
:align: center
:width: 850px
:alt: Kubernetes resource dataset
```
*<p align="center">Figure 5 - Kubernetes Resource dataset</p>*


## Worksheets

Worksheets provide a spreadsheet-type interface for directly manipulating resource or event datasets, enabling you to perform tasks such as extracting fields, aggregating, visualizing, and correlating data.

Add content to worksheets in the form of stages, each based on a specific dataset, such as `Container Logs`, or on specific metrics such as `CPU_Utilization`. Stages can be completely independent of each other or you can link them together. Linked stages can be useful to show a series of logical steps in an investigation, which you can also share with other users.

You can interact with a worksheet in several different ways. Mostly, you create stages and manipulate data using the Observe user interface. If you want more control, Observe provides OPAL (Observe Processing Analytics Language) scripting through the Observe Console.  To assist with learning OPAL, most actions in the Observe user interface generate the corresponding OPAL script in the console window located at the bottom of the worksheet.

Create a new Worksheet from a Dataset by hovering over the name of the Dataset and selecting the **Worksheet** icon.

## Metrics

Observe defines a metric as a numeric value you can measure over time. The values can be the number of nodes in a cluster, the number of users logging into a website, or CPU usage over time. Observe reports metrics in the form of a *time series*, a set of values in the order of time. Each value in a time series represents a measurement from a single resource and includes the name and value.

Observe ingests and curates time-series data into event datasets which contain both metrics data and metadata for additional context. Any event dataset with a numerical value column can be interpreted as a *metric dataset*.

Observe provides two different formats for metric datasets, and the best approach depends on the source of the data as well as the type of operations you want to perform.

* Narrow metrics – the *Metric Dataset* contains one metric per row in a table. One metric consists of a single data point with a timestamp, name, value, and zero or more tags. 

* Wide metrics – the *Metric Dataset* contains several related metrics. This form provides the ability to easily calculate data, such as percentage of usage, because Observe groups the values in the same row.

Observe provides you with the capability to convert back and forth between the two formats.

For more information on metrics, see [Introduction to Metrics](../metrics/MetricsIntro.md).

## Dataset Graph

The **Dataset Graph** feature displays the interrelationships between your datasets. When you create worksheets, you use Dataset Graph to link that information to other datasets and create a relational database of all your data.

The **Dataset Graph** contains three different views of datasets:

* **Links** - displays datasets and their links, and optionally displays the status of each dataset, such as whether the dataset is currently receiving data or not.
* **Lineage** - displays how each dataset is derived, with source datasets on the far left of the graph, and data flow to the right, to successively derived datasets. 
* **Focus** - displays the currently selected dataset and links to and from it.  

[Take a tour of GraphLink](https://player.vimeo.com/video/696195193)

## Dashboards

*Dashboards* provide a logical way to group data visualizations within Observe.

You create dashboards by selecting **New Dashboard** from the **Explore** page.  You may base the new dashboard on an existing dataset, and benefit from auto-generated content, or start from scratch.
When creating a dashboard, go to the dashboard panel **Definition** and add the data you want to view.  Then you can go to the **Design** panel, and drag and drop data on your dashboard and arrange tiles as you want to view them.

Dashboards may be driven by parameters and you can customize the view based on the parameter value selected.  For example, you may want to view the health of a Kubernetes cluster and use a parameter named **Environment** to determine whether the dashboard shows **Development**, **Test**, or **Production** dashboard views.

Parameters may be supplied directly by you or they can be a list of values derived from an existing resource dataset.

```{image} images/sample_dash.png
:align: center
:width: 850px
:alt: Example dashboard
```
*<p align="center">Figure 6 - Dashboard Example</p>*

## Applications

Observe *Applications* packages related content to make it easier for you to get started.

Observe Applications typically consist of definitions for datastreams, datasets, dashboards, and monitors relevant to a specific use case such as AWS or Kubernetes. The exact details of the app content can be seen by selecting **View Content** on an application tile, on the **Apps** home page.  

```{image} images/new-apps.png
:align: center
:width: 850px
:alt: Apps landing page
```
*<p align="center">Figure 7 - Available Apps</p>*

Install an application into an Observe environment by clicking **Install** on the relevant application tile.  You can choose to customize the installation and select only the items you want to install or proceed with a typical installation.

Application definitions are exported to JSON and can be managed as code in a Github repository. Observe manages the upgrades to all applications within the user interface.


## Monitors

Observe Monitors provide a flexible way to alert for patterns in your incoming data. Define who should receive alerts with channels and channel actions, then create monitors to watch for your desired conditions. When one occurs, Observe sends alerts to everyone or every service in the channel. You can send alerts to any combination of email addresses and Webhook-enabled services.

Monitors complement resource notifications by adding alerts.

A Monitor watches a dataset for a particular condition, such as a count of events or a specific text value. When you create a monitor, Observe creates a new dataset based on the contents of the page and your conditions. This allows multiple monitors from the same page to be independent of each other.

Monitors provide five options to use for the notifications:

* **Threshold Metrics** - Compare the count of events to a static threshold.
* **Count** - Compare a numeric value to a static threshold.
* **Text Value** - Use a specific text field to monitor the dataset.
* **Promote** - Select an event in the dataset to receive Notifications when the event occurs.
* **Threshold Log** - select a log message that triggers an Alert and receive Notifications about the Alert. 


More information about **Metrics** can be found [here](https://docs.observeinc.com/en/latest/content/metrics/MetricsIntro.html).


## Data Table Settings Overview

For each table in Worksheets, Logs, and Datasets, you can adjust the table to suit your viewing needs. The following settings can be viewed when you click the **Table settings** <img src="images/table-settings.png" alt="Table settings icon" width="120" height="22" style="vertical-align:bottom;"> icon:

* **Columns** - a list of columns in the table
* **View** - adjust the row sizing
* **Limit** - select the **MAX RESULTS** to view the table from 1 to 100,000 rows.

```{image} images/menu-table.png
:align: center
:width: 250px
:alt: Column Settings Menu
```
*<p align="center">Figure 9 - Column Settings</p>*

```{image} images/table-view.png
:align: center
:width: 250px
:alt: View Settings Menu
```
*<p align="center">Figure 10 - View Settings</p>*

The default value is **Flexible**.

```{image} images/table-limit.png
:align: center
:width: 250px
:alt: Limit Settings Menu
```
*<p align="center">Figure 11 - Limit Settings</p>*

 The default value is **10,000**.
 

### Displaying All Rows in a Data Table

Logs, and Datasets display **Filters** with parameters you can use to narrow or expand data in the table. Initially, the **Filters** use the default Row Limit to locate the applicable parameters. 

```{image} images/filters.png
:align: center
:width: 250px
:alt: Initial Set of Filters from 10,000 Rows
```
*<p align="center">Figure 10 - Initial Set of Filters from 10,000 Rows</p>*

If you cannot locate your Filter in the initial **Filters** list, click the **V** next to **Fetched from 10.0K rows** and click **Fetch from all rows**. Now all of the rows in the dataset display in the table, and all Filters become available in the **Filters** list. 

```{image} images/fetch-all-rows.png
:align: center
:width: 250px
:alt: Fetched from All Rows
```
*<p align="center">Figure 11 - Fetched from All Rows</p>*

To view the entire list of Filters, click **View More** and scroll down the list until you locate your Filter. 

```{image} images/view-more.png
:align: center
:width: 350px
:alt: View More Filters
```
*<p align="center">Figure 12 - View More Filters</p>*

After you select a Filter, the **Filters** list returns to the default Row Limit.

```{toctree}
---
maxdepth: 1 
hidden: true
---
IntroAdvObserveConcepts

```

