# access_log table:

Find a list of bad IPs.

Look for bad IP addresses by volume of their activities, specifically - we use here a factor of 2.5 times of the average number of requests by IP address. You can also use median instead of average.

```SQL
SELECT
  remote_ip,
  COUNT(*) as cnt
FROM
  [oreilly-securit-ny-2017:bqlabs.lab2_data]
GROUP BY
  remote_ip
HAVING
  cnt > (
  SELECT
    AVG(cnt_per_req)
  FROM (
    SELECT
      remote_ip,
      COUNT(*) cnt_per_req
    FROM
      [oreilly-securit-ny-2017:bqlabs.lab2_data]
    GROUP BY
      remote_ip ) ) * 2.5
ORDER BY
  cnt DESC
```

Maybe there is a NAT? lets count distinct user-agents for the IP address:
```sql
SELECT remote_ip, COUNT(DISTINCT user_agent) as cnt_ips FROM [oreilly-securit-ny-2017:bqlabs.lab2_data]
GROUP BY remote_ip
ORDER BY cnt_ips DESC
```

Add UDF code to parse the query string and generate a JSON with the key-value pairs. We can then use JSON EXTRACT to get a specific value in future queries



UDF:
```
function explodeParamsFunc(row, emit) {
  url = row.path;
  if (url.indexOf('?') <= 0) return;
  var queryString = url.substring( url.indexOf('?') + 1 );

  var query = {};
  var pairs = (queryString[0] === '?' ? queryString.substr(1) : queryString).split('&');
  for (var i = 0; i < pairs.length; i++) {
    var pair = pairs[i].split('=');
    emit({param: decodeURIComponent(pair[0]), value: decodeURIComponent(pair[1] || '')});
  }
}

bigquery.defineFunction(
  'explodeParams',                           
  ['path'],                    
  [{'name': 'param', 'type': 'string'},  
   {'name': 'value', 'type': 'string'}],
  explodeParamsFunc
);
```

use it in an sql query:

```SQL
SELECT param, COUNT(DISTINCT value) FROM
explodeParams( SELECT path FROM [oreilly-securit-ny-2017:bqlabs.lab2_data] )
GROUP BY param
```
