## use console to control

The reason I mention this at the end is because these configurations can currently only be stored in memory, and I don't know how to persist them.

this is web site: https://dnsdist.org/guides/console.html

Enable console control and set only 127.0.0.1/32 can only use console control
```
controlSocket('0.0.0.0:5199')
setConsoleACL('127.0.0.1/32')
```

