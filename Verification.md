# Verification

## Results and how to repeat them

| Feature | Check | Evidence or expected outcome |
|---|---|---|
| DHCP | Run `ipconfig` on IT and Sales PCs | IT leased .10.21, Sales leased .20.21. Sales gateway corrected to 192.168.20.1 after renewal |
| Limerick transit | Run `show ip interface brief` on R3 | Gi0/2 at 10.255.0.6 came up/up after recabling |
| OSPF | Run `show ip ospf neighbor` and `show ip route ospf` on R3 | FULL/BDR with 10.255.0.5 and a learned Cork LAN route |
| OSPF default route | Run `show ip route` on Cork and Limerick | O*E2 default via 10.255.0.1 on Cork and 10.255.0.5 on Limerick |
| PAT | Browse to `http://198.51.100.10` from each office and check `show ip nat translations` on R1 | Recorded TCP translations from one PC at each office, sharing 203.0.113.1 |
| Guest ACL | From PC5, open `http://198.51.100.10` and `http://192.168.30.10` | External HTTP opened, intranet blocked. Guest TCP translation appears in the table |
| LACP member failure | Run `show etherchannel summary`, shut one member, and ping the IT gateway from PC6 | Both ports initially bundled and gateway ping passed with one member shut |

See [Observed-Output.txt](Observed-Output.txt) for CLI evidence. Test statuses reflect the reported outcomes and supplied output, not an independent re-run of the final Packet Tracer lab.

## Checks to capture on the final copy

- **Intranet DNS:** From a PC with DNS set to 192.168.30.10, open `http://intranet.company.test`. If the site opens by IP alone, check both the DNS A record and client DNS setting.
- **SSH management:** From PC1-IT, run `ssh -l netadmin 192.168.10.1`. Verify successful login, then repeat from PC2-SALES and verify denial.
- **Independent STP backup:** On SW4, run `show spanning-tree vlan 10`. Check Po1 in forwarding state and Fa0/22 blocked. Shut both Po1 members, wait for Fa0/22 to forward, and ping the IT gateway from PC6. Restore both members and verify the standby state again. This test is separate from the confirmed one-member EtherChannel test.
- **Syslog:** Enable the service on SRV-Dublin, configure R1 to send logs to 192.168.30.10, then verify that the server receives an event.
- **Configuration backup:** Enable TFTP on SRV-Dublin and use `copy running-config tftp:` from R1. Verify the named backup appears in the TFTP file list.
