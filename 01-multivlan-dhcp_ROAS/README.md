Cisco Packet Tracer Lab: 

VLANs, Router-on-a-Stick, DHCP Relay, and DHCP Snooping



Project Overview
I built a multi-VLAN network in Cisco Packet Tracer to practice network segmentation, inter-VLAN routing, centralized DHCP, and Layer 2 security.
The topology uses a Cisco 3560 multilayer switch, Cisco 2960 access switches, a router configured for Router-on-a-Stick, and a centralized server providing DHCP services. I separated endpoints into VLAN 10 (DEVOPS), VLAN 20 (FINANCE), and VLAN 30 (SERVERS).
The goal was to get devices in different VLANs communicating through the appropriate gateway, automatically assign IP addresses to clients, and begin implementing DHCP snooping to protect the network against rogue DHCP servers.
What I Configured
•	VLANs: Created separate broadcast domains for DEVOPS, FINANCE, and the server network.
•	Access ports: Assigned PC-facing switchports to the correct VLANs.
•	Trunk ports: Configured switch-to-switch and router-facing links to carry the required VLANs.
•	Router-on-a-Stick: Configured 802.1Q subinterfaces on the router to provide default gateways for each VLAN.
•	DHCP relay: Used ip helper-address on the client VLAN subinterfaces so DHCP requests could reach a server on a different subnet.
•	Centralized DHCP: Configured DHCP pools with the correct network, subnet mask, starting address, default gateway, and DNS server.
•	DHCP snooping: Enabled snooping on the relevant switches and worked through trusted versus untrusted interfaces.
Troubleshooting and Lessons Learned
This lab involved more troubleshooting than I initially expected, which made it useful practice.
1. Troubleshooting VLAN 30 connectivity
Initially, the router could reach its own 10.0.30.1 interface but could not ping the DHCP server at 10.0.30.10. I checked the server’s static IP configuration, default gateway, and the switchport assigned to VLAN 30. Once connectivity was restored, I could move on to testing DHCP itself.
2. Verifying Router-on-a-Stick configuration
I learned to distinguish the physical router interface from its VLAN subinterfaces. The physical interface connected to the switch carries tagged VLAN traffic, while the subinterfaces provide the individual VLAN gateway addresses. The separate routed link to the upstream router does not need those VLAN subinterfaces.
3. Troubleshooting DHCP address assignment
Even after basic connectivity was working and the ip helper-address configuration was correct, the PCs were not receiving addresses. I eventually found that the end devices were still configured with static IP settings instead of DHCP. Switching the clients to DHCP allowed them to request their addresses automatically.
That was a good reminder to check the endpoint configuration before assuming the network infrastructure is broken.
4. Understanding ARP and packet behavior
While testing pings between devices in the same VLAN, I observed packets appearing at multiple devices in Packet Tracer. This helped me understand how ARP broadcasts are flooded within a VLAN, how switches learn MAC addresses, and how subsequent unicast traffic is forwarded toward its destination.
5. Implementing DHCP snooping
I also worked on DHCP snooping across the access and distribution layers. This introduced me to the importance of configuring trusted interfaces along the legitimate DHCP-server response path while keeping client-facing ports untrusted. I learned that a zero-entry snooping binding table does not automatically indicate a misconfiguration; clients must complete DHCP exchanges that the switch can inspect and record.
What I Learned
This project helped reinforce several core networking concepts:
•	How VLANs separate traffic into distinct broadcast domains.
•	How access ports differ from trunk ports.
•	How 802.1Q tagging and router subinterfaces enable inter-VLAN routing.
•	How DHCP relay allows clients to use a centralized DHCP server across subnets.
•	How to troubleshoot systematically using connectivity tests, interface status, VLAN assignments, trunk verification, and configuration inspection.
•	How ARP, MAC address learning, and DHCP snooping fit into normal network operations.
Most importantly, I practiced troubleshooting the network layer by layer instead of changing configurations at random. I learned to verify physical and Layer 2 connectivity first, confirm routing and relay configuration next, and then investigate DHCP pool settings and client behavior.
This was a hands-on learning project built to strengthen my CCNA networking fundamentals and develop a more realistic approach to network troubleshooting.
Tools: Cisco Packet Tracer, Cisco IOS CLI, VLANs, 802.1Q, Router-on-a-Stick, DHCP, DHCP relay, DHCP snooping, ARP, and MAC address tables.

