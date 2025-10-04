# 🌐 MPLS VPN DEPLOYMENT: BGP OVER OSPF WITH LIBRENMS INTEGRATION

## Executive Summary
This project simulates a complex **Service Provider core network architecture** using BGP over an OSPF backbone with MPLS for traffic forwarding. This demonstrates expertise in carrier-grade routing, traffic engineering, and essential **Network Operations (NetOps)** functions via NMS integration.

---

## 1. Network Architecture and Protocol Layering
The design employs a three-tiered protocol architecture to provide secure, scalable service:

| Protocol | Role | Skill Demonstrated |
| :--- | :--- | :--- |
| **OSPF** | **Interior Gateway Protocol (IGP).** Used to distribute the loopback addresses of the Provider Edge (PE) routers across the backbone. | Mastery of link-state routing and stable core routing. |
| **MPLS (LDP)** | **Data Plane.** Provides the Label Switched Path (LSP) across the core network, ensuring high-speed label-based traffic forwarding. | Expertise in traffic engineering and LDP configuration. |
| **BGP ** | **Control Plane.** Distributes **costumer networks** between the PE routers, | Advanced knowledge of BGP route reflectors and multiprotocol extensions. |

---

## 2. Key Technical Implementation Details

* **BGP Configuration:** Implemented **iBGP configuration Between R1 and R3 and eBGP between CE and PE routers , integrating OSPF protocol for interface connectivity between multiple routers. and using MPLS protocol for traffic distribution using label switching to ensure fast distribution of packet limiting IGP routing protocol FIB lookup. 

 
## 3. Network Operations (NetOps) and Monitoring

The network was integrated with a simulated **LibreNMS** server to demonstrate operational readiness.

* **Integration:** A dedicated device was configured to connect to the NMS via the **GNS3 Cloud device** to simulate real-world monitoring.
* **Security:** Devices were configured with **SNMPv2c/v3** and restricted via **ACLs** to ensure only the LibreNMS server could securely poll network metrics.

**(Reference the attached image: LibreNMS_Dashboard_Proof.png here to visually confirm successful polling and monitoring.)**

---
## 4. Device and Network Security Hardening

To demonstrate a holistic approach to network management, this repository also includes a dedicated lab focused on implementing strict security controls at both the switch and firewall layers.

### Key Security Implementations:

* **ASA Firewall ACLs:** Configured multiple **Access Control Lists (ACLs)** to strictly control traffic for critical services (TACACS+, Mail, Web) into defined security zones, enforcing the principle of least privilege.
* **Switch Hardening:** Implemented essential Layer 2 security features to mitigate common threats:
    * **Port Security:** Limiting the number of MAC addresses and using **sticky MAC** to prevent unauthorized devices.
    * **BPDU Guard and PortFast:** Mitigating potential Spanning Tree Protocol (STP) manipulation.
    * **Storm Control:** Preventing Denial of Service (DoS) attacks from broadcast storms.
    * **802.1x:** Enabled port-based authentication to manage device access control.
* **Reliable Timing:** Configured **NTP (Network Time Protocol)** for reliable time synchronization across all devices—critical for accurate log correlation during security events.

**(All configuration files for these security features are stored in the dedicated sub-folder: `Security_Hardening_Lab/`)**

interface FastEthernet0/2
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport protected
 spanning-tree portfast
 spanning-tree bpduguard enable
 storm-control broadcast level 10
!
interface FastEthernet0/3
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport protected
 spanning-tree portfast
 spanning-tree bpduguard enable
 storm-control broadcast level 10
!
interface FastEthernet0/4
 switchport port-security maximum 3
 switchport port-security mac-address sticky 
 switchport protected
 spanning-tree portfast
 spanning-tree bpduguard enable
 storm-control broadcast level 10

interface FastEthernet0/1
 switchport mode access
 authentication port-control auto
 dot1x pae authenticator
!
interface FastEthernet0/2
 switchport mode access
 authentication port-control auto
 dot1x pae authenticator
!
logging trap debugging
logging 192.168.1.5
line con 0
 exec-timeout 20 0
 logging synchronous
 login authentication NETACAD
!
line aux 0
!
line vty 0 3
 exec-timeout 0 90
 logging synchronous
 login authentication NETACAD
line vty 4
 exec-timeout 0 0
 transport input none
!
!
ntp authentication-key 10 md5 0822455D0A16 7
ntp authenticate
ntp server 10.1.1.2
