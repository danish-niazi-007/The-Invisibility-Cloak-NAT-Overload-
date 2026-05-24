🕵️‍♂️ The Invisibility Cloak: NAT Overload (PAT) Implementation

📝 Project Overview
In modern enterprise networks, internal end-devices are assigned Private IP addresses (e.g., `192.168.x.x`) which are non-routable over the public Internet. For internal users to access external resources securely, their private identities must be masked.

This project demonstrates the configuration of **NAT Overload**, also known as **Port Address Translation (PAT)**, inside Cisco Packet Tracer. It acts as an "Invisibility Cloak" at the network edge, taking traffic from multiple internal PCs and translating them into a single Public IP address provided by the ISP. This ensures internal network security while providing seamless internet connectivity.

🏗️ Network Topology
* **Inside Network (LAN):** 2x PCs (`192.168.10.0/24` network) connected via a Catalyst 2960 Switch to the edge router.
* **Edge Router:** `R1-Company` functioning as the NAT Gateway.
* **Outside Network (WAN):** An `ISP-Router` bridging the connection to a simulated public `Google-Server` (`8.8.8.8`).

🎯 Objectives Achieved
1. **Public/Private Boundary Definition:** Successfully delineated the network into `ip nat inside` (LAN) and `ip nat outside` (WAN) security zones.
2. **Traffic Identification via ACL:** Configured Standard Access Control Lists (ACLs) to specifically define which internal private subnets are permitted to be translated.
3. **PAT / NAT Overload Activation:** Mapped an entire /24 subnet to a single outbound GigabitEthernet public interface.
4. **Default Routing:** Configured a gateway of last resort (`0.0.0.0 0.0.0.0`) on the company edge router to forward all unknown traffic toward the ISP.

🛠️ Tools & Technologies
* **Environment:** Cisco Packet Tracer
* **Core Concepts:** Network Address Translation (NAT), Port Address Translation (PAT), Standard ACLs, Default Routing
* **Protocols:** ICMP, IP

⚙️ Configuration Commands Used
To build the translation engine on the `R1-Company` edge router, the following core configurations were applied:

```text
enable
configure terminal

! 1. Defining the boundaries
interface gigabitEthernet 0/1
ip nat inside
exit

interface gigabitEthernet 0/0
ip nat outside
exit

! 2. Creating an ACL to identify interesting internal traffic
access-list 1 permit 192.168.10.0 0.0.0.255

! 3. Applying the NAT Overload (The Invisibility Cloak)
ip nat inside source list 1 interface gigabitEthernet 0/0 overload
exit
