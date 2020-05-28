[Back to Homepage](https://linuxcloudadmin.github.io)

## Performance Tools
- This section has the various performance tools, their usage and field expanation.

### top

- To find usage by threads

```
top -H   	##usage by individual threads
```

- Few short cuts used in top command

```
c to show full command
1 to show individual cpu stats
Z for color toggle
< and > sort with next column
B for bolb

O to switch to sort mode
Below are the sorting list
  a: PID        = Process Id
  b: PPID       = Parent Process Pid
  c: RUSER      = Real user name
  d: UID        = User Id
  e: USER       = User Name
  f: GROUP      = Group Name
  g: TTY        = Controlling Tty
  h: PR         = Priority
  i: NI         = Nice value
  j: P          = Last used cpu (SMP)
* K: %CPU       = CPU usage
  l: TIME       = CPU Time
  m: TIME+      = CPU Time, hundredths
  n: %MEM       = Memory usage (RES)
  o: VIRT       = Virtual Image (kb)
  p: SWAP       = Swapped size (kb)
  q: RES        = Resident size (kb)
  r: CODE       = Code size (kb)
  s: DATA       = Data+Stack size (kb)
  t: SHR        = Shared Mem size (kb)
  u: nFLT       = Page Fault count
  v: nDRT       = Dirty Pages count
  w: S          = Process Status
  x: COMMAND    = Command name/line
  y: WCHAN      = Sleeping in Function
  z: Flags      = Task Flags <sched.h>

VIRT stands for the virtual size of a process, which is the sum of memory it is actually using, memory it has mapped into itself (for instance the video card’s RAM for the X server), files on disk that have been mapped into it (most notably shared libraries), and memory shared with other processes. VIRT represents how much memory the program is able to access at the present moment.  VIRT=SWAP+RES
		
RES stands for the resident size, which is an accurate representation of how much actual physical memory a process is consuming. (This also corresponds directly to the %MEM column.) This will virtually always be less than the VIRT size, since most programs depend on the C library.
		
SHR indicates how much of the VIRT size is actually sharable (memory or libraries). In the case of libraries, it does not necessarily mean that the entire library is resident. For example, if a program only uses a few functions in a library, the whole library is mapped and will be counted in VIRT and SHR, but only the parts of the library file containing the functions being used will actually be loaded in and be counted under RES.
```

- Top procs-ng 3.3.10 is another version of top

```
F or f for listing the above menu
d to display that entry
-> selects the entry. so that it can be moved up and down.
s sorts by the current column
```

### sar - system activity report

```
sar -BbSbrudHf			
##-f to specify file in /var/log/sa; -u cpu ;-r memory; -S swap ; -B paging; -b i/o stats; -d disk utilization; 

sar -p -f /var/log/sa/sa25	##cpu
sar -q -f /var/log/sa/sa25	##load average
sar -B 2 5      		##paging in/s are more than 100pgs/s
```

- To calculate free memory in sar

```
sar -r |egrep -v "Linux|kbmemfree|Average"|tr -s " "|awk '{print "total free memory during",$1,"is",((($3+$6+$7)/1024)/1024),"G"}' |sed 1d
```

- Historical disk utilization of individual disks

```
sar -d -p |grep -i nvme0n1    
```

### netstat
```
netstat -anplt  	
# -a all; -n for number format of IP; -p for PID; -l to list only listening; -t/-u for tcp/udp; -c promiscous mode to refresh every 5 sec; -r for  table;
```

### iostat
```
iostat -d -x 2 5 	                 ## -d for disks; -x for extended o/p
Device:         rrqm/s   wrqm/s     r/s     w/s   rsec/s   wsec/s avgrq-sz avgqu-sz   await r_await w_await  svctm  %util
sda               0.17     6.22    1.38    3.11    58.35    74.67    29.64     0.01    2.26    1.50    2.59   0.93   0.42
		
rrqm/s -- The number of read requests merged per second that were queued to the device.
wrqm/s -- The number of write requests merged per second that were queued to the device.
r/s -- The number of read requests that were issued to the device per second.
w/s -- The number of write requests that were issued to the device per second.
rsec/s -- The number of sectors read from the device per second.
wsec/s -- The number of sectors written to the device per second.
avgrq-sz -- The average size (in sectors) of the requests that were issued to the device.
avgqu-sz -- The average queue length of the requests that were issued to the device.
await -- The  average  time  (in  milliseconds)  for  I/O  requests  issued to the device to be served. This includes the time spent by the requests in queue and the time spent servicing them.
svctm -- The average service time (in milliseconds) for I/O requests that were issued to the  device.  Warning! Do not trust this field any more. This field will be removed in a future sysstat version.
%util -- Percentage  of elapsed time during which I/O requests were issued to the device (bandwidth utilization for the device). Device saturation occurs when this value is close to 100%.
```

- Few flags

```
iostat -V 		##version 
iostat -p sda 		##stats for individual disk		
iostat -N  		##r/w stat of lvm
 
iostat -d 		##tps -- number of transfer per second
Device:            tps   Blk_read/s   Blk_wrtn/s   Blk_read   Blk_wrtn
sda               4.49        58.35        74.67  344618858  441051954

tps -- Indicate  the  number  of transfers per second that were issued to the device. A transfer is an I/O request to the device. multiple logical requests can be combined into a single I/O request  to  the device. A transfer is of indeterminate size.
Blk_read/s -- Indicate the amount of data read from the device expressed in a number of blocks per second. Blocks are equivalent to sectors with kernels 2.4 and later and therefore have a size of 512  bytes.
Blk_wrtn/s -- Indicate the amount of data written to the device expressed in a number of blocks per second.
Blk_read -- The total number of blocks read.
Blk_wrtn -- The total number of blocks written.

iostat -n               ##for nfs filesystems
```

```
iostat -c 		##for cpu 
Linux 2.6.32-642.13.2.el6.x86_64 (segotl0836)   09/07/2017      _x86_64_        (1 CPU)
	
avg-cpu:  %user   %nice %system %iowait  %steal   %idle
           6.25    0.25    6.58    0.32    0.00   86.59

%user -- Show  the  percentage  of CPU utilization that occurred while executing at the user level (application).
%nice -- Show the percentage of CPU utilization that occurred while executing at the user  level  with  nice priority.
%system -- Show  the percentage of CPU utilization that occurred while executing at the system level (kernel).
%iowait -- Show the percentage of time that the CPU or CPUs were idle during which the system had an outstanding disk I/O request.
%steal -- Show the percentage of time spent in involuntary wait by the virtual CPU or CPUs while the hypervisor was servicing another virtual processor.
%idle -- Show the percentage of time that the CPU or CPUs were idle and the system did not have an outstanding disk I/O request.
		
nfsiostat -s 5    ##sort with high IO shares.
```

### iowait / iotop

iowait is the percentage of time spent by the CPU being idle (or) waiting for an IO operation to complete. It happens in the below scenarios.

- It could be due to other processes which are writing to the disks.  IO is high here as P92adm process is continuously writing to the disk. This is the issue in this case. So, you have to check from application side why the process is taking more time to read and write.
- It might also be due to low performing H/W disks, which takes more time to read and write. This was fixed when you migrated to VSAN.
- However in some cases, IO will reach maximum, when shares are completely unavailable (like Storage issue) causing very high abnormal CPU usage which might lead to server hung. It will be during major storage outage.

```
iotop -o		##iowait of process which are causing the issue.
iotop -o -p <pid>	##iowait of an particular pid.
```

### iftop

- Used to check which process is using high bandwidth or high network usage.

```
iftop           ##-n to suppress dns lookup which used high cpu, -S to show source port, D to show destination port
```

### netstat

- grep the port number in netstat command as below, to know which process is using 

```
netstat -tup |grep -i 57136
```

### vmstat

```
vmstat -s 		##total memory and swap usage break up
vmstat -wt 1		## -w increase space width in the display output
vmstat 1 5		
	
vmstat			##check for si an so which shows high usage when fluctuates frequently
procs -----------memory---------- ---swap-- -----io---- --system-- -----cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 0  0 223168 2521888  90964  78996    0    0   116  1358    0    6  7  7 87  0  0

Procs
    r: The number of processes waiting for run time.
    b: The number of processes in uninterruptible sleep
Memory
    swpd: the amount of virtual memory used.
    free: the amount of idle memory.
    buff: the amount of memory used as buffers.
    cache: the amount of memory used as cache.
    inact: the amount of inactive memory. (-a option)
    active: the amount of active memory. (-a option)
Swap
    si: Amount of memory swapped in from disk (/s).
    so: Amount of memory swapped to disk (/s).
IO
    bi: Blocks received from a block device (blocks/s).
    bo: Blocks sent to a block device (blocks/s).
System
    in: The number of interrupts per second, including the clock.
    cs: The number of context switches per second.
CPU
    These are percentages of total CPU time.
    us: Time spent running non-kernel code. (user time,including nice time)
    sy: Time spent running kernel code. (system time)
    id: Time spent idle. Prior to Linux 2.5.41, this includes IO-wait time.
    wa: Time spent waiting for IO. Prior to Linux 2.5.41, included in idle.
    st: Time stolen from a virtual machine. Prior to Linux 2.6.11, unknown.
```
		   
### ps - Process Status

```
ps -p <pid> -o %cpu					##cpu usage of a single pid
ps -eo user,pid,comm,pmem,pcpu 				##Customize columns
ps h -Led -o user | sort | uniq -c | sort -n 		##including light-weight process (LWP)/ multi threads
ps -o nlwp,pid,lwp,args -u oracle | sort -n 		##no. of threads per process
```


[Back to Homepage](https://linuxcloudadmin.github.io)
