# Rubrik Mosaic Splunk Integration Quick Start Guide

## Introduction to the Rubrik Mosaic Add-On for Splunk

Rubrik Mosaic’s API first architecture allows for integration with a wide array of monitoring and visibility tools. This quick start guide will provide everything needed to begin streaming metrics from Rubrik Mosaic into Splunk, allowing you to consume dashboards, alerts, and analytics in an easy to use web interface. Data is ingested by Splunk via Mosaic's REST API and analyzed by the Splunk platform, presenting the following information:

* Notifications
* System operability and performance

The Rubrik Mosaic Add-On for Splunk is comprised of two pieces: the Rubrik Splunk Add-On, available on [Splunkbase](https://splunkbase.splunk.com/app/4636/), and the Rubrik Mosaic Splunk Application, available in this repository. The add-on performs the heavy lifting of communicating with the Rubrik REST API endpoint to gather metrics, while the application contains the dashboards and other logic used for visualization in Splunk.

## Installing the Rubrik Mosaic Add-On for Splunk

This section details the steps required to install all necessary
components for the Rubrik Mosaic Add-On for Splunk.

### Prerequisites

* Splunk 7.0 or greater
* [Splunk Datasets Add-On](https://splunkbase.splunk.com/app/3245/)

## Installing the Rubrik Mosaic Splunk Add-On

Search for the Rubrik Mosaic Splunk Add-On by clicking on **Apps** → **Find More Apps** in your Splunk GUI, or browse to `http://<Your Splunk Server>:8000/en-US/manager/system/appsremote`.
Perform a search for “Rubrik”, and the Rubrik Mosaic Splunk Add-On will be displayed in the results. Click ‘Install’, and complete the displayed dialog.

After installation is complete, you will be prompted to restart your Splunk server. Click **Restart Now** or **Restart Later** depending on your preference. Continue with the installation steps once the restart is complete.

After logging back into the Splunk server, you will see the **Rubrik Mosaic
Splunk Add-On** listed in the Apps menu.

## Installing the Rubrik Mosaic Splunk Application

Download the Rubrik Mosaic Splunk Application file (named `rubrik-mosaic.spl`) from the `app/packaged` folder in the root of this repository. Click on Apps → Manage Apps in your Splunk GUI, then click Install app from file.

Click Choose File, browse to the downloaded file and select Upload. A notification will be displayed near the top of the GUI once the application is successfully installed. Notice that the Rubrik Application is now displayed in the App menu, along with the Rubrik Mosaic Splunk Add-On. Installation of all required components is now complete.

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

## Upgrading the Rubrik Mosaic Add-On for Splunk

Upgrades for the Rubrik Mosaic Splunk Add-On can be performed directly through the Splunk GUI by clicking on Apps → Manage Apps, and clicking Update for the Rubrik Mosaic Splunk Add-On.

Upgrades for the Rubrik Msoaic Splunk Application will be available on the GitHub Repo. After downloading an upgrade, install it by browsing to Apps → Manage Apps in your Splunk GUI, then click Install app from file. Click Choose File, browse to the downloaded file, ensure that the Upgrade App box is checked and click Upload.

Review the inputs and datasets section above, comparing them against the current configuration to identify changes in fields, queries, and new datasets and inputs.

## Usage

There are two dashboards included with the Rubrik Add-On for Splunk:

* **Mosaic - Status Dashboard:** displays the current operational state of the Rubrik Mosaic cluster.
* **Mosaic - Alerts Dashboard:** displays historic alert information for activities on the Mosaic cluster.
