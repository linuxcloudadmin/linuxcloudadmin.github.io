[Back to Homepage](https://linuxcloudadmin.github.io)

## Patching
- Linux server must be patched in regular intervals to keep the system updated and to avoid any vulnerability.

### Prechecks

- Planning and performing prechecks are important for a successful patching.
- Before reboot or patching, run the below command to take required output and save the console logs.

```
script -c 'hostname;uname -a;cat /etc/redhat-release;dmidecode -t 1  | grep "Manufacturer:";echo "BIT";getconf LONG_BIT;echo "Processor";grep -c processor /proc/cpuinfo;echo "Memory";free -m;echo "mountpoints";df -hPT; df -hPT | wc -l;echo "mounts";mount | wc -l;echo "cat /proc/mounts ";cat /proc/mounts | wc -l;echo "/etc/fstab";cat /etc/fstab | wc -l;echo "/etc/grub.conf";cat /etc/grub.conf;ls -lrt /boot;echo "pvs output";pvs;echo "VGS output";vgs;echo "lvs output";lvs;echo "SELINUX";getenforce -a;echo "INTERFACES";ifconfig -a;lsblk -i;lvmdiskscan;multipath -ll;' /tmp/precheck.log
```

- Verify the root password or Change it if required. We might need it in case of any issues after patching.
- Check whether the server has cluster setup. If so, we need to perform different steps.

```
clustat;pcs status
```

- Verify the repository list and add or remove wanted or unwanted channels.

```
yum repolist
```

- For SAN or iscsi disks add **_netdev** in fstab. We need this because LVM is started before network initialisation is complete, thus the iscsi / SAN disks will not detected by the OS. 
- We use the “_netdev” mount option to instruct the system to delay the mount attempt until network initialisation.
  
```
multipath -ll

# vi /etc/fstab
LABEL=data /data ext4 _netdev 1 2
```

### OS patching

- Take necessary prechecks.
- Then run, yum update --exclude=pkg1,pkg2,pkg3
- Reboot the server.
- Check the UNIX functionalities.

- Find last patched date

```
rpm -qa --last | grep kernel | awk 'NR==1{print $3,$4,$5}'
```



### Patching rollback

- It is not recommended to rollback tha patches. It is advised to restore snapshot backup for virtual servers.
- But in certain cases we can use below command for rollback.

```
yum-complete-transaction
yum history undo 51 --exclude=selinux*,vmware*,kexec*
reboot
yum update 
reboot
```

### Download the patches

```
/usr/bin/yum update --assumeyes --downloadonly --exclude=pkg1,pkg2,pkg3
```

[Back to Homepage](https://linuxcloudadmin.github.io)
