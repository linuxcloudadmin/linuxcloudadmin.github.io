[Back to Homepage](https://linuxcloudadmin.github.io)

## Linux Multipath
- Linux multiptah commands

```
echo 1> /sys/class/fc_host/host3/issue_lip          ##Loop Initialization Protocol (LIP) a bus reset and scans the scsi layer
	
echo "- - -" > /sys/class/scsi_host/host3/scan      ##rescan the device path

multipath -r                                        ##reload

multipath -f                                        ## flush a multipath device map specified as parameter, use this alone to remove any unused Luns.
                                         										
multipath -ll |grep dm-  (or) multipath -ll |grep mpath    ##shows multipaths
```

- Delete multipath devices

**Error:**

```
sdat: checker msg is "tur checker reports path is down"
sdax: checker msg is "tur checker reports path is down"
```

**Solution:** 
After verifying disk paths are removed due to a planned activity, run the below command.

```
echo 1 > /sys/block/$i/device/delete	
						 
multipath -ll|grep -i failed 
	
for i in `multipath -ll|grep -i failed| awk '{print $3}' `; do  echo $i; echo 1 > /sys/block/$i/device/delete ;done 
		
for i in `cat /tmp/list `; do  echo $i; echo 1 > /sys/block/$i/device/delete ;done 
```


[Back to Homepage](https://linuxcloudadmin.github.io)
