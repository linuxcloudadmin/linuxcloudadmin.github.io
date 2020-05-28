[Back to Homepage](https://linuxcloudadmin.github.io)

## SUID, SGID, Sticky Bit

- SUID is set to give the permission as the owner to any user who runs the file.  This is given only to files.  

```
# chmod u+s [file-name]
# chmod 4555 [file-name]

eg:

# ls -l /usr/bin/passwd
-rwsr-xr-x. 1 root root 27832 Jan 30  2014 /usr/bin/passwd
```

- SGID can be set to both files and directories.  It is set to give permission of a group to a person who ever is running the file or accessing the directories.  
- It is used when multiple users share a single dir to create files and access it.  

```
chmod g+s file1 
chmod 2555 [directory]

eg:

# ls -l /usr/bin/write
-r-xr-sr-x  1   root tty 11484 Jan 15 17:55 /usr/bin/write
```

- In sgid comes a issue where every user has the permission to delete the files of others in the dir. 
- To over come this setting sticky bit provides a quick solution of providing permission only to the owner of the file to remove the file, and provides permission  to others to access or modify it.  
		
```
chmod o+t file1 
chmod 1777 [directory]

Eg : /var/tmp and /tmp

# ls -ld /var/tmp
drwxrwxrwt  2   sys   sys   512   Jan 26 11:02  /var/tmp
```	


[Back to Homepage](https://linuxcloudadmin.github.io)
