# Project #3: Campus Network

---

---

## 1 - Objectives and Requirements

<aside>
📌

Albion University is a large university with two campuses 20 miles apart. The university's students and staff are distributed in four faculties: Health and Sciences, business, engineering/Computing, and Art/Design. Each member of staff has a PC, and students have access to PCs in the labs.

### **Requirements:**

a. Create a network topology with the main components to support the following:

- Main campus:
    - Building A: Administrative staff in the departments of management, HR and finance. The admin staff PCs are distributed in the building offices and it is expected that they will share some networking equipment (Hint: use of VLANs is expected here). The Faculty of Business is also situated in this building
    - Building B: Faculty of Engineering and Computing and Faculty of Art and Design
    - Building C: Students' labs and IT department. The IT department hosts the University Web server and other servers
    - There is also an email server hosted externally on the cloud.
- Smaller campus:
    - Faculty of Health and Sciences (staff and students' labs are situated on separate floors)

b. You will be expected to configure the core devices and few end devices to provide end-to-end connectivity and access to the internal servers and the external server.

- Each department/faculty is expected to be on its own separate IP network
- The switches should be configured with appropriate VLANs and security settings
- RIPv2 will be used to provide routing for the routers in the internal network and static routing for the external server.
- The devices in building A will be expected to acquire dynamic IP addresses from a router-based DHCP server

### Tasks:

***Task 1:*** Your task is to plan, design, and prototype the network topology for Albion University's network using Cisco Packet Tracer. Formative feedback will be given on this task in week 6.

***Task 2*:** Configure in Packet Tracer the network with appropriate settings to achieve the connectivity and functionalities specified in the requirements

</aside>

---

# 2-Steps of Solution

<aside>
📌

1 - design the network 

2 - turn on the routers 

3 - turn on the power supply of the multilayer switch 

4 - assign the clock rate on the interfaces se0/1/0 & se0/1/1

5 - make the configuration for the switches → multilayer switch → routers 

6 - configure the IPs of all routers and sub networks 

7 - convert the IP of the hosts to dynamic DHCP

8 - implementing the RIP 

</aside>

---

# 3 - The Design

![Screenshot 2025-03-09 074552.png](Project%20#3%20Campus%20Network/Screenshot_2025-03-09_074552.png)

---

# 4 - The Configuration

<aside>
📌

Router R1 

```bash
en
conf t
in gig0/0 
no sh 
in se0/1/0
no sh 
in se0/1/1
no sh 
do wr 
ex
------------- give it a clock rate ---------------
in se0/1/0
clock rate 6400
in se0/1/1
clock rate 6400
do wr
ex

--------------ip addressing ---------------------------
en 
conf t 
in se0/1/0
ip address 10.10.10.1 255.255.255.252

ex 

in se0/1/0
ip address 10.10.10.5 255.255.255.252

ex 

do wr

------------ ip addressing for vlans ------------------

en 
conf t

in gig0/0.10 

encapsulation dot1Q 10
ip address 192.168.1.1 255.255.255.0

ex

in gig0/0.20 

encapsulation dot1Q 20
ip address 192.168.2.1 255.255.255.0

ex

in gig0/0.30 

encapsulation dot1Q 30
ip address 192.168.3.1 255.255.255.0

ex

in gig0/0.40 

encapsulation dot1Q 40
ip address 192.168.4.1 255.255.255.0

ex

in gig0/0.50 

encapsulation dot1Q 50
ip address 192.168.5.1 255.255.255.0

ex

in gig0/0.60 

encapsulation dot1Q 60
ip address 192.168.6.1 255.255.255.0

ex

in gig0/0.70 

encapsulation dot1Q 70
ip address 192.168.7.1 255.255.255.0

ex

in gig0/0.80 

encapsulation dot1Q 80
ip address 192.168.8.1 255.255.255.0

ex

do wr 

-------------------- configure dhcp service ---------------

service dhcp 

----------- for admin ------------

ip dhcp pool admin-pool
network 192.168.1.0 255.255.255.0
default-router 192.168.1.1 
dns-server 192.168.1.1 
ex 

----------- for hr -----------

ip dhcp pool hr-pool
network 192.168.2.0 255.255.255.0
default-router 192.168.2.1 
dns-server 192.168.2.1 
ex 

----------- for finance ------------

ip dhcp pool finance-pool
network 192.168.3.0 255.255.255.0
default-router 192.168.3.1 
dns-server 192.168.3.1 
ex 

----------- for business -----------

ip dhcp pool business-pool
network 192.168.4.0 255.255.255.0
default-router 192.168.4.1 
dns-server 192.168.4.1 
ex 

----------- for ec ------------

ip dhcp pool ec-pool
network 192.168.5.0 255.255.255.0
default-router 192.168.5.1 
dns-server 192.168.5.1 
ex 

----------- for ad -----------

ip dhcp pool ad-pool
network 192.168.6.0 255.255.255.0
default-router 192.168.6.1 
dns-server 192.168.6.1 
ex 

----------- for lab ------------

ip dhcp pool lab-pool
network 192.168.7.0 255.255.255.0
default-router 192.168.7.1 
dns-server 192.168.7.1 
ex 

----------- for it -----------

ip dhcp pool it-pool
network 192.168.8.0 255.255.255.0
default-router 192.168.8.1 
dns-server 192.168.8.1 
ex 

do wr

---------------- implementing RIP ------------
en 
conf t
router rip 
version 2
network 10.10.10.0
network 10.10.10.4
network 192.168.1.0
network 192.168.2.0
network 192.168.3.0
network 192.168.4.0
network 192.168.5.0
network 192.168.6.0
network 192.168.7.0
network 192.168.8.0

ex 

do wr
```

</aside>

<aside>
📌

Router R2

```bash
en
conf t
in gig0/0 
no sh 
in se0/1/0
no sh 
do wr 
ex

--------------ip addressing ---------------------------
en 
conf t 
in se0/1/0
ip address 10.10.10.2 255.255.255.252

ex 

do wr

------------ ip addressing for vlans ------------------

in gig0/0.90 

encapsulation dot1Q 90
ip address 192.168.9.1 255.255.255.0

ex

in gig0/0.90 

encapsulation dot1Q 100
ip address 192.168.10.1 255.255.255.0

ex

do wr 

-------------------- configure dhcp service ---------------
----------- for staff ------------
service dhcp 

ip dhcp pool Staff-pool
network 192.168.9.0 255.255.255.0
default-router 192.168.9.1 
dns-server 192.168.9.1 
ex 

do wr 

----------- for stud_lab -----------

ip dhcp pool studlb-pool
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1 
dns-server 192.168.10.1 
ex 

do wr 

---------------- implementing RIP ------------
en 
conf t
router rip 
version 2
network 192.168.9.0
network 192.168.10.0
network 10.10.10.0

ex 

do wr
```

</aside>

<aside>
📌

Router R3

```bash
en
conf t
in gig0/0 
no sh 
in se0/1/1
no sh 
do wr 
ex 

--------------ip addressing ---------------------------
en 
conf t 
in se0/1/1
ip address 10.10.10.6 255.255.255.252

ex 

do wr

---------------- implementing RIP ----------------
en 
conf t
router rip 
version 2
network 20.0.0.0
network 10.10.10.4

ex 

do wr
```

</aside>

<aside>
📌

Switches⇒  S1- S10  

note : just change the value of vlan

```bash
en
conf t
in range fa0/1-24 
switchport mode access
switchport access vlan 10
do wr 
ex 
```

</aside>

<aside>
📌

Multilayer Switch D1 

note : you implement every vlan with the interface connected with it 

```bash
en
conf t
in gig1/0/2
switchport mode access
switchport access vlan 10
ex 

in gig1/0/3 
switchport mode access
switchport access vlan 20
ex

in gig1/0/4 
switchport mode access
switchport access vlan 30
ex

in gig1/0/5 
switchport mode access
switchport access vlan 40
ex

in gig1/0/6 
switchport mode access
switchport access vlan 50
ex

in gig1/0/7 
switchport mode access
switchport access vlan 60
ex

in gig1/0/8 
switchport mode access
switchport access vlan 70
ex

in gig1/0/9 
switchport mode access
switchport access vlan 80
ex

do wr 

------------------------------------

in gig1/0/1
switchport trunk encapsulation dot1q

switchport mode trunk 

ex 

do wr
```

</aside>

<aside>
📌

Multilayer Switch D2 

note : you implement every vlan with the interface connected with it 

```bash
en
conf t
in gig1/0/2
switchport mode access
switchport access vlan 90
ex 

in gig1/0/3 
switchport mode access
switchport access vlan 100
ex

do wr 

----------------------------------------

in gig1/0/1
switchport trunk encapsulation dot1q

switchport mode trunk 

ex 

do wr
```

</aside>

<aside>
📌

Server ⇒ Email Server 

in the configuration 

Desktop ⇒ IP Configuration 

ipv4 address ⇒ 20.0.0.2

subnet mask ⇒ 255.255.255.252

default gate way ⇒ 20.0.0.1

</aside>

---

<aside>
📌

For more details : 

you can watch this video → [https://youtu.be/qIbhkmTB8Q8?si=VKgMmYVlmE1Ht624](https://youtu.be/qIbhkmTB8Q8?si=VKgMmYVlmE1Ht624)

</aside>