# Example
## The most basic command
*Here, I am using a container to run DNSDist. Therefore, there is only a command to run DNSDist in the foreground. If you want it to run in the background, you need to write a service file. However, here I am only talking about DNSDist*

To run dnsdist in the foreground 
```
dnsdist --supervised --disable-syslog -C <ENTRY_FILE>
```
Here is a *ENTRY_FILE*

ENTRY_FILE: This is a lua script file. Beacuse dnsdist use lua script as configure file. We will talk it Later.

For test this ENTRY_FILE command is

```
dnsdist -C <ENTRY_FILE> --check-config
```

## most basic config
**DNSDist uses Lua scripts. However, we won't discuss Lua grammar here.**

### Create a upstream server
https://dnsdist.org/reference/config.html?highlight=newserver#newServer
```
newServer( {address="8.8.8.8",name="google00",checkName="www.google.com",pool="google",maxCheckFailures=2 } )
newServer( {address="8.8.4.4",name="google01",checkName="www.google.com",pool="google",maxCheckFailures=2 } )
```
Here you create an upstream 8.8.8.8 name google00 and create a pool name *google*  and add 8.8.8.8 to this pool
Then  you create an upstream 8.8.4.4 name google00 and create a pool name *google*  and add 8.8.8.8 to this pool


### Listen on a port to accept requests
https://dnsdist.org/reference/config.html?highlight=newserver#addLocal
```
addLocal('0.0.0.0:53', { reusePort=true })
```
Listen on port 53 of all local IP addresses

### Provide a network segment that can use this DNS
https://dnsdist.org/reference/config.html?highlight=newserver#addACL
```
addACL("0.0.0.0/0")
```
Set any Ip can access this dns


### 
