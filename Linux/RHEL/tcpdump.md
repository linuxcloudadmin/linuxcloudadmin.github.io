[Back to Homepage](https://linuxcloudadmin.github.io)

## tcpdump
- tcpdump captures or filter TCP/IP packets that received or transferred over the network. 
- It is required to analyse network issue in an environment.
- To capture packets from a specific interface

```
tcpdump -i eth0
```

- Write captured data to a file

```
tcpdump -w 0001.pcap -i eth0
```

- Capture packets only from source IP received in the interface.

```
tcpdump -i eth0 src 192.168.0.2
```

- Capture only tcp packets from a specifie port and source

```
tcpdump -c 5 -A -i eth0 tcp port 22 src 192.168.0.2 

## -A for dump in acsii; -c for only 6 packets; tcp to specify protocols; port to specify a port; src to give a source
```

- Read dump file.

```
tcpdump -r 0001.pcap		  //read tcpdump o/p
```

- Capture 8 files of 500M each

```
tcpdump -s 0 -i eth0 -vvv -C 500M -W 8 -w /tmp/tcpdump1.pcap host 192.168.0.21 or host 192.168.0.25 or host 192.168.0.22
```

- Capture dump to or from the specified IP.

```
tcpdump -nni eth0 -w /tmp/tcpdump1.pcap host 192.168.2.115
```

### Run tcpdump for 5 mins (300s)

```
timeout 300 tcpdump -tnn -i eth0 > /tmp/tcpdump.txt

## -nn Don’t convert protocol and port numbers etc. to names either; -t Don’t print a timestamp on each dump line
```

### Top talkers / to find to which IP maximum communication is happening

```
cat /tmp/tcpdump.txt | awk -F "." '{print $1"."$2"."$3"."$4}' | sort |uniq -c |tr -s " " |cut -d " " -f2,3,4 |sort -rn;
```

[Back to Homepage](https://linuxcloudadmin.github.io)
