## Open ssh

- check whether openssh packages are installed.If not install **openssh-server** and **openssh-clients** package

```
rpm -qa|grep -i ssh
```

- Enable ssh service

```
chkconfig --list sshd 
chconfig sshd on 
service sshd status
```

- Edit config file /etc/ssh/sshd_config and allow the below parameters.

```
PasswordAuthentication yes 
PermitRootLogin yes 
```

- Configure iptables file /etc/sysconfig/iptables to allow traffic in port 22.

```
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT 
```

## Passwordless SSH Authentication

- Generate ssh keys for the source user.

```
# ssh-keygen -t rsa 		##-t type of key algorithm, creates key in ~/.ssh/id_rsa.pub 
# ssh-copy-id -i ~/.ssh/id_rsa.pub remote-host
```

- Go to destination server and add entry in destination user home directory <user-home-dir>/.ssh/authorized_keys

```
cd ~/.ssh/       	      ##FID home dir 
vi authorized_keys            ##create authorized_keys and add the add the entry in id_rsa.pub / append the same for multiple keys.
```

- Below permissions for the files and dir is important. No write permission for group and others in destination server. 755 is fine.

```
chmod -R 755 /home/<user-id>/

root@github# ls -l /home/ftp/.ssh/authorized_keys
-rw-r--r--. 1 ftp dba 12488 Dec 19 11:50 /home/ftp/.ssh/authorized_keys
root@github# ls -ld /home/ftp/.ssh/
drwxr-xr-x. 2 ftp dba 29 Dec 19 11:50 /home/ftp/.ssh/
root@github# ls -ld /home/ftp/
drwxr-xr-x. 5 ftp dba 154 Dec 19 14:23 /home/ftp/
```

- "ALL" is required in access.conf for the local user in destination server.

```
root@github# grep -i ftp /etc/security/access.conf                ##it should be All for the destination user in destination server
+ : ftp : ALL
```

## Execute commands via SSH

- Let us execute uname command over SSH.

```
$ ssh linuxtechi@192.168.10.10 uname
```

- Execute multiple commands

```
$ ssh user1@192.168.10.10 "uname;hostname;date"
```

- Execute command with elevated privileges. Sometimes we need to execute command with elevated privileges, in that case we can use it with sudo.

```
$ ssh -t user1@192.168.10.10 sudo touch /etc/banner.txt

Note that we have used ‘-t‘ option with SSH, which allows pseudo-terminal allocation. sudo command requires interactive terminal hence this option is necessary.
```

- Execute local script

```
$ chmod +x system-info.sh
$ ssh user1@192.168.10.10 ./system-info.sh
```

- To execute remote script, First copy script to remote machine the execute the below command

```
$ ssh user1@192.168.10.10 "sudo /tmp/remote-script.sh"
```

- Looping execution of remote script

```
for i in `cat /tmp/st/list`; do ssh -t $i "sudo /tmp/remote-script.sh"; done

/tmp/st/list has list of server names
```

## httpd installation as example

- Copy necessary packages and script

```
for i in `cat /tmp/st/list`; do scp /tmp/httpd.conf /tmp/http-install-script.sh user1@$i:/tmp; done
```

- Run script

```
for i in `cat /tmp/st/list`; do ssh -t $i "sudo /tmp/http-install-script.sh"; done
```

```
#cat /tmp/http-install-script.sh
#!/bin/bash
yum update -y
yum install httpd -y
service httpd start
chkconfig httpd on
cd /var/www/html
echo "<html><h1>Welcome to the EC2 Fleet!</h1></html>" > index.html
```
