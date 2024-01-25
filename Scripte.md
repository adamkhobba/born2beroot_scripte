# Script
```shell
#!/bin/bash

Arch=$(uname -a)
CPUP=$(lscpu | grep Socket | awk '{printf $2}')
vCPU=$(lscpu | grep "CPU(s)" | awk 'NR==1{print $2}')
MU=$(free   | awk '/^Mem/ {printf "%d/%dMB (%.2f%%)",$3/2048,$2/2048,$3/$2*100}')
DU=$(df -h --total | awk '/^total/ {printf "%d/%dGb (%d%%)",$3*1024,$2,$3/$2*100}')
CPUL=$(mpstat 1 1| awk '/Average:/ {print 100 - $12}')
LB=$(who -b | awk '{print $3" "$4}')
LVM=$(sudo lvs)
TCP=$(netstat -an | grep ESTABLISHED |  wc -l)
UL=$(users | tr ' ' '\n' | sort -u | wc -l)
IP=$(hostname -I)
MAC=$(ip address| grep ether | awk '{print $2 }')
sudo=$(sudo journalctl _COMM=sudo -q| grep COMMAND | wc -l)
check="no"

if [ "$LVM" != "\0" ];
then
	check="yes"
fi

wall "	#Architecture : $Arch
	#CPU physical : $CPUP
	#vCPU: $vCPU
	#Memory Usage : $MU
	#Disk Usage : $DU
	#CPU load : $CPUL%
	#Last boot : $LB
	#LVM use : $check
	#Connections TCP : $TCP ESTABLISHED
	# User log : $UL
 	#Network : $IP ($MAC)
	#sudo : $sudo cmd"
```
