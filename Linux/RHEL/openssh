## Open ssh

- check whether openssh packages are installed.

```
rpm -qa|grep -i ssh
```

- Enable ssh service

```
chkconfig --list sshd 
chconfig sshd on 
service sshd status
```

- Edit config file /etc/ssh/sshd_config

```
PasswordAuthentication yes 
PermitRootLogin yes 
```

- Configure iptables file /etc/sysconfig/iptables to allow traffic in port 22.

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT 
```


*ssh-keygen -t rsa 
~/.ssh/authourized_keys 


- Install **openssh-server** and **openssh-clients** package
  
- Go to FID Home 
#ssh-keygen -t rsa                        //-t type of key algorithm, creates key in ~/.ssh/id_rsa.pub 
  
ssh-copy-id -i ~/.ssh/id_rsa.pub remote-host

3.go to destination server 
cd ~/.ssh/                        //FID home dir 
vi authorized_keys                         //create authorized_keys and add the add the entry in id_rsa.pub/ append the same for multiple keys.

	4. Below permissions for the files and dir is imp. No write permission for group and others in destination server. 
	755 is fine.

chmod -R 755 /home/<user-id>/

root@segotl3471-n2# ls -l /home/wmsftp/.ssh/authorized_keys
-rw-r--r--. 1 wmsftp dba 12488 Dec 19 11:50 /home/wmsftp/.ssh/authorized_keys
root@segotl3471-n2# ls -ld /home/wmsftp/.ssh/
drwxr-xr-x. 2 wmsftp dba 29 Dec 19 11:50 /home/wmsftp/.ssh/
root@segotl3471-n2# ls -ld /home/wmsftp/
drwxr-xr-x. 5 wmsftp dba 154 Dec 19 14:23 /home/wmsftp/
root@segotl3471-n2#

	5. "ALL" is required in access.conf.
	
root@segotl3471-n1# grep -i wmsftp /etc/security/access.conf                              // it should be All for the destination user in destination server
+ : wmsftp : ALL
root@segotl3471-n1#


Execute commands via SSH

Let us execute uname command over SSH.

$ ssh linuxtechi@192.168.10.10 uname


Execute multiple commands

$ ssh linuxtechi@192.168.10.10 "uname;hostname;date"


Execute command with elevated privileges
Sometimes we need to execute command with elevated privileges, in that case we can use it with sudo.

$ ssh -t linuxtechi@192.168.10.10 sudo touch /etc/banner.txt
Note that we have used ‘-t‘ option with SSH, which allows pseudo-terminal allocation. sudo command requires interactive terminal hence this option is necessary.

Execute local script

$ chmod +x system-info.sh
$ ssh linuxtechi@192.168.10.10 ./system-info.sh

Execute remote script

First copy script to remote machine

$ ssh linuxtechi@192.168.10.10 "sudo /tmp/remote-script.sh"


Looping execution of remote script

for i in `cat /tmp/st/list`; do ssh -t $i "sudo /tmp/remote-script.sh"; done

-- /tmp/st/list has server name


Bigfix installation as example

	• Copy necessary packages and script

	for i in `cat /tmp/st/list`; do scp /tmp/BESAgent-9.5.13.130-rhe6.x86_64.rpm /tmp/actionsite.afxm /tmp/bigfix.sh a262237@$i:/tmp; done

	• Run script

	for i in `cat /tmp/st/list`; do ssh -t $i "sudo /tmp/bigfix.sh"; done


[a262237@segotl0836 ~]$ cat /tmp/bigfix.sh
cd /tmp
rpm -ivh BESAgent-9.5.13.130-rhe6.x86_64.rpm
mkdir -p /etc/opt/BESClient
cp actionsite.afxm /etc/opt/BESClient
/etc/init.d/besclient start
[a262237@segotl0836 ~]$

