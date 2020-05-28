## HP ILO

- To print HP ILO IP,

```
hponcfg -w <file-name>
			
cat ILO_ouput.out				##cat the file and look for IP, something like the below.

<IP_ADDRESS VALUE = "192.19.229.30"/>
<SUBNET_MASK VALUE = "255.255.255.0"/>
<GATEWAY_IP_ADDRESS VALUE = "192.19.229.1"/>
```

- To Reset ILO

```
</>hpiLO-> cd /map1
status=0
status_tag=COMMAND COMPLETED
</map1>hpiLO-> reset
status=0
status_tag=COMMAND COMPLETED
Resetting iLO.
```

## DELL IDRAC

- To get idrac IP,
	
```
racadm getniccfg

IPv4 settings:
NIC Enabled          = 1
IPv4 Enabled         = 1
DHCP Enabled         = 0
IP Address           = 10.220.26.205
Subnet Mask          = 255.255.254.0
Gateway              = 10.220.26.1
```

- Connecting serial console

```
admin1-> console com2			##connecting to serial text console
```

- We can use soft reset of iDRAC without affecting the server. From server run below,
		
```
racadm>>racadm racreset soft
```

- To close console session taken by others, 

```
racadm closessn -a
```

### IDRAC commands 

```
racadm getconfig -g cfgUserAdmin -i 6

racadm config -g cfgUserAdmin -o cfgUserAdminPassword -i 6 <password>

racadm set iDRAC.Users.6.Password <password>

racadm get iDRAC.Users

racadm getconfig -u <user>

ipmitool lan print

ipmitool user list 1

ipmitool user set password 6 <password>

Default idrac password -- root /calvin
```
