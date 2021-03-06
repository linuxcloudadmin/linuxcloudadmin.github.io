[Back to Homepage](https://linuxcloudadmin.github.io)

## Linux LVM
- This section covers the linux lvm commands.
- For scanning newly added disks or devices. This makes the disk visible to linux OS.
- "- - -" The three values stand for channel, SCSI target ID, and LUN. The dashes act as wildcards meaning "rescan everything".

```
cd /sys/class/block 
cd /sys/class/scsi_disk 
echo "- - -">/sys/class/scsi_host/host2/scan
```

- We can also use **lvmdiskscan** which lists devices that may be used as physical volumes. It scans  all  SCSI, (E)IDE disks, multiple devices and a bunch of other block devices in the system looking for LVM PVs.

- To delete a device.

```
cd /sys/class/block/sdb/device/ 
echo "- - -">delete 
```

- Creating  FS from scratch.

```
pvcreate /dev/sde /dev/sdb 
vgcreate sample2vg /dev/sdb /dev/sdc 
lvcreate -L 100M -n svglv1 samplevg  
mkfs -t ext4 /dev/samplevg/svglv1 
mount /svglv1
```
- Extending FS.

```
[root@linux1 ~]# lvextend -l +25 /dev/mapper/samplevg-svglv1        ## -l no. of extents
  Extending logical volume svglv1 to 200.00 MiB 
  Logical volume svglv1 successfully resized 

or

[root@linux1 ~]# lvextend -L +100M /dev/mapper/samplevg-svglv1      ## -L specify the space in MB, GB
  Extending logical volume svglv1 to 200.00 MiB 
  Logical volume svglv1 successfully resized 
```

```
[root@linux1 ~]# resize2fs /dev/samplevg/svglv1 
resize2fs 1.41.12 (17-May-2010) 
Filesystem at /dev/samplevg/svglv1 is mounted on /svglv1; on-line resizing required 
old desc_blocks = 1, new_desc_blocks = 1 
Performing an on-line resize of /dev/samplevg/svglv1 to 204800 (1k) blocks. 
The filesystem on /dev/samplevg/svglv1 is now 204800 blocks long. 

[root@linux1 ~]# df -h 
Filesystem                      Size  Used Avail Use% Mounted on 
/dev/sda2                        18G  7.9G  8.8G  48% / 
tmpfs                           504M     0  504M   0% /dev/shm 
/dev/sda1                       291M   31M  246M  11% /boot 
.host:/                          84G   50G   35G  59% /mnt/hgfs 
/dev/mapper/samplevg-svglv1     194M  5.6M  179M   3% /svglv1 
```

- Reducing a FS. We need to unmount the FS before reducing. xfs filesystem cannot be reduced.

``` 
[root@linux1 ~]# umount /svglv1 

[root@linux1 ~]# resize2fs /dev/samplevg/svglv1 150M 
resize2fs 1.41.12 (17-May-2010) 
Resizing the filesystem on /dev/samplevg/svglv1 to 153600 (1k) blocks. 
The filesystem on /dev/samplevg/svglv1 is now 153600 blocks long. 

[root@linux1 ~]# lvscan 
  ACTIVE            '/dev/samplevg/svglv1' [200.00 MiB] inherit 
  
[root@linux1 ~]# lvreduce -L 150M /dev/samplevg/svglv1 
  Rounding up size to full physical extent 152.00 MiB 
  WARNING: Reducing active logical volume to 152.00 MiB 
  THIS MAY DESTROY YOUR DATA (filesystem etc.) 
Do you really want to reduce svglv1? [y/n]: y 
  Reducing logical volume svglv1 to 152.00 MiB 
  Logical volume svglv1 successfully resized
  
[root@linux1 ~]# mount /svglv1 

[root@linux1 ~]# df -h 
Filesystem                        Size  Used Avail Use% Mounted on 
/dev/sda2                          18G  7.9G  8.8G  48% / 
tmpfs                             504M     0  504M   0% /dev/shm 
/dev/sda1                         291M   31M  246M  11% /boot 
.host:/                            84G   50G   35G  59% /mnt/hgfs 
/dev/mapper/samplevg-svglv1       146M  5.6M  133M   5% /svglv1 
[root@linux1 ~]# 
```

- Removing a logical volume. unmount the FS and run below commands.

```
[root@linux1 ~]# lvscan 
  ACTIVE            '/dev/samplevg/svglv1' [100.00 MiB] inherit 
  
[root@linux1 ~]# lvremove /dev/samplevg/svglv1 
Do you really want to remove active logical volume svglv1? [y/n]: y 
  Logical volume "svglv1" successfully removed 
[root@linux1 ~]# 
```

- Adding and removing disk from volume group.

```markdown
- Removing a disk from VG.
 
[root@linux1 s2vglv1]# vgreduce samplevg /dev/sdd 
  Removed "/dev/sdd" from volume group "samplevg" 
[root@linux1 s2vglv1]# pvs 
  PV         VG        Fmt  Attr PSize PFree 
  /dev/sdb   sample2vg lvm2 a--  2.00g 1.93g 
  /dev/sdc   samplevg  lvm2 a--  3.00g 2.85g 
  /dev/sdd             lvm2 a--  2.00g 2.00g 
  /dev/sde   sample2vg lvm2 a--  3.00g 3.00g
```

- Adding a disk to VG.

```
[root@linux1 s2vglv1]# pvcreate /dev/sdd
[root@linux1 s2vglv1]# vgextend sample2vg /dev/sdd 
  Volume group "sample2vg" successfully extended 
```

- Renaming a Volume group. unmount all file systems in the VG and run below command.

```
[root@linux1 archive]# vgrename samplevg sample1vg 
  Volume group "samplevg" successfully renamed to "sample1vg"
```

- Renaming a logical volume.

```
[root@linux1 archive]# lvrename /dev/vgname/old-lvname /dev/vgname/new-lvname
```

- To display the logical volume present in a physical disk.

```
pvdisplay -m  
```

- To display logical volume and physical volume in a VG.

```
vgdisplay -v 
```

- Corrupting a superblock 

```
# dumpe2fs  <lvpath>                                           ##lists superblock 
# dd  if=/dev/zero count=1 bs=4096 of=/dev/<FS to corrupt>     ##removes first super block. 
# e2fsck <lvpath>                                              ##run fsck interactively 
# fsck –b <superblock>  /dev/snapvg/lv1 
```

- Taking snapshot of a existing LV

```
lvcreate -L 10G -s -n <snapshotname>  <lvpathname>        -s creates snapshot volume, create snapshot as same size as the FS 
```
- We can also extend snap shot using lvextend command.
- Merge snapshot with FS using 

```
lvconvert --merge <lvpathname>
```

- To extend snapshot automatically put entry in /etc/lvm/lvm.conf


[Back to Homepage](https://linuxcloudadmin.github.io)
