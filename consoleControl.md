## use console to control

The reason I mention this at the end is because these configurations can currently only be stored in memory, and I don't know how to persist them.

Anything you can get or set, you can do using the API

this is web site: https://dnsdist.org/guides/console.html

Enable console control and set only 127.0.0.1/32 can only use console control
```

controlSocket('0.0.0.0:5199')
setConsoleACL('127.0.0.1/32')
setKey("XXXXXXX")

```
XXXXXXX is a rand string  you can use print( makeKey() ) to get the sting
Script is
```
echo 'makeKey()' > /createKey.lua
--supervised --disable-syslog -C /createKey.lua
```
Then you can get XXXXXXX

Use console
```
dnsdist -c 127.0.0.1:5199 -k 'XXXXXXX'
Unable to read configuration from '/etc/dnsdist.conf'
> topSlow()
   1  mask.apple-dns.net.                        37  5.2%
   .......
>
```

