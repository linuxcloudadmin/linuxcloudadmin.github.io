[Back to Homepage](https://linuxcloudadmin.github.io)

## rsync
- Rsync is a fast and extraordinarily versatile file copying tool. 
- It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon. 
- It offers a large number of options that control every aspect of its behavior and permit very flexible specification of the set of files to be copied.

```
rsync -avrtzhp --progress /source/dir/ /destination/dir/

rsync -avrtzhp --progress --exclude 'child-dir1' --exclude 'dir1' /source/dir/ /destination/dir/

-a archive mode, -v  verbose, -r recursive, -t preserve modification time, -z compress, -h human readable form, -p preserve permissions
--exclude prevents the directory 'child-dir1' and 'dir1' present under /source/dir/ from copying
```


[Back to Homepage](https://linuxcloudadmin.github.io)
