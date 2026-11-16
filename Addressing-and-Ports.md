# Addressing and Ports

## Router interfaces

| Device | Interface | IPv4 address | Purpose |
|---|---|---|---|
| R1-Dublin | Gi0/0 | Unassigned | 802.1Q trunk to Dublin-Switch Gi0/1 |
| R1-Dublin | Gi0/0.10 | 192.168.10.1/24 | IT gateway and PAT inside |
| R1-Dublin | Gi0/0.20 | 192.168.20.1/24 | Sales gateway and PAT inside |
| R1-Dublin | Gi0/0.30 | 192.168.30.1/24 | Servers gateway |
| R1-Dublin | Gi0/0.60 | 192.168.60.1/24 | Guest gateway, PAT inside and inbound guest ACL |
| R1-Dublin | Gi0/0.90 | 203.0.113.1/30 | Simulated ISP transit and PAT outside |
| R1-Dublin | Gi0/1 | 10.255.0.1/30 | Cork uplink and PAT inside |
| R1-Dublin | Gi0/2 | 10.255.0.5/30 | Limerick uplink and PAT inside |
| R2-Cork | Gi0/0 | 192.168.40.1/24 | Cork LAN gateway |
| R2-Cork | Gi0/1 | 10.255.0.2/30 | Dublin uplink |
| R3-Limerick | Gi0/0 | 192.168.50.1/24 | Limerick LAN gateway |
| R3-Limerick | Gi0/2 | 10.255.0.6/30 | Dublin uplink |
| R4-ISP | Gi0/0 | 203.0.113.2/30 | Transit via Dublin-Switch Fa0/4 |
| R4-ISP | Gi0/1 | 198.51.100.1/24 | External web server gateway |

## End devices

| Device | IPv4 address | Gateway | Assignment or purpose |
|---|---|---|---|
| PC1-IT | 192.168.10.21/24 | 192.168.10.1 | Observed DHCP address |
| PC2-SALES | 192.168.20.21/24 | 192.168.20.1 | Observed DHCP address |
| SRV-Dublin | 192.168.30.10/24 | 192.168.30.1 | Static HTTP/DNS server |
| PC3-CORK | 192.168.40.11/24 | 192.168.40.1 | Static address observed in PAT output |
| PC4-LIMERICK | 192.168.50.11/24 | 192.168.50.1 | Static address observed in PAT output |
| PC5-GUEST | 192.168.60.11/24 | 192.168.60.1 | Static guest client |
| PC6-IT-TEST | DHCP on VLAN 10 | 192.168.10.1 | EtherChannel test PC. The exact lease was not captured |
| SRV-Internet | 198.51.100.10/24 | 198.51.100.1 | Static simulated external HTTP server |

Dublin's DHCP pools exclude .1 through .20 from each of the IT and Sales subnets. The DNS server for internal clients is 192.168.30.10. The branch PCs used static addresses in the recorded PAT test. A /24 mask is 255.255.255.0 and a /30 mask is 255.255.255.252.

## Cable and switch-port map

| From | To | Role |
|---|---|---|
| R1 Gi0/1 | R2 Gi0/1 | Dublin–Cork L3 link |
| R1 Gi0/2 | R3 Gi0/2 | Dublin–Limerick L3 link |
| R1 Gi0/0 | Dublin-Switch Gi0/1 | Trunk carrying 10, 20, 30, 60 and 90 |
| R2 Gi0/0 | Cork switch Gi0/1 | Cork LAN |
| R3 Gi0/0 | Limerick switch Gi0/1 | Limerick LAN |
| PC3 Fa0 | Cork switch Fa0/1 | Cork client |
| PC4 Fa0 | Limerick switch Fa0/1 | Limerick client |
| PC1 Fa0 | Dublin-Switch Fa0/1 | VLAN 10 IT |
| PC2 Fa0 | Dublin-Switch Fa0/2 | VLAN 20 Sales |
| SRV-Dublin Fa0 | Dublin-Switch Fa0/3 | VLAN 30 Servers |
| R4 Gi0/0 | Dublin-Switch Fa0/4 | VLAN 90 ISP transit |
| PC5 Fa0 | Dublin-Switch Fa0/5 | VLAN 60 Guest |
| R4 Gi0/1 | SRV-Internet Fa0 | Simulated external server segment |
| Dublin-Switch Fa0/23 and Fa0/24 | SW4 matching ports | Two LACP members in Port-channel 1 |
| Dublin-Switch Fa0/22 | SW4 Fa0/22 | Separate STP backup exercise |
| PC6 Fa0 | SW4 Fa0/1 | IT VLAN test client |

Dublin’s WAN links and switch-to-switch links use copper crossover in the lab. Use straight-through for switch-to-router and client access links. Use a compatible or automatically selected cable for the ISP router-to-server link. No switch management IPs were recorded.
