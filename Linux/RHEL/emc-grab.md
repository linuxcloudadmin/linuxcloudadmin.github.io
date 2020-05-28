[Back to Homepage](https://linuxcloudadmin.github.io)

## EMCgrab
- EMCgrab is a utility that is run locally on each host and gathers storage-specific information (driver version, storage-technical details, etc).
- When we face issue while using EMC SAN storage, EMC will ask for emc grab. Below is the steps to run emc grab.

```
Step 1: Download the EMCGrab File from the Below FTP Link

ftp://ftp.emc.com/pub/emcgrab/Unix/emcgrab_Linux_v4.7.3.tar
or browse the below for respective OS
ftp://ftp.emc.com/pub/emcgrab/Unix/

and download emcgrab_Linux_v4.7.10.tar

Step 2: Upload the file to the root of the host

Step 3: unzip the file on the host

CLI:
  # cd /usr
  # tar xvf /root/emcgrab_Linux_v4.7.3.tar (or else give the file path where you uploaded the .tar file instead of /root/emcgrab_Linux_v4.7.3.tar)
  # mv /usr/emcgrab /usr/emc
  # cd /usr/emc
  # ./emcgrab.sh
DO YOU AGREE TO ACCEPT THE TERMS & CONDITIONS OF THIS LEGAL AGREEMENT (Y/N): Y
 EMC GRAB IDENTIFICATION INFORMATION
 -----------------------------------
 1) Service Request Number:              No Information Supplied

 2) Customer Party Number:

 3) Customer Company Name:
 4) Customer Contact Name:
 5) Customer Phone Number:
 6) Customer Email Address:

 Hostname / OS Info:                  hostname / Linux

Is the EMC Grab Identification Information Correct (Y/N)        : Y

Do you want to collect SMI-S information (y/n) ? n
Do you want to collect XtremIO Array information (y/n) ? n
Please submit this file to EMC for Analysis.  Thank you
Removing Temporary Files....

Step 4: check the file is generated
  # cd /usr/emc/outputs
  #ls   (Check file is generated)
HostName_2016-04-14-21.53.32_Linux_emcgrab_V4.7.3_full_CC0000000000.tar.gz

Step 5: Open WinSCP Tool and login in the host and download the file from path: /usr/emc/outputs/HostName_TimeStamp_Linux_emcgrab_V4.7.3_full_CC0000000000.tar.gz
Step 6: Upload it to EMC vendor.

Let me know if you face Any Issues

CleanUP:
As the root user on <<emc install node>> do the following.

# cd /root
# rm <<emc storage rpm>>
# rm license.txt
# rm <<emc grab tar file>>
```


[Back to Homepage](https://linuxcloudadmin.github.io)
