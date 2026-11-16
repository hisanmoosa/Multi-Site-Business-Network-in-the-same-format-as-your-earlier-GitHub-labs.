# Incident Notes

## 01 Limerick could not reach Cork

**Symptom:** The Limerick PC could ping 192.168.50.1 locally, but the Cork target was unreachable. The Limerick router also failed to ping its directly connected Dublin peer at 10.255.0.5.

**Checks:** The PC's gateway was initially unset and was corrected to 192.168.50.1. With the direct router ping still failing, I checked interface status. Limerick's Gi0/2 had 10.255.0.6 but showed down/down. A cable check came before any further diagnosis of OSPF.

**Fix:** Corrected the cable between Dublin Gi0/2 and Limerick Gi0/2. The Limerick interface returned to up/up.

**Verification:** `show ip ospf neighbor` showed FULL/BDR with Dublin at 10.255.0.5. `show ip route ospf` showed 192.168.40.0/24 via that same next hop. See the [verification image](../images/ospf-verification.png) and [CLI excerpts](../tests/Observed-Output.txt).

## 02 Sales PC had the wrong DHCP gateway

**Symptom:** PC2 received 192.168.20.21/24, but `ipconfig` showed gateway 192.168.20.0. Cross-VLAN traffic timed out.

**Fix:** Configured the Sales DHCP pool to supply 192.168.20.1. The PC retained the old value until I ran `ipconfig /release` and `ipconfig /renew`.

**Verification:** After renewal, the corrected gateway appeared. The IP address could remain the same while the DHCP gateway option changed.

## 03 Guest could ping its gateway but the allowed website failed

**Symptom:** The guest PC reached 192.168.60.1, but its external web request produced no guest PAT entry.

**Checks:** `show access-lists` showed a TCP port 80 rule aimed at the gateway, 192.168.60.1. The intended HTTP destination was the external server at 198.51.100.10.

**Fix and verification:** Added the correct TCP port 80 permit. The NAT table then showed 192.168.60.11 translated to 203.0.113.1 for the external web server. The external website opened and the intranet remained blocked.

## 04 One EtherChannel member down

**Test:** Confirmed both ports were bundled, then shut Fa0/23 on SW4-Dublin. The test PC could still ping 192.168.10.1 using the remaining member. Restoring both ports returns the lab to its normal state.
