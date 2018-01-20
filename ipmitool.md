# show sensors

`ipmitool sdr`

# get power stats

`ipmitool dcmi power reading`

# asrock fan control (tested with C2750D4I and E3C226D2I)

`ipmitool raw 0x3a 0x01 0xAA 0xBB 0xCC 0xDD 0xEE 0xFF 0xGG 0xHH`

AA: CPU_FAN1  
BB: CPU_FAN2  
CC: REAR_FAN1  
DD: REAR_FAN2  
EE: FRNT_FAN1  
FF: FRNT_FAN2  
GG: 0x00 (?)  
HH: 0x00 (?)  

0x00 smart fan(bios configured)  
0x01 stoped fan  
0x02 lower rpm value  
......  
0x64 max rpm value  

# fru edit

backup fru information: `ipmitool fru read 0 /root/fru.bin`

write fru information: `ipmitool fru write 0 /root/fru.bin`

generate fru information: https://github.com/genotrance/fru-tool

# set ip address

port number depends on board:

- 8 is the dedicated port on the asrock boards i had to test
- 1 is the dedicated port on the supermicro boards i had to test

```
ipmitool lan set 8 ipsrc static
ipmitool lan set 8 ipaddr 10.25.11.249
ipmitool lan set 8 netmask 255.255.255.0
ipmitool lan set 8 defgw ipaddr 10.25.11.1
ipmitool lan set 8 ipaddr 10.25.11.249
```

or

`ipmitool lan set 8 ipsrc dhcp`



