# web and monitor

here are a lot of functions you can perform using the web API. here is web site: https://dnsdist.org/guides/webserver.html

example
```
webserver("0.0.0.0:8084")
setWebserverConfig({apiKey="m@gict@vern", password="p@ssw@rd",  acl="0.0.0.0/0"})
```

then you can use your web brower to access with <dnsdist_host_id>:8084 to login , user name is m@gict@vern, password is p@ssw@rd

set promethus to scrape data


```
- job_name: dns
  honor_timestamps: true
  scrape_interval: 10s
  scrape_timeout: 10s
  metrics_path: /metrics
  scheme: http
  basic_auth:
    username: m@gict@vern
    password: p@ssw@rd
  static_configs:
  - targets:
    - <dnsdist_host_id>:8084
```


