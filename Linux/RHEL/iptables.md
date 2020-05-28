[Back to Homepage](https://linuxcloudadmin.github.io)

## iptables

- Here are the iptable basic commands.

```
iptables -n -L -v --line-numbers                        ##list, numbers, verbose and line numbers 
iptables -F                                             ##deleting iptables 
iptables -L INPUT -n --line-numbers                     ##list only input rules 
iptables -D INPUT 5                                     ##delete input line 5 
iptables -I INPUT 5 -s ipaddress -j DROP                ##input a new line 
iptables-save
```


[Back to Homepage](https://linuxcloudadmin.github.io)
