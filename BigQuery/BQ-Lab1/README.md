# access_log table:
-- Take the top IPs that hited the site 2.5 times more than the average per IP
```SQL
SELECT
  remote_ip,
  COUNT(*) as cnt
FROM
  [oreilly-securit-ny-2017:wbeserver_logs.access_sample]
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
      [oreilly-securit-ny-2017:wbeserver_logs.access_sample]
    GROUP BY
      remote_ip ) ) * 2.5
ORDER BY
  cnt DESC
```


# LOGIN ATO
-- 99 precentile of UA per minute doing a login
```sql
SELECT c, QUANTILES(c, 10)
   FROM (
  SELECT COUNT (*) c, user_agent, h_time, d_time,  m_time,
     FROM (
               SELECT user_agent, hour(timestamp) as h_time, day(timestamp) as d_time, minute(timestamp) as m_time,path
                 FROM [oreilly-securit-ny-2017:login_logs.sample_1]
             )
   GROUP BY user_agent, h_time, d_time, m_time
       )
```

-- 99 precentile of IP per minute doing a login
```sql
SELECT FLOOR((NTH(991, QUANTILES(c, 1001))))
   FROM (
  SELECT COUNT (*) c, ip, h_time, d_time,  m_time,
     FROM (
               SELECT remote_ip as ip, hour(timestamp) as h_time, day(timestamp) as d_time, minute(timestamp) as m_time,path
                 FROM [oreilly-securit-ny-2017:login_logs.sample_1]

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
                 FROM [oreilly-securit-ny-2017:login_logs.sample_1]

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
                         FROM [oreilly-securit-ny-2017:login_logs.sample_1]
                           GROUP by 1,2,3,4
                       )
                   GROUP BY 1
                 )
                 GROUP BY 1
                 )
)
WHERE f0_ = 90
```
