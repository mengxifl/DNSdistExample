# DNSAction
fhis is core feature of dnsdist. We need to discuss this in detail.

first This is website
https://dnsdist.org/reference/rules-management.html?highlight=addaction#addAction

When a DNS query is coming in, dnsdist will check if the query matches any DNSActions. If it matches, then it will do something. 
You can set a DNSAction to run after a match and check other DNSActions if needed.

we had set a basic configuration and use the action.
```
addAction( AllRule(), PoolAction("google") )
```

expression:
```
addAction(DNSrule, action[, options])
```
DNSrule: This is a condition, indicating that when it is met, the action will be executed. It is called a rule selector.

## detail the rule selector
web site: https://dnsdist.org/reference/selectors.html#rule-selectors

Here are a lot of DNSrule. I am not using them all , but the logic is the same
Here are some DNSRules that I think can resolve my issue.


1、all rule: [https://dnsdist.org/reference/selectors.html#AllRule](https://dnsdist.org/reference/selectors.html#AllRule)
```
addAction( AllRule(), PoolAction("google") )
```

2、QNameRule [https://dnsdist.org/reference/selectors.html#QNameRule](https://dnsdist.org/reference/selectors.html#QNameRule)
```
addAction( QNameRule("www.google.com"), PoolAction("google") )
```
This means if the query domain name is www.google.com, then use the Google pool to resolve it.

3、RegexRule [https://dnsdist.org/reference/selectors.html#NetmaskGroupRule](https://dnsdist.org/reference/selectors.html#RegexRule)
```
addAction( RegexRule(".*\\.a\\.b\\.c\\.com"), PoolAction("google") )
```
This means if the query is match  Regex .*\\.a\\.b\\.c\\.com, then use the Google pool to resolve it.

4、NetmaskGroupRule  [https://dnsdist.org/reference/selectors.html#NetmaskGroupRule](https://dnsdist.org/reference/selectors.html#NetmaskGroupRule)
```
addAction( NetmaskGroupRule("192.168.1.200/32"), PoolAction("google") )
```
This means if the query from host 192.168.1.200/32 net stagment , then use the Google pool to resolve it.

5、QTypeRule [ https://dnsdist.org/reference/selectors.html#RegexRule](https://dnsdist.org/reference/selectors.html#RegexRule)
```
addAction(QTypeRule(DNSQType.AAAA), PoolAction("google"))
```
This means if the query domain name is match type AAAA, then use the Google pool to resolve it.


6、LuaRule [https://dnsdist.org/reference/selectors.html#AndRule](https://dnsdist.org/reference/selectors.html#AndRule)
```
addAction(LuaRule(luaRuleTest), PoolAction("google"))

function luaRuleTest(dq)
  return true
end
```

7、 combine rules  [combine rules ](https://dnsdist.org/reference/selectors.html#combining-rules)
andRule:
```
  addAction(
    AndRule{
      QNameRule("www.google.com"),
      NetmaskGroupRule("172.16.2.30/32")
    },
    PoolAction( "google" )
  )
```

orRule:
```
  addAction(
    OrRule{
      QNameRule("www.google.com"),
      NetmaskGroupRule("172.16.2.2/32")
    },
    PoolAction( "google" )
  )
```

notRule
```
  addAction(
    NotRule{
      QNameRule("www.google.com"),
      NetmaskGroupRule("172.16.2.2/32")
    },
    PoolAction( "google" )
  )
```

combine rule
```
  addAction(
    AndRule{
      QNameRule("www.google.com"),
      NetmaskGroupRule("172.16.2.2/32"),
      NotRule{NetmaskGroupRule("172.16.2.2/32"), NetmaskGroupRule("172.16.2.3/32")}
    },
    PoolAction( "google" )
  )
```

### actions
As you can see, all the above examples query from the Google pool. This is not what we want.

Then we must change the Action
web site : [https://dnsdist.org/reference/actions.html](https://dnsdist.org/reference/actions.html)

as Rules
Here are a lot of Action. I am not using them all , but the logic is the same.
Here are some Action that I think can resolve my issue.

1、PoolAction [https://dnsdist.org/reference/actions.html#PoolAction](https://dnsdist.org/reference/actions.html#PoolAction)
```
addAction( QNameRule("www.google.com"), PoolAction("google") )
```

2、NegativeAndSOAAction  [https://dnsdist.org/reference/actions.html#NegativeAndSOAAction](https://dnsdist.org/reference/actions.html#NegativeAndSOAAction)
```
addAction(RegexRule(".*\\.example\\.com"), NegativeAndSOAAction(true, "example.com", 3600, "ns1.example.com", "hostmaster.example.com", 2023100101, 3600, 900, 604800, 86400))
```
tip: Setting an SOA record in dnsdist is not recommended; it should only be done in special cases.


3、SpoofAction  [https://dnsdist.org/reference/actions.html#SpoofAction](https://dnsdist.org/reference/actions.html#SpoofAction)
```
addAction("www.dist-test.com", SpoofAction({"1.1.1.1"}))
```
tip: Setting an A record in dnsdist should only be done according to your environment.

4、LuaAction  https://dnsdist.org/reference/actions.html#LuaAction
```
  addAction(
    AndRule{
      QNameRule('www.dist-test.com'),
      makeRule("172.16.2.2/32")
    },
    LuaAction(
      function (dq)
        print(dq)
        return DNSAction.Pool, 'google'
      end
    )
```
tip: This action is very important as it allows you to write Lua scripts to process DNS requests.


5、RCodeAction https://dnsdist.org/reference/actions.html#RCodeAction
```
addAction(QTypeRule(DNSQType.AAAA), RCodeAction(DNSRCode.NOERROR))
```
turn DNSRCode to some code
code is here : https://dnsdist.org/reference/constants.html#rcode

The codes and their functions can be found here: 
https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml#dns-parameters-6


### LuaAction
Using LuaAction, we can do many things that we want to accomplish.
LuaAction: https://dnsdist.org/reference/actions.html#LuaAction

example:
```
  addAction(  AllRule(),
    LuaAction(
      function (dq)
        print(dq)
        return DNSAction.Pool, 'google'
      end
    )
  )
```
The function has a parameter called dq, which refers to the DNSQuestion object. More information can be found here: https://dnsdist.org/reference/dq.html?highlight=dq#the-dnsquestion-dq-object.
You can use dq functions and this construct.

such as:
```
  addAction( QNameRule('www.dist-test.com'),
    LuaAction(
      function (dq)
        print(dq.qname)
        dq:spoof({ newCA('1.1.1.1') })
        return DNSAction.SpoofRaw
      end
    )
  )
```
When a client queries the domain name www.dist-test.com, dnsdist will print 'www.dist-test.com' and then tell the client that the IP address for www.dist-test.com is 1.1.1.1.


LuaAction had a special function is
setRestartable  https://dnsdist.org/reference/dq.html?highlight=dq#DNSQuestion:setRestartable
that will allow you use lua script control query. that how the action will do when query is faild


Last luaAction requre return 
https://dnsdist.org/reference/constants.html#dnsaction

example
```
  addAction( QNameRule('www.dist-test.com'),
    LuaAction(
      function (dq)
        -- this is must retrun DNSAction
        return DNSAction.None
      end
    )
  )
```



