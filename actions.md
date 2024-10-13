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

Here are a lot of DNSrule. I am not using them all
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

