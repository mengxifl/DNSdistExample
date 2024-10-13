# cache
Adding cache can significantly improve the speed of responses. this is image
![image](https://github.com/user-attachments/assets/dd9b822a-6254-4886-8c41-f419607bb573)

to do that can use that
```
newServer( {address="8.8.8.8:53",name="google01",checkName="www.google.com",pool="google",maxCheckFailures=2 } )
packetCache = newPacketCache(10000, {})
getPool( 'google' ):setCache( packetCache )
```

web set: https://dnsdist.org/guides/cache.html?highlight=setcache
