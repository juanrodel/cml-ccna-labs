##### **Goal**

Configure VLANs and inter-VLAN routing, DHCP, NAT, ACL and port security.

##### **Topology diagram**

!\[Network diagram for lab](lab1-vlans-routing-nat/diagram.png)

##### **Addressing Table**

###### **VLANs and Subnets**

|VLAN|Name|Subnet|Gateway (R1)|Addressing|DHCP Range|
|-|-|-|-|-|-|
|10|Staff|10.0.10.0/24|10.0.10.1|DHCP|10.0.10.11 - 10.0.10.254|
|20|Guest|10.0.20.0/24|10.0.20.1|DHCP|10.0.20.11 - 10.0.20.254|
|30|Servers|10.0.30.0/24|10.0.30.1|Static|None|

The first 10 addresses of VLANs 10 and 20 are excluded from DHCP to leave room for static devices.

###### **Device Interfaces**

|Device|Interface|Connects To|Address / VLAN|
|-|-|-|-|
|R1|e0/0|External Connector|DHCP from outside (NAT outside)|
|R1|e0/1.10|SW1 e0/0 (trunk)|10.0.10.1/24 (VLAN 10)|
|R1|e0/1.20|SW1 e0/0 (trunk)|10.0.20.1/24 (VLAN 20)|
|R1|e0/1.30|SW1 e0/0 (trunk)|10.0.30.1/24 (VLAN 30)|
|SW1|e0/0|R1 e0/1|Trunk (VLANs 10, 20, 30)|
|SW1|e0/1|PC1|Access, VLAN 10|
|SW1|e0/2|PC2|Access, VLAN 20|
|SW1|e0/3|PC3|Access, VLAN 30|

###### **Hosts**

|Host|VLAN|IP Address|Gateway|DNS|
|-|-|-|-|-|
|alpine-0|10 (Staff)|DHCP (10.0.10.x)|10.0.10.1|1.1.1.1, 8.8.8.8|
|alpine-1|20 (Guest)|DHCP (10.0.20.x)|10.0.20.1|1.1.1.1, 8.8.8.8|
|alpine-2|30 (Servers)|10.0.30.10/24|10.0.30.1|1.1.1.1|

###### **Test Matrix**

|From|To|Expected|
|-|-|-|
|alpine-0 (Staff)|alpine-2 (10.0.30.10)|Works|
|alpine-1 (Guest)|alpine-2 (10.0.30.10)|Blocked|
|alpine-1 (Guest)|alpine-0|Blocked|
|alpine-1 (Guest)|10.0.20.1 (gateway)|Works|
|alpine-1 (Guest)|8.8.8.8|Works|

\####**Requirements**

CML Free

\####**How to import**

Import topology.yaml into CML and start the lab

\####**What I learned**

This was my first network project in a while, so it has been fairly simple as I have reviewed some basic networking concepts while creating this lab.

