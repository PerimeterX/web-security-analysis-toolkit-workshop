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
# Exectue the following command:
%%bq query
SELECT timestamp, activity FROM `oreilly-securit-ny-2017.bqlabs.lab1_data` LIMIT 1000
```
