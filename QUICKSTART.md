# Rubrik Mosaic Splunk Integration Quick Start Guide

## Installation

The repository contains three files, these can be used for installation of the application and add-on to a Splunk server:

* `TA-rubrik-mosaic-splunk-add-on-0.0.1.spl` - a packaged version of the add-on code
* `TA-rubrik-mosaic-splunk-add-on-0.0.1_export.tgz` - an exported copy of the add-on code
* `rubrik-mosaic.spl` - a packaged version of the application

The add-on contains Python scripts which query the Mosaic REST API for data, the application contains the definition of dashboards which can be used to graph this data once imported.

### Installation for modification, or inspection

When installing for modification or inspection of the underlying workings of the add-on we will use the following files:

* `TA-rubrik-mosaic-splunk-add-on-0.0.1_export.tgz`
* `rubrik-mosaic.spl`

### Installation for demo or production purposes

* `TA-rubrik-mosaic-splunk-add-on-0.0.1.spl`
* `rubrik-mosaic.spl`

### Setting up data inputs

You will create two new inputs for each Mosaic cluster. From within the Rubrik Mosaic Splunk
Add-On, click **Inputs**, then **Create New Input**.

| **Input Type** | Mosaic - Alerts |
| --- | --- |
| **Name** | mosaic\_events |
| **Interval** | 300 |
| **Index** | main |
| **Global Account** | \<mosaic_global_account\> |
| **Cluster Name** | \<cluster_friendly_name\> |
| **Mosaic Cluster IP** | \<cluster_ip_address\> |
| **Mosaic Cluster Port** | 9090 |
| **Validate SSL Certificates** | \<true/false\> |

| **Input Type** | Mosaic - Cluster Status |
| --- | --- |
| **Name** | mosaic\_cluster\_status |
| **Interval** | 180 |
| **Index** | main |
| **Global Account** | \<mosaic_global_account\> |
| **Cluster Name** | \<cluster_friendly_name\> |
| **Mosaic Cluster IP** | \<cluster_ip_address\> |
| **Mosaic Cluster Port** | 9090 |
| **Validate SSL Certificates** | \<true/false\> |

### Configuring Datasets

The Rubrik Mosaic add-on can be used to create new tables from gathered data. Two new datasets will be created to be used by the predefined dashboards.

Install this add-on before continuing if it is not already present.

You will use the search strings below to create each dataset. To create
a new dataset, click on **Apps** → **Rubrik Mosaic**, then **Create New Table
Dataset**.


Click on **Search (Advanced)**, paste in the search string, and click the green search button. This will display a list of available fields along with search results. Select the fields specified with the search string and click **Done**. Note that some fields, like **\_time** and **\_raw**, may be auto-selected for you. Click **Done**, and a preview of the dataset will be displayed. Click **Save As**, input the table
title and table ID, and click **Save**. Below are the steps to carry out for each dataset.

1. Perform a search using the Search String, select the desired fields,
and click **Done**.
1. Preview the dataset and click **Save As**.
1. Provide the **Table Title** and **Table ID** for the dataset and
click **Save.**
1. Click **Done**.

Use these values to create the datasets based on the
instructions above:

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Mosaic - Alerts</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>sourcetype="mosaic:alerts" | dedup eventId | eval _time = strptime(timestamp, "%a %b %-d %H:%M:%S %Y")</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>mosaic_dataset_alerts</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
category<br>
clusterName<br>
eventId<br>
eventType<br>
message<br>
policy</td>
schedule</td>
eventSource</td>
sourceMgmtObject</td>
store</td>
summary</td>
</tr>
</tbody>
</table>

<table width="100%">
<tbody>
<tr class="odd">
<td><strong>Table Title</strong></td>
<td>Mosaic - Cluster Status</td>
</tr>
<tr>
<td><strong>Search String</strong></td>
<td>sourcetype="mosaic:status" |  where clusterStatus != "null"</td>
</tr>
<tr class="even">
<td><strong>Table ID</strong></td>
<td>mosaic_dataset_cluster_status</td>
</tr>
<tr class="odd">
<td><strong>Fields</strong></td>
<td>_time<br>
clusterStatus<br>
clusterStatusMsg<br>
clusterName<br>
datosPrimaryIp<br>
nodeCount<br>
unhealthyNodeCount<br>
</tr>
</tbody>
</table>

