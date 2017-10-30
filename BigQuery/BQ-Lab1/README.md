# Good bot detection

Look for known bots by their user-agent header, and confirm that their remote_ip is of the actual service.
for instance - bingbot coming from Microsoft, googlebot from Google

Bingbot:
```SQL
SELECT remote_ip, user_agent, remote_ip_classification
FROM
  [oreilly-securit-ny-2017:bqlabs.lab1_data]
where
  user_agent LIKE "%bingbot%" AND
  remote_ip_classification NOT LIKE "%Microsoft%"
limit 5
```

Googlebot:
```SQL
SELECT remote_ip, user_agent, remote_ip_classification
FROM
  [oreilly-securit-ny-2017:bqlabs.lab1_data]
where
  user_agent LIKE "%bingbot%" AND
  remote_ip_classification NOT LIKE "%Microsoft%"
limit 5
```
