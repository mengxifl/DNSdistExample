# web and monitor

here are a lot of functions you can perform using the web API. here is web site: https://dnsdist.org/guides/webserver.html

example
```
webserver("0.0.0.0:8084")
setWebserverConfig({apiKey="m@gict@vern", password="p@ssw@rd",  acl="0.0.0.0/0"})
```

then you can use your web brower to access with <dnsdist_host_id>:8084 to login , user name is m@gict@vern, password is p@ssw@rd

## monitor
after you set that you can use pormetheus to scrape data 
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
## web api 
use api to edit rules web site is : https://dnsdist.org/guides/webserver.html#dnsdist-api

here are a lot of example that you can test. Test is important 


## register web handler 
this example will regist a path to get some data from dnsdist. You can use that run other lua script
access http://<dnsdist_host_id>:8084/getcache  that will return a json ["1", "2"]
```
registerWebHandler(
  `/getcache`,
  function(req, resp)
    resp.status = 200
    resp.body = '["1","2"]'
  end
)
```
