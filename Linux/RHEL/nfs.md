```
1. For nfs, portmap or rpcbind should be running for allocating ports required by remote procedure call  
  
#yum install nfs-utils nfs-utils-lib 
  
  
2. Always start in this following order : 
  
#service iptables restart 
#service rpcbind restart 
#service nfs restart 
  
  
#rpcinfo -p | grep nfs 
    100003    2   udp   2049  nfs 
    100003    3   udp   2049  nfs 
    100003    4   udp   2049  nfs 
    100003    2   tcp   2049  nfs 
    100003    3   tcp   2049  nfs 
    100003    4   tcp   2049  nfs 
  
  
#cat /proc/filesystems | grep nfs 
  
#vi /etc/exports                 //add entry as below and restart nfs always after edit L
/shares/website1      <ip-client-1>(rw,sync,no_subtree_check,no_root_squash) 
  
#exportfs -ra 
  
# getsebool -a |grep -i nfs_export //check whethet it is set to on 
nfs_export_all_ro --> on 
nfs_export_all_rw --> on 
[root@linux1 ~]# 
  
#chkconfig nfs on 
#chkconfig rpcbind on 
  
  
#enable and set the below to the default or required ports 
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
 
  
#setup //GUI firewall 
enable nfs and eth1  
save and close 
  
  
cat /proc/fs/nfs/exports 
chkconfig --list|grep rpc 
iptables -S 
  
  
edit in /etc/sysconfig/iptables 
  
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

