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
