[Back to Homepage](https://linuxcloudadmin.github.io)

## locate command
- locate command is used to find files by name.
- Below is the locate package that should be installed for this command.

```
mlocate.x86_64 : An utility for finding files by name
```

- To find a file named sshsshd_config

```
root@github# locate sshd_config
/etc/ssh/sshd_config
/etc/ssh/sshd_config.new
```

- **updatedb** command is used to update the locate database with the newly created files.
- locate db is present in **/var/lib/mlocate/mlocate.db**.

```
root@github# updatedb
```


[Back to Homepage](https://linuxcloudadmin.github.io)
