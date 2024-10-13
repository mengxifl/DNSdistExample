# addResponseAction

addResponseAction is an action. This action occurs after the upstream DNS response but before dnsdist responds to the client.

web site : https://dnsdist.org/reference/rules-management.html?highlight=addresponseaction#addResponseAction

I am not using them all , but the logic is the same

addResponseAction basic use is:
```
addResponseAction(DNSrule, action[, options])
```

## DNSRule
The DNSRule we discussed in detail can be found here: https://github.com/mengxifl/DNSdistExample/blob/main/addActions.md#detail-the-rule-selector.


## DNSRule
actions most same as this actions: https://github.com/mengxifl/DNSdistExample/blob/main/addActions.md#actions

but lua action is not same the addResponseAction require use LuaResponseAction

this is a example

```
  addResponseAction(
    rule,
    LuaResponseAction(
      function (dr)
        -- print( 'log' )
        -- print( duration_ms )
        print(
          '{'
            .. '"date": "'       .. os.date("%Y-%m-%d %H:%M:%S") .. '", '
            .. '"qureyName": "'  .. dr.qname:toString()          .. '", '
            .. '"remoteIP": "'   .. dr.remoteaddr:toString()     .. '"'
          .. '}'
        )
        return DNSResponseAction.None
      end
    )
  )
```

## addResponseAction

addResponseAction that parms of functions is dr

that dr struct and functions you can found there https://dnsdist.org/reference/dq.html?highlight=dq#dnsresponse-object

return a DNSResponseAction  web site is: https://dnsdist.org/reference/constants.html#dnsresponseaction

example 

```
  addResponseAction(
    AllRule(),
    LuaResponseAction(
      function (dr)
        -- print( 'log' )
        -- print( duration_ms )
        print(
          '{'
            .. '"date": "'       .. os.date("%Y-%m-%d %H:%M:%S") .. '", '
            .. '"qureyName": "'  .. dr.qname:toString()          .. '", '
            .. '"remoteIP": "'   .. dr.remoteaddr:toString()     .. '"'
          .. '}'
        )
        return DNSResponseAction.None
      end
    )
  )
```

that example will print client require log time and require.

but some log will not print. beacuse This action occurs after the upstream DNS response but before dnsdist responds to the client.


