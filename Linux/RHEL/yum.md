# yum
- This section covers installation and configuration of a loacal repository in a linux server.
- Mount the OS image and under /media and copy all the packages.

```
cd <media/cd> 
cp -ar * /repository
```

- Required Packages

```
createrepo-0.9.8-4.el6.noarch.rpm 
deltarpm-3.5-0.5.20090913git.el6.i686.rpm 
python-deltarpm-3.5-0.5.20090913git.el6.i686.rpm 
```

- Install the packages in the following order.

```
[root@linux1 Packages]# ls -l deltarpm* 
-r--r--r--. 1 root root 75112 Aug 16  2010 deltarpm-3.5-0.5.20090913git.el6.i686.rpm
 
[root@linux1 Packages]# rpm -ivh  deltarpm-3.5-0.5.20090913git.el6.i686.rpm 
warning: deltarpm-3.5-0.5.20090913git.el6.i686.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY 
Preparing...                ########################################### [100%] 
   1:deltarpm               ########################################### [100%] 

[root@linux1 Packages]# ls -l python-deltarpm* 
-r--r--r--. 1 root root 28136 Aug 16  2010 python-deltarpm-3.5-0.5.20090913git.el6.i686.rpm 

[root@linux1 Packages]# rpm -ivh python-deltarpm-3.5-0.5.20090913git.el6.i686.rpm 
warning: python-deltarpm-3.5-0.5.20090913git.el6.i686.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY 
Preparing...                ########################################### [100%] 
   1:python-deltarpm        ########################################### [100%] 

[root@linux1 Packages]# rpm -ivh createrepo-0.9.8-4.el6.noarch.rpm 
warning: createrepo-0.9.8-4.el6.noarch.rpm: Header V3 RSA/SHA256 Signature, key ID fd431d51: NOKEY 
Preparing...                ########################################### [100%] 
   1:createrepo             ########################################### [100%] 
   
[root@linux1 Packages]# which createrepo 
/usr/bin/createrepo 
```

- Create repository

```
[root@linux1 Packages]#createrepo -v /repository/ 
cd /etc/yum.repos.d 

[root@linux1 yum.repos.d]# cat repository.repo 
[repository.linux1.com] 
comment ="linux1repo" 
baseurl=file:///repository 
gpgcheck=0 
```

- Create yum cache

```
[root@linux1 yum.repos.d]# yum makecache 
Loaded plugins: product-id, refresh-packagekit, security, subscription-manager 
Updating certificate-based repositories. 
Repository 'repo.linux1' is missing name in configuration, using id 
repo.linux1                                                                                                                                      | 1.3 kB     00:00 ... 
repo.linux1/filelists                                                                                                                            | 2.9 MB     00:00 ... 
repo.linux1/other                                                                                                                                | 1.3 MB     00:00 ... 
repo.linux1                                                                                                                                                   2804/2804 
repo.linux1                                                                                                                                                   2804/2804 
Metadata Cache Created 
```

## Downgrade a package

```
yum history                               ##check the serial no
yum history info <serial-no.>  |less      ##check the older package version
yum downgrade <older-pkg-version>
```

## Clean duplicate packages

```markdown
- Check whether the package is installed,
	yum install yum-utils

- To list duplicates
	package-cleanup --dupes 

- To clean up packages
	package-cleanup --cleandupes   
```

## Last Installed packages

```
rpm -qa --last |less
```

## Local install

```markdown
- To only download the packages,
		/usr/bin/yum update --assumeyes --downloadonly --exclude=pkg1,pkg2,pkg3

- To Install local files,
		yum localinstall <pkg-name>
 ```
 
## More yum commands

```
yum install <packagename>                                                                
yum remove <packagename> 
yum info <packagename> 
yum search <packagename> 
yum grouplist 
yum groupinstall 'group' 
yum groupremove 'group' 
yum repolist                   							##list enabled repositories 
yum repolist all               							##list enabled and disabled repositories 
yum check-update 
yum update <package> 
yum update all 
yum clean all 
yum history 
yum –enablerepo=repo1 install <package>     ##to install from a specific repository 
yum makecache fast
yum whatprovides /usr/bin/ssh
```
