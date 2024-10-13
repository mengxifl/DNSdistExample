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
