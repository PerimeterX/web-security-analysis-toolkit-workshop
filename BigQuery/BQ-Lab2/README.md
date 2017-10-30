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
