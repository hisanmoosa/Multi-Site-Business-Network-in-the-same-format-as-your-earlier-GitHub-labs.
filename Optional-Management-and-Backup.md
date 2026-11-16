# Management and Backup Exercises

## SSH on R1-Dublin

Set `ip domain-name company.test`. Create the local admin account with `username netadmin privilege 15 secret YOUR_LAB_PASSWORD` and substitute a lab-only password. Run `crypto key generate rsa` and enter 2048 when prompted. Then apply the remaining settings.

```text
ip ssh version 2
access-list 15 permit 192.168.10.0 0.0.0.255
line vty 0 4
 login local
 transport input ssh
 access-class 15 in
 exit
```

Test access from IT and verify that Sales is blocked. Use lab-only credentials in any .pkt copy you share.

## Separate STP backup

On Dublin-Switch, set `spanning-tree vlan 10 priority 4096`. On both switches, configure Fa0/22 as a trunk allowing VLAN 10. On SW4-Dublin, enter `spanning-tree vlan 10 cost 100` on Fa0/22. Connect the matching ports with a copper crossover cable. Verify the alternate path before and after taking both EtherChannel members down.

## Syslog and TFTP

On SRV-Dublin, turn on Syslog and TFTP. On R1, set `service timestamps log datetime msec` and `logging 192.168.30.10`. Create a temporary Loopback99, shut it down, and check the server for an event. Remove the test loopback afterwards.

From R1's privileged prompt, run `copy running-config tftp:`. Use 192.168.30.10 as the remote server and `R1-Dublin-Stage09.cfg` as the destination filename. Check that the file appears in the server list.

## Optional branch DHCP

The evidence shows static .11 addresses for both branch PCs. To switch them to automatic addressing, create the matching pool on each local router, then select DHCP on that branch's PC.

On R2-Cork

```text
ip dhcp excluded-address 192.168.40.1 192.168.40.20
ip dhcp pool CORK
 network 192.168.40.0 255.255.255.0
 default-router 192.168.40.1
 dns-server 192.168.30.10
 exit
```

On the Limerick router

```text
ip dhcp excluded-address 192.168.50.1 192.168.50.20
ip dhcp pool LIMERICK
 network 192.168.50.0 255.255.255.0
 default-router 192.168.50.1
 dns-server 192.168.30.10
 exit
```
