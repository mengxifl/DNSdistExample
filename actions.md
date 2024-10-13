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


1、all rule: https://dnsdist.org/reference/selectors.html#AllRule
```
addAction( AllRule(), PoolAction("google") )
```


