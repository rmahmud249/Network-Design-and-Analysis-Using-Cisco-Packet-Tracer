<img width="769" height="457" alt="Network Design" src="https://github.com/user-attachments/assets/6a2e59f3-0aaa-49b8-bc30-52bdaa3539e4" />
# Network Design and Analysis

A multi-router enterprise network designed and simulated in **Cisco
Packet Tracer** as part of the Computer Networking Lab. The project
demonstrates LAN segmentation, inter-VLAN communication, dynamic
routing, automated IP addressing, NAT/PAT, and application-layer
services.

> **Project type:** Academic networking lab project\
> **Course:** Computer Networking Lab (CSE342.3)\
> **Program:** B.Sc. in Computer Science & Engineering\
> **Institution:** Southeast University\
> **Tools:** Cisco Packet Tracer, Cisco IOS CLI

------------------------------------------------------------------------

## Table of Contents

-   [Project Overview](#project-overview)
-   [Network Topology](#network-topology)
-   [Key Features](#key-features)
-   [Technologies and Concepts](#technologies-and-concepts)
-   [VLAN Design](#vlan-design)
-   [IP Addressing and Configuration
    Highlights](#ip-addressing-and-configuration-highlights)
-   [Verification Commands](#verification-commands)
-   [How to Run the Project](#how-to-run-the-project)
-   [Learning Outcomes](#learning-outcomes)
-   [Project Documentation](#project-documentation)

------------------------------------------------------------------------

## Project Overview

This project focuses on planning, configuring, and analyzing a routed
enterprise-style network in Cisco Packet Tracer. It brings together
multiple routers, switches, end devices, and servers to demonstrate how
separate network segments can communicate through routing while
maintaining logical separation.

The lab work covers VLAN creation and port assignment, trunk links,
router subinterfaces for inter-VLAN routing, DHCP pools, OSPF and RIP
routing, NAT/PAT for translated connectivity, and DNS/HTTP/HTTPS
services.

The supplied lab report documents the configurations and Packet Tracer
results. The topology preview below is an additional network-design
illustration.

## Network Topology

![Network Design and Analysis topology](assets/network-design.webp)

The supplied topology illustration shows a network with multiple routers
and switches, department-based end devices (including Sales, Engineers,
and HR), remote-office segments, and dedicated HTTPS and DNS servers.

The lab report also contains a separate Packet Tracer topology used to
demonstrate the routing, VLAN, DHCP, and server-service configurations.

## Key Features

-   **Multi-router network:** Connects multiple routed network segments.
-   **VLAN segmentation:** Separates Student, Faculty, and Server
    devices into logical networks.
-   **Trunking:** Carries multiple VLANs across a switch-to-switch link.
-   **Inter-VLAN routing:** Uses router subinterfaces and IEEE 802.1Q
    encapsulation.
-   **DHCP:** Automatically provides client IP configuration, default
    gateway, and DNS information.
-   **Dynamic routing:** Demonstrates OSPF and RIPv2 configuration.
-   **NAT/PAT:** Translates private network addresses for external
    connectivity.
-   **Network services:** Configures and tests DNS, HTTP, and HTTPS
    services.
-   **CLI-based configuration:** Uses Cisco IOS commands to configure
    and inspect network devices.

## Technologies and Concepts

  -----------------------------------------------------------------------
  Technology / Concept                Purpose in the project
  ----------------------------------- -----------------------------------
  Cisco Packet Tracer                 Network design and simulation

  VLAN 10 / 20 / 30                   Logical separation of Student,
                                      Faculty, and Server networks

  IEEE 802.1Q                         VLAN tagging on trunk links and
                                      router subinterfaces

  Router-on-a-Stick                   Routing traffic between VLANs

  DHCP                                Automatic IPv4 configuration for
                                      clients

  OSPF                                Link-state dynamic routing; Area 0
                                      is shown in the report

  RIPv2                               Dynamic routing using hop count

  NAT/PAT                             Translation of private addresses to
                                      public addressing

  DNS                                 Resolves domain names to IP
                                      addresses

  HTTP / HTTPS                        Demonstrates web access and secure
                                      web access
  -----------------------------------------------------------------------

## VLAN Design

The lab report defines the following VLANs:

    VLAN ID VLAN Name   Intended Segment
  --------- ----------- ------------------
         10 Student     Student devices
         20 Faculty     Faculty devices
         30 Server      Server devices

Example switch configuration from the lab:

``` cisco
enable
configure terminal

vlan 10
 name Student

vlan 20
 name Faculty

vlan 30
 name Server

interface fastEthernet 0/2
 switchport mode access
 switchport access vlan 10

interface fastEthernet 0/3
 switchport mode access
 switchport access vlan 20

interface fastEthernet 0/4
 switchport mode access
 switchport access vlan 30

interface fastEthernet 0/5
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30

end
write memory
```

> Interface numbers must match the actual Packet Tracer topology. Adjust
> them if your device connections use different ports.

## IP Addressing and Configuration Highlights

The report uses separate IPv4 subnets for the VLANs and routed links.
The documented router-on-a-stick subinterfaces include:

  Subinterface               VLAN Gateway IP      Subnet Mask
  ------------------------ ------ --------------- -----------------
  GigabitEthernet 0/0.10       10 `192.168.1.1`   `255.255.255.0`
  GigabitEthernet 0/0.20       20 `192.168.2.1`   `255.255.255.0`
  GigabitEthernet 0/0.30       30 `192.168.3.1`   `255.255.255.0`

Example subinterface configuration:

``` cisco
interface gigabitEthernet 0/0.10
 encapsulation dot1Q 10
 ip address 192.168.1.1 255.255.255.0

interface gigabitEthernet 0/0.20
 encapsulation dot1Q 20
 ip address 192.168.2.1 255.255.255.0

interface gigabitEthernet 0/0.30
 encapsulation dot1Q 30
 ip address 192.168.3.1 255.255.255.0
```

### DHCP

The report configures separate DHCP pools for the VLAN subnets and
provides each pool with a default gateway and DNS server.

``` cisco
ip dhcp pool DHCP1
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 192.168.3.2

ip dhcp pool DHCP2
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 192.168.3.2

ip dhcp pool DHCP3
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 192.168.3.2
```

### OSPF

The lab report demonstrates OSPF process configuration and advertises
selected networks in **Area 0**. It also shows a default-information
origination command on one router.

``` cisco
router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0
 network 192.168.3.0 0.0.0.255 area 0
 network 192.168.4.0 0.0.0.255 area 0
 network 192.168.5.0 0.0.0.255 area 0
```

The exact OSPF process IDs, router IDs, and network statements vary by
router in the report. Apply the statements that correspond to each
router's connected networks.

### RIPv2

The report also demonstrates RIPv2:

``` cisco
router rip
 version 2
 network 192.168.5.0
 network 192.168.7.0
```

The network statements shown here are an example from the report;
configure each router with its own connected networks.

### NAT/PAT

The ISP section of the report demonstrates an access list, a public NAT
pool, and overload configuration.

``` cisco
access-list 1 permit 192.168.1.0 0.0.0.255

ip nat pool public_pool 209.165.200.225 209.165.200.230 netmask 255.255.255.248

ip nat inside source list 1 pool public_pool overload
```

The report also identifies inside and outside interfaces. Interface
roles and address ranges must be matched to the actual topology before
reusing the configuration.

## Verification Commands

Use these Cisco IOS commands to inspect the configuration and
troubleshoot the simulated network:

  Command                      What it checks
  ---------------------------- -------------------------------------------
  `show ip interface brief`    Interface IP addresses and status
  `show vlan brief`            VLANs and access-port membership
  `show interfaces trunk`      Operational trunk links and allowed VLANs
  `show ip route`              Routing table
  `show ip ospf neighbor`      OSPF neighbor relationships
  `show ip protocols`          Active routing protocols and settings
  `show ip dhcp binding`       DHCP leases assigned to clients
  `show ip nat translations`   Active NAT translations
  `show running-config`        Current device configuration
  `ping <destination-ip>`      End-to-end IP reachability

For application-level testing, use Packet Tracer client web browsers to
access the configured HTTP/HTTPS service and test name resolution
through the configured DNS server.

## How to Run the Project

1.  Install and open **Cisco Packet Tracer**.
2.  Recreate the topology using the routers, switches, PCs, and servers
    shown in the lab report and topology diagram.
3.  Connect devices using appropriate Ethernet links.
4.  Configure device hostnames and interface IP addresses according to
    the topology.
5.  Create VLANs, assign access ports, and configure trunk links.
6.  Configure router subinterfaces and DHCP pools for the VLANs.
7.  Configure the routing protocol required for each part of the lab
    (OSPF or RIPv2).
8.  Configure NAT/PAT and the required DNS, HTTP, and HTTPS services.
9.  Verify interfaces, VLANs, routing tables, DHCP leases, NAT
    translations, and end-to-end connectivity.

**Important:** The lab report contains separate configuration exercises
and example topologies. Do not assume every command block is intended to
run simultaneously on the same device. Follow the relevant topology and
configuration section for each exercise.

## Learning Outcomes

Through this project, I practised:

-   Designing and interpreting multi-router network topologies.
-   Segmenting a LAN with VLANs and configuring trunk links.
-   Enabling communication between VLANs using router subinterfaces.
-   Automating client addressing with DHCP.
-   Understanding and configuring OSPF and RIPv2.
-   Applying NAT/PAT concepts to routed networks.
-   Configuring basic DNS, HTTP, and HTTPS services.
-   Verifying network operation and troubleshooting through Cisco IOS
    commands.

## Project Documentation

The original Computer Networking Lab report contains the topology
diagrams, configuration steps, and Packet Tracer screenshots used for
this project.

For academic integrity and reproducibility, review the original lab
report and adapt the example configurations to the exact device
interfaces and topology before deploying them.

------------------------------------------------------------------------

## Author

**Md. Rasel Mahmud**\
B.Sc. in Computer Science & Engineering\
Southeast University, Bangladesh
