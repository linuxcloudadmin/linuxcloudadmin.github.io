[Back to Homepage](https://linuxcloudadmin.github.io)

## ITM Agents
- This sectio is for handling ITM monitoring agent, a product of IBM.

- universal agent UM,  os agents ux and lz.

```
ps -ef | grep itm | grep ux
/etc/rc.d/init.d/itm start
/etc/rc.d/init.d/itm stop
/etc/rc.d/init.d/itm status		
```

- itm installation path will vary based on customer environment. Use "find" or "locate" command to find the path.
	
```
/usr/libexec/itm/bin/cinfo -r

/usr/libexec/itm/bin/itmcmd agent stop

/usr/libexec/itm/bin/itmcmd agent stop ux		

/usr/libexec/itm/bin/itmcmd agent -f stop ux

/usr/libexec/itm/bin/itmcmd agent start ux

/usr/libexec/itm/bin/itmcmd agent -p <instance-name> stop um		##If we have other instance of universal agent running

/usr/libexec/itm/bin/itmcmd agent -p <instance-name> start um

/usr/libexec/itm/itmcmd agent -p CANDLEHOME start all
```

- Some commands

```
cinfo -i
cinfo -r
cinfo -c <pc>
cinfo -v
cinfo -t
Typing cinfo -? displays this help:
cinfo -?
cinfo [-h candle_directory] [-c product] [-i] [-r] [-s product] [-R] [-v] [-t]

-c <product> Displays configuration prompts and values
-i Displays an inventory of installed products
-r Shows running processes
-s <product> Displays configuration parameters and settings
-R Shows running processes, after updating a tracking database
-v Shows the installed CD versions in this CandleHome
-t Shows the product name, version, build info and install date
```


[Back to Homepage](https://linuxcloudadmin.github.io)
