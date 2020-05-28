## NFS - Installing and configuring new NFS server
- This section covers installing and configuring a NFS in a centos 6 server.
- For nfs, portmap or rpcbind should be running for allocating ports required by remote procedure call.

``` 
#yum install nfs-utils nfs-utils-lib 
```

- Allow services to start after reboot in this runlevel.

```
#chkconfig nfs on 
#chkconfig rpcbind on 
#chkconfig --list|egrep -i "rpc|nfs"
```

- Always start the services in this following order.

``` 
#service iptables restart 
#service rpcbind restart 
#service nfs restart 
   
#rpcinfo -p | grep nfs        ##port 2049 should be there for nfs
    100003    2   udp   2049  nfs 
    100003    3   udp   2049  nfs 
    100003    4   udp   2049  nfs 
    100003    2   tcp   2049  nfs 
    100003    3   tcp   2049  nfs 
    100003    4   tcp   2049  nfs 
```

- Change the Selinux booleans

```
# setsebool -P nfs_export_all_rw on
# setsebool -P nfs_export_all_ro on

# getsebool -a |grep -i nfs_export          ##check whether it is set to on 
nfs_export_all_ro --> on 
nfs_export_all_rw --> on 
[root@linux1 ~]# 
```

- Check file /etc/sysconfig/nfs and uncomment the ports.
 
 ```
[root@linux1 ~]# egrep -i "LOCKD_TCPPORT|LOCKD_UDPPORT|MOUNTD_PORT|RQUOTAD_PORT|STATD_PORT|STATD_OUTGOING_PORT" /etc/sysconfig/nfs 
RQUOTAD_PORT=875 
LOCKD_TCPPORT=32803 
LOCKD_UDPPORT=32769 
MOUNTD_PORT=892 
STATD_PORT=662 
STATD_OUTGOING_PORT=2020 
 
rpc.mountd -- NFS maintains list of exported FS in a export table. This table has acl to determine access permission to a client. 
rpc.statd -- once the client reboots it informs the server about the earlier file locks. 
rpc.lockd -- starts NFS lock manager, usually started by default. 
```

- Enable Firewall

```
#setup            ##GUI firewall 
enable nfs and eth1  
save and close 
```

- Add rules in Firewall.  
 
```
iptables -S         ## lists rules
  
- edit in /etc/sysconfig/iptables 
  
-A INPUT -m state --state NEW -m udp -p udp --dport 111 -j ACCEPT 
-A INPUT -m state --state NEW -m tcp -p tcp --dport 111 -j ACCEPT 
-A INPUT -m state --state NEW -m tcp -p tcp --dport 32803 -j ACCEPT 
-A INPUT -m state --state NEW -m udp -p udp --dport 32769 -j ACCEPT 
-A INPUT  -m state --state NEW -m tcp -p tcp --dport 892 -j ACCEPT 
-A INPUT -m state --state NEW -m udp -p udp --dport 892 -j ACCEPT 
-A INPUT -m state --state NEW -m tcp -p tcp --dport 875 -j ACCEPT 
-A INPUT -m state --state NEW -m udp -p udp --dport 875 -j ACCEPT 
-A INPUT  -m state --state NEW -m tcp -p tcp --dport 662 -j ACCEPT 
-A INPUT -m state --state NEW -m udp -p udp --dport 662 -j ACCEPT 
```

- Add entry in exports file as below and restart nfs after editing.

```
#cat /proc/filesystems | grep nfs

#cat /proc/fs/nfs/exports               ## Info about what filesystems are exported and mounted on the client 
  
#vi /etc/exports                 
/shares/website1      <ip-client-1>(rw,sync,no_subtree_check,no_root_squash) 
  
#exportfs -ra         ##refresh the exports

#cat /etc/exports 
/vg1 192.168.2.3(rw,sync,no_root_squash) 
/repository *(ro) 
/linux6.2 *(ro) 

#showmount -e
```
