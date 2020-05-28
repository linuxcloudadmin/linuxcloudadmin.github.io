[Back to Homepage](https://linuxcloudadmin.github.io)

## Dell Hardware Commands

```		
omreport chassis memory            		##Check DIMM in dell server
omreport chassis pwrsupplies 			##Power Supplies Information
omreport storage controller 			##List storage controller and status
omreport chassis batteries
omreport storage controller controller=0 	##Details of controller
omreport storage pdisk controller=0	      	##Details of physical disk
omreport storage vdisk controller=0 	      	##Details of virtual disk 
omreport chassis nics			      	##NIC status
```

## HP storage commands
- HP Storage array commands

```
mv /dev/shm/sem.hpacu.appLock /dev/shm/sem.hpacu.appLock.old

hpacucli ctrl all show				 		##lists the controller configured

hpacucli ctrl slot=0 show config			 	##shows disk details of specified controller

hpacucli ctrl slot=0 show detail    			       	##shows the details of the controller

hpacucli ctrl slot=0 logicaldrive 1 show detail  		##shows detail of the logical drive and its equivalent to /dev/dsa

hpacucli ctrl slot=0 physicaldrive 1I:1:1 show detail   	##shows detail of physical drive

hpacucli ctrl slot=1 array e show detail			##shows detail of the array

hpssacli ctrl slot=4 ld all show detail

hpssacli ctrl slot=0 ld 2 modify led=on 	      		##blink logical drive led

physicaldrive 2I:1:25 (port 2I:box 1:bay 25, SAS, 1200.2 GB, OK, spare)

hpssacli ctrl slot=0 pd 1:25 modify led=on 
```
	
- To prevent the delay from occurring when accessing local storage, type the following command

```
export INFOMGR_BYPASS_NONSA=1    
```

- Cheat sheet

<http://www.datadisk.co.uk/html_docs/redhat/hpacucli.htm>

## HP Hardware commands

```
hpasmcli -s "SHOW POWERSUPPLY"
		
hpssacli ctrl all show status
```

## hpasm

- hpasm is a daemon which check hard components on the server.
- If you can connect to the server, run the below command and check if the service is running or not, If  (hpasmlited) is running... then please close the case 

```
/etc/init.d/hp-health status 
```


[Back to Homepage](https://linuxcloudadmin.github.io)
