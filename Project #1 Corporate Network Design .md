# Project #1: Corporate Network Design

---

## 1 - Objectives and Requirements

A corporate network is required to connect the Head Office, three branches (Branch 1, Branch 2, Branch 3), and a Server Room. The network must support staff, departments, and server access across multiple locations.

### Requirements:

a. Create a network topology with the main components to support the following:

- Head Office: Departments with VLANs for different teams (VLAN 10, 20, 30, 40).
- Branch 1: Office network with PCs and a printer.
- Branch 2: Office network with PCs and a printer.
- Branch 3: Office network with PCs, laptops, and a printer.
- Server Room: Hosts Web Server, FTP Server, and DNS Server.b. Configure core devices for end-to-end connectivity and access to internal servers.
- Each department/branch must have a separate IP network.
- Switches should be configured with appropriate VLANs and security settings.
- Use dynamic routing (e.g., RIPv2) for internal networks and static routing for server access.
- Devices in branches should acquire dynamic IP addresses via DHCP where applicable.

### Tasks:

**Task 1**: Plan, design, and prototype the network topology using Cisco Packet Tracer.

**Task 2**: Configure the network with appropriate settings for connectivity and functionality.

---

## 2 - Steps of Solution

1. Design the network topology based on the provided diagram.
2. Power on routers (Router PT, Router 1, Router 2) and switches.
3. Assign clock rates on serial interfaces (e.g., Se0/0, Se0/1).
4. Configure switches with VLANs and IP addresses.
5. Assign IP addresses to all routers, subnetworks, and end devices.
6. Configure DHCP for dynamic IP assignment on branch networks.
7. Implement routing (e.g., RIPv2) for internal connectivity and static routes for servers.

---

## 3 - The Design

![bank enterprise project 2.png](Project%20#1%20Corporate%20Network%20Design/bank_enterprise_project_2.png)

---

## 4 - The Configuration

### Router PT

```bash
en
conf t
interface fa0/0
 no shutdown
interface se0/0
 no shutdown
 clock rate 64000
 ip address 10.10.0.1 255.255.255.252
interface fa0/1
 no shutdown
 ip address 10.10.0.5 255.255.255.252
ip dhcp pool branch1-pool
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 10.0.0.3
ip dhcp pool branch2-pool
 network 192.168.2.0 255.255.255.0
 default-router 192.168.2.1
 dns-server 10.0.0.3
service dhcp
router rip
 version 2
 network 10.10.0.0
 network 192.168.1.0
 network 192.168.2.0

```

### Router 1

```bash
en
conf t
interface fa0/0
 no shutdown
interface se0/0
 no shutdown
 ip address 10.10.0.2 255.255.255.252
interface fa0/1
 no shutdown
 ip address 20.20.0.1 255.255.255.252
ip dhcp pool branch3-pool
 network 192.168.3.0 255.255.255.0
 default-router 192.168.3.1
 dns-server 10.0.0.3
service dhcp
router rip
 version 2
 network 10.10.0.0
 network 20.20.0.0
 network 192.168.3.0

```

### Router 2

```bash
en
conf t
interface fa0/0
 no shutdown
interface se0/0
 no shutdown
 ip address 20.20.0.2 255.255.255.252
interface fa0/1
 no shutdown
 ip address 10.0.0.1 255.255.255.0
ip route 10.0.0.0 255.255.255.0 20.20.0.1
router rip
 version 2
 network 10.0.0.0
 network 20.20.0.0

```

### Switch (Head Office - VLAN 10)

```bash
en
conf t
interface range fa0/1-6
 switchport mode access
 switchport access vlan 10
interface vlan 10
 ip address 192.168.0.1 255.255.255.0
 no shutdown

```

### Switch (Branch 1)

```bash
en
conf t
interface range fa0/1-6
 switchport mode access
 switchport access vlan 1
interface vlan 1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

```

### Switch (Branch 2)

```bash
en
conf t
interface range fa0/1-6
 switchport mode access
 switchport access vlan 1
interface vlan 1
 ip address 192.168.2.1 255.255.255.0
 no shutdown

```

### Switch (Branch 3)

```bash
en
conf t
interface range fa0/1-6
 switchport mode access
 switchport access vlan 1
interface vlan 1
 ip address 192.168.3.1 255.255.255.0
 no shutdown

```

### Switch (Server Room)

```bash
en
conf t
interface range fa0/1-3
 switchport mode access
 switchport access vlan 1
interface vlan 1
 ip address 10.0.0.2 255.255.255.0
 no shutdown

```