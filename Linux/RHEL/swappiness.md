## Swappiness

- Why the SWAP is not utilized though normal usage + Memory with file system cache crossed 90%. Can you please explain us little more in detail ?

> In linux, any unused resource means a wasted resource. So OS will try to use the resources to the fullest. That is the reason free memory is used for caching purpose. In this server, Swap will be used only when 90% of memory is used by the processes as per the configuration. The FS cache usage is not taken into account as memory usage because, it is not allocated for any process, the FS cache is used by the OS only, which will be released and reused whenever memory  is needed. 

- FS cache memory is used on LUN (which over the network) and Cache usage is more.  Can you please let us know when this cache usage will be released and on what technical criteria this will be cleared ?

> The cache will be released automatically when the OS needs more memory for existing process / for staring new process. The  application data lies in the network filesystem. The access of data via N/W will be slow. To speed up access a portion of FS data is cached in the memory which is virtual, which will increase the app performance.

- Is there any assurance from Linux support team side stating the Memory usage will utilize the available swap when it reaches 90% of the actual allocated.

> The respective parameter is already set in the OS to use swap when 90% of memory is utilised. (i.e., when only 10% memory is free). That's the way the OS is designed.


### Changing Swappiness value
- Higher the value higher the swapping will be. 
- Default value is 60. When the value is 60, after 40 percent of RAM usage, swap will be used.

```
cat /proc/sys/vm/swappiness	            ##displays swappiness value. Default value is 60

echo 10 > /proc/sys/vm/swappiness       ##sets swappiness on the fly

sysctl -w vm.swappiness=10              ##sets swappiness

Permanent changes are made in /etc/sysctl.conf

echo "vm.swappiness = 10" >> /etc/sysctl.conf

vm.vfs_cache_pressure = 50
```
