[Back to Homepage](https://linuxcloudadmin.github.io)

## Create swap using logical volume
- Use below commands to create swap volume from a new disk.

```
lvmdiskscan                             ##Scan for physical volumes
pvcreate /dev/sde                       ##Creates PV
vgcreate swapvg /dev/sde                ##Create a swap VG
lvcreate -L 20G -n swapvol swapvg       ##Create swap volume
lvcreate -l 5119 -n swapvol swapvg
mkswap /dev/swapvg/swapvol              ##set up a Linux swap area
swapon /dev/swapvg/swapvol              ##enable devices and files for paging and swapping
swapon -s                               ##Display swap summary
free -m 
dmsetup ls
```


[Back to Homepage](https://linuxcloudadmin.github.io)
