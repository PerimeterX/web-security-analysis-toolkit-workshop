# Login abuse: Account Takeover (ATO)

Dataset: oreilly-securit-ny-2017:bqlabs.lab3_data

Challenge:
* ATO - show histogram of actions per minute
* ATO - find threshold per minute
* Find IP addresses above threshold
* Advanced assignment - include the headers as well

This data set also includes an "is_bad" column that you can use as a reference
* Compare to is_bad field (how?)


# Sample code

Examples for different percentile searches

-- 99 precentile of IP per minute doing a login
```sql
SELECT FLOOR((NTH(991, QUANTILES(c, 1001))))
   FROM (
  SELECT COUNT (*) c, ip, h_time, d_time,  m_time,
     FROM (
               SELECT remote_ip as ip, hour(timestamp) as h_time, day(timestamp) as d_time, minute(timestamp) as m_time,path
                 FROM [oreilly-securit-ny-2017:bqlabs.lab3_data]

             )

   GROUP BY ip, h_time, d_time, m_time
       )
```

-- 99 precentile of IP+UA per minute doing a login
```sql
SELECT FLOOR((NTH(991, QUANTILES(c, 1001))))
   FROM (
  SELECT COUNT (*) c, ua_ip, h_time, d_time,  m_time,
     FROM (
               SELECT hash(CONCAT(remote_ip, user_agent)) as ua_ip, hour(timestamp) as h_time, day(timestamp) as d_time, minute(timestamp) as m_time,path
                 FROM [oreilly-securit-ny-2017:bqlabs.lab3_data]

             )

   GROUP BY ua_ip, h_time, d_time, m_time
       )
```

```SQL
SELECT num_of_req_per_ua,f0_
FROM
(
   SELECT num_of_req_per_ua, ntile(101) over(order by frequency desc)
     FROM (        
           SELECT num_of_req_per_ua, count(*) as frequency
           FROM (
                 SELECT user_agent ,count(*) as num_of_req_per_ua
                 FROM (
                        SELECT user_agent, hour(timestamp) as h_time, day(timestamp) as d_time, minute(timestamp) as m_time,
                         FROM [oreilly-securit-ny-2017:bqlabs.lab3_data]
                           GROUP by 1,2,3,4
                       )
                   GROUP BY 1
                 )
                 GROUP BY 1
                 )
)
WHERE f0_ = 90
```

using the UDF from previous lab - you can also look at the referrer header:
```SQL
SELECT param, COUNT(DISTINCT value) FROM
explodeParams( SELECT header_referer as path FROM [oreilly-securit-ny-2017:bqlabs.lab3_data] )
GROUP BY param
```
