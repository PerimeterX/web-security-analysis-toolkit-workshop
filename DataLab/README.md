datalab
..* Browse to https://console.cloud.google.com
..* Open the webshell
..* datalab connect [YOURMACHINE]
..* Connect to your machine
..* Open notebook
..* Create a new notebook, name it oreillysec17
..* ```python
# Exectue the following command:
%%bq query
SELECT timestamp, activity FROM `oreilly-securit-ny-2017.wbeserver_logs.access_logs_data_2017_10` LIMIT 1000
```
