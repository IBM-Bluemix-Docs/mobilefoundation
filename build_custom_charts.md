---

copyright:
  years: 2018, 2019
lastupdated: "2019-11-15"

keywords: mobile analytics, charts, app sessions, crashes, graph

subcollection:  mobilefoundation

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:download: .download}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}

# Build Custom Charts
{: #build_custom_charts}

The Custom Charts view in the {{site.data.keyword.mobileanalytics_short}} console provides you the flexibility to build your own visualizations around the analytics data that is captured and stored. The custom charts help in deepening insights further from what is provided out of the box or even extend it seamlessly into Business Analytics that is formulated around custom data.

In this view, you can choose any one of the supported analytics datasets and then select one of the supported chart types to plot the dataset. You may even further prune the visualization by defining filters to be applied over the data that is being plotted.  

Supported datasets are:
 * App Logs
 * App Sessions
 * Custom Data
 * Network Transactions

Supported chart types are:
 * Bar Graph
 * Flow Chart
 * Line Graph
 * Metric Group
 * Pie Chart
 * Table

The selection of the dataset, the chart type to plot, definition of the chart characteristics, and data filters to apply can be pulled together into a Custom Chart Definition and saved. You can create and save as many Custom Chart definitions as you need. Saved Custom Charts show up on the Custom Charts view with the relevant analytics data plotted.

## Creating a custom chart
{: #creating_custom_chart}

Create a custom chart by using the following steps:

1. Click the **Create Chart** button in the **Custom Charts** tab from the {{site.data.keyword.mobileanalytics_short}} dashboard.
1. In the **General Settings** tab, select **Chart Title**, **Event Type**, and the **Chart Type**.
1. On selecting the *Event Type* and *Chart Type*, the **Chart Definition** tab appears. Use the *Chart Definition* tab to define the chart for the specified chart type that you previously selected. After you define the chart, you can set the chart filters and chart properties.
1. **Chart Filters** are used to fine-tune the custom chart. More than one filter can be defined for a chart.
   For example, after you define a chart to visualize average app session duration if you wish to visualize this chart only for a specific app you can create a filter as follows:
   * Select **Application Name** for **Property**.
   * Select **Equals** for **Operator**.
   * Select the name of your app for **Value**.
   * Click **Add Filter**.
   The app name filter is added to the table of filters for your chart.
1. Chart properties are available for the **Table**, **Bar Graph**, and **Line Graph** chart types. The goal of chart properties is to enhance how the data is presented so that the visualization is more effective.
   If you created a **Table** chart, the chart properties are set to define the table page size, the field on which to sort, and the sort order of the field.
   If you created a **Bar Graph** or **Line Graph** chart, the chart properties are set to label threshold lines to add a frame of reference for anyone who is monitoring the chart.

## Obtaining custom insights from custom data logs
{: #creating_custom_chart_for_client_logs}    

If you wish to gain deeper custom insights such as users trail across the application then first you are required to capture the relevant user trail information such as page chosen or option that is selected or button clicked as custom data and log them. See the topic on [instrumenting your app](/docs/services/mobilefoundation?topic=mobilefoundation-instrument_your_app#instrument_your_app), on how to log custom data.

Next, create a custom chart definition with Custom Data as the EventType and choose a Chart Type. As you proceed to define  **Chart Properties** or **Chart Filters**, you notice the Custom Data types and values show up in the drop-down boxes. Make the relevant selections for the kind of insight you are looking for.  

The depth and usefulness of custom insights entirely depends on how effectively or relevantly the custom data in your applications is defined and captured.
{: note}

## Types of charts
{: #types_of_charts}

### Bar graph
{:  #bar_graph}

The bar graph allows for visualization of numeric data over an X-axis. When you define a bar graph, you must choose the value for X-Axis first. You can choose from the following possible values.

* **Timeline** - if you want to see your data as a trend (for example, average app session duration over time), choose *Timeline* for X-Axis.
* **Property** - choose Property if you want to see a count breakdown for the specific property. If you choose Property for X-Axis, then Total is implicitly chosen for Y-Axis. For example, choose *Property* for X-Axis and *Application Name* for *Property* to see a count for a specified event type, which is broken down by app name.

After you define a value for X-Axis, you can define a value for Y-Axis. If you choose *Timeline* for X-Axis, you can choose the following possible values for Y-Axis.

* **Average** - averages a numeric property in the supplied event type.
* **Total** - a total count of a property in the supplied event type.
* **Unique** - a unique count of a property in the supplied event type.

After you define the chart axes, you must choose a value for Property.

### Line graph
{:  #line_graph}

The line graph allows for the visualization of some metric over time. This type of chart is valuable when you want to visualize the data that is trending over time. The first value to define when you create a line graph is **Measure**, which has the following possible values:

* **Average** - averages a numeric property in the supplied event type.
* **Total** - a total count of a property in the supplied event type.
* **Unique** - a unique count of a property in the supplied event type.

After you define the measurement, you must choose a value for **Property**.

### Flow chart
{:  #flow_chart}

The flow chart allows for the visualization of flow breakdown of one property to another. For a flow chart, the following properties must be set.

* **Source** - the value of a source node in the diagram.
* **Destination** - the value of the destination node in the diagram.
* **Property** - a property value from either the source node or the destination node.

With the flow chart, you can see the density breakdown of various sources that flow to a destination, or the other way around. For example, if you want to see the breakdown of log severities for an app, you can define the following values.

* Select *Application Name* for **Source**.
* Select *Log Level* for **Destination**.
* Select the name of your app for **Property**.

### Metric group
{:  #metric_group}

The metric group can be used to visualize a single metric that is measured as either an average value, a total count, or a unique count. To define a metric group, you must define one of the following possible values for **Measure**.

* **Average** - averages a numeric property in the supplied event type.
* **Total** - a total count of a property in the supplied event type.
* **Unique** - a unique count of a property in the supplied event type.

After you define the measurement, you must choose a value for *Property*. This metric is displayed in the metric group.

### Pie chart
{:  #pie_chart}

The pie chart can be used to visualize the count breakdown of values for a particular property. For example, if you want to see a crash breakdown, define the following values.

* Select *App Session* for **Event Type**.
* Select *Pie Chart* for **Chart Type**.
* Select *Closed By* for **Property**.

The resulting pie chart shows the breakdown of app sessions that were closed by the user instead of app sessions, which were closed by a crash.

### Table
{:  #table}

The table is useful when you want to see the raw data. Building a table is as simple as adding columns for the raw data that you want to see.
Since not all properties are required for specific event types, null values can appear in your table. If you want to prevent these rows from appearing in your table, add an *Exists* filter for a specific property in the **Chart Filters** tab.
