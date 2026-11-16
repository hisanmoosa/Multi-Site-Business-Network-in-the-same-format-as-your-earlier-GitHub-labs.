# Project 16 — Multi-Site Business Network

I built this Cisco Packet Tracer lab to connect three offices in Dublin, Cork and Limerick. Dublin is the main site. The branches learn routes through OSPF and share Dublin’s connection to a simulated ISP. I used the lab to practise cable fault diagnosis, guest access rules, and link failure testing.

## Topology

![Original Cisco Packet Tracer topology showing the three offices](images/topology.png)

[Labelled topology](images/topology-labelled.png) · [Limerick OSPF verification](images/ospf-verification.png) · [Full addressing and cable plan](docs/Addressing-and-Ports.md)

The original screenshot was taken before the topology notes were fully updated. The Limerick router uses **Gi0/2** for its uplink. The port label and OSPF output confirm this. The separate STP backup link and SW4 test PC are later exercises and are outside that screenshot.

## What I built

| Area | Setup |
|---|---|
| Dublin LAN | VLANs 10 (IT), 20 (Sales), 30 (Servers) and 60 (Guest), with 802.1Q trunking and router subinterfaces |
| Branch routing | OSPF area 0 over two /30 links. Cork and Limerick learn a default route from Dublin |
| Network services | DHCP for Dublin IT and Sales, plus HTTP and DNS for the internal site on the Dublin server |
| Simulated internet | R1-Dublin uses NAT/PAT to share 203.0.113.1. The external web server is reached through ISP transit VLAN 90 |
| Guest access | Guests can ping their gateway and open the simulated external HTTP site. The inbound ACL blocks access to the intranet |
| Link resilience | LACP EtherChannel between the two Dublin switches. I tested that the IT test PC could still reach its gateway with one member link shut |

## Incident I worked through

Limerick could ping its local gateway but could not reach Cork. From the Limerick router, a ping to its directly connected Dublin peer failed. I found that Limerick’s Gi0/2 was down, corrected the cable connection and verified that the interface came up. The OSPF neighbour returned to FULL and Limerick learned the Cork LAN route via 10.255.0.5.

See [the incident notes](docs/Incident-Notes.md) for the investigation, the Sales DHCP gateway fix, and the guest ACL fault.

## Files

| Folder | Contents |
|---|---|
| [lab](lab) | Location for the original Cisco Packet Tracer file |
| [configs](configs) | Router and switch rebuild references by device, with a separate section for optional management and logging |
| [images](images) | Original topology, a labelled diagram and selected OSPF evidence |
| [tests](tests/Verification.md) | CLI evidence and steps to repeat the checks |
| [docs](docs) | IP addressing, cabling, build order and incident notes |

The project files available here include verified CLI excerpts and rebuild references assembled from the lab work. Add the final saved Packet Tracer file to `lab/` when publishing. The rebuild references are not raw device exports. The full STP backup test, SSH access checks, and Syslog/TFTP checks are listed separately in the test guide so they can be documented with their own results.
