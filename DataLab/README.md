# Datalab

## Prerequisites

1. Install the datalab Cloud SDK component
2. Create your own datalab instance by following these instructions: https://cloud.google.com/datalab/docs/quickstarts

## Test that it is working

* Browse to https://console.cloud.google.com
* Open the webshell
* datalab connect [YOURMACHINE]
* Connect to your machine
* Open notebook
* Create a new notebook, name it oreillysec17
```python
import google.datalab.bigquery as bq
```
click the run button on the top

add the following command next (it is important that the %%bq command will be first) and run it
```python
%%bq query
SELECT timestamp, remote_ip FROM `oreilly-securit-ny-2017.bqlabs.lab2_data` LIMIT 1000
```
