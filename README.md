# Hatchetfield Axe-Throwing & Recreation Venues: Campus LAN Design

Case Study No. 122, Computer Networks, School of Future Tech, ITM Skills University.

**Author:** Yuvraj Mishra (Roll / ID 150096725036)
**Tool:** Cisco Packet Tracer
**Project type:** Enterprise campus LAN design and implementation

## Overview

Hatchetfield runs three venues (Downtown, Westside and Harbor District) connected in a triangle. The original network was one flat broadcast domain. That caused broadcast storms on league nights and let customers' devices reach the lane-safety sensors.

This project redesigns the switching layer with VLAN segmentation, secure trunking, inter-VLAN routing with an access policy, loop-free Spanning Tree, link aggregation and Layer-2 port security.

## Features

| Area | Implementation |
|---|---|
| VLAN segmentation | VLANs 15, 25, 35, 45, 925 and an unused native VLAN 999 |
| Secure trunking | 802.1Q trunks, DTP disabled, VLAN pruning, native VLAN 999 to mitigate VLAN hopping |
| Inter-VLAN routing | Router-on-a-stick with extended ACLs; Guest can never reach Lane-Safety-Systems |
| Loop prevention | Rapid PVST+ with SW-HarborDistrict as root bridge (priority 4096) |
| Link aggregation | LACP EtherChannel (Po1) between SW-HarborDistrict and SW-Downtown |
| Port security | Max 2 MACs, violation mode `restrict`, on Westside Guest ports |
| VTP | Transparent mode recommended |
| Failure test | Simulated HarborDistrict-Westside link failure with STP re-convergence documented |

## VLAN Plan

| VLAN | Name | Location | Typical devices |
|---|---|---|---|
| 15 | Lane-Safety-Systems | Downtown, Westside | Lane sensors, scoring terminals |
| 25 | Staff-POS | All sites | POS terminals, staff tablets |
| 35 | Admin | Harbor District | HQ finance / admin |
| 45 | IT-Mgmt | All sites | Switch / AP management |
| 925 | Guest | Westside | Public lobby Wi-Fi |
| 999 | Native-Unused | All trunks | Deliberately unused |

## Topology

- 3 x Cisco Catalyst 2960-24TT switches: `SW-HarborDistrict`, `SW-Downtown`, `SW-Westside`
- 1 router providing router-on-a-stick inter-VLAN routing at Harbor District
- Triangle of inter-switch trunks; the two Harbor District-Downtown links form the EtherChannel
- Exactly one port is blocked by STP in the triangle

## Repository Contents

| File | Description |
|---|---|
| `Hatchetfield_Case_Study_122_Report_with_screenshots.docx` | Full case study report (editable) |
| `Hatchetfield_Case_Study_122_Report_with_screenshots.pdf` | Full case study report (PDF) |
| `Hatchetfield_Case_Study_122.pptx` | Summary presentation |
| `*.pkt` | Packet Tracer project file (add your own file here) |

## How to Run

1. Install [Cisco Packet Tracer](https://www.netacad.com/).
2. Open the `.pkt` project file.
3. Wait for the links to turn green, then verify with:
   ```
   show vlan brief
   show interfaces trunk
   show spanning-tree vlan 25
   show etherchannel summary
   show port-security address
   show access-lists
   ```
4. Run the ping tests described in the report (Section 16) to confirm the access policy.

## Report Sections

Abstract, Introduction, Problem Statement, Objectives, Requirements and Assumptions, Topology, VLANs, Trunking, Inter-VLAN Routing and ACLs, IP Addressing, Spanning Tree, EtherChannel, Port Security, VTP Memo, Link-Failure Report, Testing and Verification, OSI Mapping, Deliverables Checklist, Conclusion, Future Improvements, Appendix A (device configurations).

## Future Improvements

See Section 20 of the report (for example Layer-3 switching, DHCP and a guest Wi-Fi portal).

## License

Submitted as coursework for ITM Skills University. Add a license of your choice before publishing.
