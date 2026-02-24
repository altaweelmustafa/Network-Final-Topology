\# Network Topology Documentation



\## Overview



\*\*Base Network:\*\* `192.10.17.0/24`  

\*\*Routing Protocol:\*\* OSPFv2 (IPv4) — Both Autonomous Systems  

\*\*Inter-AS Routing:\*\* BGP between AS100 and AS200  



---



\## Autonomous Systems



\### AS100 — Ramallah

\- \*\*Devices:\*\* R1, R2, Switch0, Switch1

\- \*\*End Hosts:\*\* Server1 (www.ramallah.com), Server2 (User@ramallah.com), Laptop0, Server0/DNS (dns.palestine.com)



\### AS200 — Masri Lab

\- \*\*Devices:\*\* R3, Switch2, Multilayer Switch0

\- \*\*End Hosts:\*\* PC0, Laptop1, Printer0, Tablet PC0 (wireless via Access Point0)



---



\## Subnet Allocation



| Subnet | Network Address | Range         | Hosts | Assignment        |

|--------|----------------|---------------|-------|-------------------|

| N1     | 192.10.17.0/26  | .0 – .63      | 62    | AS100 LAN         |

| N2     | 192.10.17.160/30| .160 – .163   | 2     | Point-to-Point Link|

| N3     | 192.10.17.64/27 | .64 – .95     | 30    | AS100 LAN         |

| N4     | 192.10.17.164/30| .164 – .167   | 2     | Point-to-Point Link|

| N5     | 192.10.17.168/30| .168 – .171   | 2     | Point-to-Point Link|

| N6     | 192.10.17.96/27 | .96 – .127    | 30    | \*\*VLAN 20 — EE\*\*  |

| N7     | 192.10.17.128/27| .128 – .159   | 30    | \*\*VLAN 30 — CE\*\*  |

| N8     | 192.10.17.176/28| .176 – .191   | 14    | \*\*VLAN 10 — CS\*\*  |



---



\## VLAN Configuration (Multilayer Switch0 — AS200)



| VLAN ID | Name | Network          |

|---------|------|-----------------|

| VLAN 10 | CS   | 192.10.17.176/28 |

| VLAN 20 | EE   | 192.10.17.96/27  |

| VLAN 30 | CE   | 192.10.17.128/27 |



---



\## Access Control Lists (ACLs)



\### ACL 1 — Block Laptop0 from VLAN 10 (CS)

\- \*\*Type:\*\* Extended ACL

\- \*\*Source:\*\* Laptop0's IP address

\- \*\*Destination:\*\* 192.10.17.176/28 (VLAN 10 — CS)

\- \*\*Action:\*\* Deny

\- \*\*Applied on:\*\* Interface facing Laptop0 (inbound)



```

deny ip host <Laptop0-IP> 192.10.17.176 0.0.0.15

permit ip any any

```



\### ACL 2 — Block HTTP from VLAN 30 (CE)

\- \*\*Type:\*\* Extended ACL

\- \*\*Source:\*\* 192.10.17.128/27 (VLAN 30 — CE)

\- \*\*Destination:\*\* Any

\- \*\*Protocol:\*\* TCP port 80 (HTTP)

\- \*\*Action:\*\* Deny

\- \*\*Applied on:\*\* VLAN 30 SVI or uplink interface (inbound)



```

deny tcp 192.10.17.128 0.0.0.31 any eq 80

permit ip any any

```



---



\## Device Summary



| Device           | Location | Role                        |

|-----------------|----------|-----------------------------|

| R1              | AS100    | Border/OSPF Router (to AS200)|

| R2              | AS100    | Internal OSPF Router         |

| Switch0         | AS100    | Layer 2 Switch               |

| Switch1         | AS100    | Layer 2 Switch               |

| Server0         | AS100    | DNS Server (dns.palestine.com)|

| Server1         | AS100    | Web Server (www.ramallah.com)|

| Server2         | AS100    | Mail Server (User@ramallah.com)|

| Laptop0         | AS100    | End Host                     |

| R0              | AS200    | Border/OSPF Router (to AS100)|

| Switch2         | AS200    | Layer 2 Switch               |

| Multilayer Switch0 | AS200 | Layer 3 Switch (VLAN routing)|

| Access Point0   | AS200    | Wireless AP                  |

| PC0             | AS200    | End Host (VLAN 30/CE)        |

| Laptop1         | AS200    | End Host                     |

| Tablet PC0      | AS200    | Wireless End Host            |

| Printer0        | AS200    | Network Printer              |

