# Use external resources

import relevant libraries:
```python
import socket
import google.datalab.bigquery as bq
```

Get list of requests from fake Googlebot:
```python
get_ipython().run_cell_magic(u'bq', u'query -n goodbots', u"SELECT remote_ip FROM `oreilly-securit-ny-2017.bqlabs.lab1_data`\nWHERE user_agent LIKE 'Googlebot%' --AND is_fake = true\nGROUP BY remote_ip\nLIMIT 100")
```

print the first 5 to test it worked:
```python
get_ipython().run_cell_magic(u'bq', u'sample --query goodbots --count 5', u'')
```

Validate the the `remote_ip` reverse DNS lookup resolves to a hostname ending with `googlebot.com`, otherwise - add it to the list of fake googlebots
```python
goodbots_df = goodbots.execute(output_options=bq.QueryOutput.dataframe()).result()

fakes = []

for addr in goodbots_df['remote_ip']:
  try:
    host = socket.gethostbyaddr(addr)[0]
    if (not host.endswith('googlebot.com')):
      fakes.append([addr, host])
  except:
    fakes.append([addr, 'none'])
```

print the results

```python
print fakes
```
