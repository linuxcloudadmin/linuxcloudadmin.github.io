## Samba - Installing and Configuring Samba server

- This section covers Installation and configuration of Samba server.
- Install the below packages in the server side.

```
server side : 
samba.i686 0:3.5.10-114.el6 
samba-common.i686 0:3.5.10-114.el6 
samba-winbind.i686 0:3.5.10-114.el6 
```

- Enable and start the necessary services required.

```
smb                 smbd                (SMB/CIFS Server) main samba service which provide user authentication and authorization and file and printer sharing 
nmb                 nmbd                (NetBIOS name server) Resources browsing 
winbind         winbindd                For host and user name resolution (optional) 
  
chkconfig smb on 
chkconfig nmb on 
chkconfig winbind on 

[root@linux1 ~]# service smb status 
smbd (pid  6830) is running... 
[root@linux1 ~]# service nmb status 
nmbd (pid  6845) is running... 
[root@linux1 ~]# service winbind status 
winbindd (pid  6857) is running... 
[root@linux1 ~]# 
```

- To make Samba to communicate outside the server we have to configure iptables and SELinux. SAMBA uses ports 137,138,139 and 445.

```
netbios-ns      137/tcp                         # NETBIOS Name Service 
netbios-ns      137/udp 
netbios-dgm     138/tcp                         # NETBIOS Datagram Service 
netbios-dgm     138/udp 
netbios-ssn     139/tcp                         # NETBIOS session service 
netbios-ssn     139/udp 
microsoft-ds    445/tcp 
microsoft-ds    445/udp 
```  

- Add the firewall / iptable rules.

```  
#iptables -A INPUT -m state --state NEW -m udp -p udp --dport 137 -j ACCEPT  
#iptables -A INPUT -m state --state NEW -m udp -p udp --dport 138 -j ACCEPT  
#iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 139 -j ACCEPT  
#iptables -A INPUT -m state --state NEW -m tcp -p tcp --dport 445 -j ACCEPT 
```

- Allow SElinux booleans that are required.

```
#chcon -R -t samba_share_t /sharedata 
 
#setsebool -P samba_enable_home_dirs on                       ##Enables the sharing of home directories 
#setsebool -P samba_export_all_ro on                          ##Enable read-only access to any directory 
#setsebool -P samba_export_all_rw on                          ##Sets up read/write access to any directory 
#samba_share_t <default_folder>                               ## which Samba can share 
```

- Add users to OS and to samba database.

```
useradd user1
passwd user1
smbpasswd -a user1  ##can be used to add a user to the password database under /etc/samba/ for SAMBA authentication
groupadd sambagrp
usermog -G sambagrp user1
```

- Confirm workgroup in /etc/samba/smb.conf (# and ; disables entry in smb.conf). command **testparam** tests errors in smb.conf.

```
netbios name = MYSERVER 
workgroup = MYGROUP 
hosts allow = 127. 192.168.12. 192.168.13. 

[suda] 
   comment = Icomming data 
   writable = yes 
   path = /suda 
   browseable = no 
   public = yes 
   valid users = %S 
   

root@linux1 suda]# service smb reload 
Reloading smb.conf file:                                   [  OK  ] 
```

- Command **smbstatus** shows the connection and shares shared from the server.
- In client machine, install the below packages.

```
samba-common.i686 0:3.5.10-114.el6 
samba-winbind.i686 0:3.5.10-114.el6 
samba-client-3.5.10-114.el6.i686
```

- check connection from client server.
  
 ```
[root@linux2 smbmount]# smbclient -L 192.168.2.2 -U user1                ##L netbios i.e., server ip addr and -U username added in smbpasswd 
Enter user1's password: 
Domain=[MYGROUP] OS=[Unix] Server=[Samba 3.5.10-114.el6] 
  
        Sharename       Type      Comment 
        ---------       ----      ------- 
        IPC$            IPC       IPC Service (Samba Server Version 3.5.10-114.el6) 
        Fax:3           Printer   Fax 
        Microsoft_XPS_Document_Writer:2 Printer   Microsoft XPS Document Writer 
        Send_To_OneNote_2010:1 Printer   Send To OneNote 2010 
        user1           Disk      Home Directories 
Domain=[MYGROUP] OS=[Unix] Server=[Samba 3.5.10-114.el6] 
  
        Server               Comment 
        ---------            ------- 
        LINUX1               Samba Server Version 3.5.10-114.el6 
        SUDHARSAN-PC 
  
        Workgroup            Master 
        ---------            ------- 
        MYGROUP              LINUX1 
[root@linux2 smbmount]# 
```

- cifs-utils.i686 should be installed on client to avoid the below error. 
  
```
[root@linux2 /]# mount -t cifs //192.168.2.2/home/samuser1 smbmount -o username=samuser1 
mount: block device //192.168.2.2/home/samuser1 is write-protected, mounting read-only 
mount: cannot mount block device //192.168.2.2/home/samuser1 read-only 
```

- Mention the share name specified in smb.configure i.e.,[suda] 

```
[suda] 
   comment = Icomming data 
   writable = yes 
   path = /suda 
```

- User access can be limited by specifying only valid or invalid users, and by using valid user group name to permit only users in this group. 

```
more options: 
  read only = no 
  public = yes 
  invalid users = users1 
  validusers = @group1, users1 
```  
