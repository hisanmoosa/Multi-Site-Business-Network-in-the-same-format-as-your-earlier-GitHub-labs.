# Build Order

1. Cable the Dublin router, switch and two PCs. Verify basic LAN connectivity.
2. Move the PCs to IT VLAN 10 and Sales VLAN 20. Configure the router trunk and subinterfaces. Test traffic between VLANs.
3. Add the IT and Sales DHCP pools on R1. Check the lease, mask and gateway on each PC. Release and renew a lease after changing options.
4. Add the Cork and Limerick branches. Ping both ends of each /30 transit link before configuring OSPF area 0. Verify learned routes and test PC-to-PC reachability.
5. Add SRV-Dublin on VLAN 30. Enable HTTP and DNS. Create an A record for `intranet.company.test` pointing to 192.168.30.10. Check HTTP by IP first, then by name. Advertise VLAN 30 through OSPF and give clients the DNS server address.
6. Connect R4-ISP through Dublin-Switch Fa0/4 in VLAN 90. Add the external server behind R4. Enable HTTP on that server. Configure Dublin's default route and PAT, then originate the default through OSPF. Check the browser and NAT translations from the three offices.
7. Add guest VLAN 60. Apply the extended guest ACL inbound on R1's Gi0/0.60 and add the guest subnet to the PAT ACL. Verify both the permitted external website and the blocked intranet.
8. Bundle Fa0/23 and Fa0/24 using LACP between Dublin-Switch and SW4. Connect the IT test PC to SW4 Fa0/1. Shut one member and recheck the gateway ping. Restore it. For the separate STP exercise, use Fa0/22 on both switches and repeat with both LACP members shut, then restore them.
9. For management and backups, configure SSH access from IT only and enable Syslog and TFTP services on SRV-Dublin. The capture checklist in `tests/Verification.md` gives the expected results.
10. Save each device configuration and the Packet Tracer file. Keep the addressing table, screenshots and incident notes with the lab.
