# What is DNSDist and why I need use DNSDist

## what is DNSDist

dnsdist is a highly DNS-, DoS- and abuse-aware loadbalancer. Its goal in life is to route traffic to the best server, delivering top performance to legitimate users while shunting or blocking abusive traffic.

dnsdist is dynamic, its configuration language is Lua and it can be changed at runtime, and its statistics can be queried from a console-like interface or an HTTP API.

here is dnsdist web site https://dnsdist.org

## why I  need use this

In my Administrative Network area. There are a lot of issues

1、The speed of accessing networks in different regions heavily depends on the DNS servers corresponding to those regions

2、Requests from different network segments for different domain names will yield different results.

3、To reduce network demand, we have some caching servers, such as using SNIProxy, to cache data and save bandwidth.

4、Other special requirements

5、monitor DNS itself and Upstream DNS status monitor and we require access to DNS logs to send to Elasticsearch (ES).

*If you do not have the aforementioned requirements I mentioned. Then you won't use DNSDist*

## Basic  usage 
[https://github.com/mengxifl/DNSdistExample/blob/main/base.md](https://github.com/mengxifl/DNSdistExample/blob/main/base.md)

## addActions  
An action performed after the client initiates a request but before dnsdist forwards the request to the upstream pool
[https://github.com/mengxifl/DNSdistExample/blob/main/base.md](https://github.com/mengxifl/DNSdistExample/blob/main/addActions.md)
