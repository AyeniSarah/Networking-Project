# NetCore Systems Ltd. Departmental Network & ACL Implementation

This project designs and simulates a multi-department enterprise network for NetCore Systems Ltd. using Cisco Packet Tracer. It implements inter-subnet routing, automated IP assignment via DHCP, centralized HTTP and FTP services, and an Extended Access Control List (ACL) to enforce network security policies between departments.

## Table of Contents

* Project Overview
* Network Topology
* Tools and Technologies
* Configuration Steps
* Results and Findings
* Author

## Project Overview

The objective of this project is to build a segmented, functional local area network for **NetCore Systems Ltd.** supporting three core departments: **IT**, **Sales**, and **HR**. The infrastructure enables cross-departmental communication, centralizes web and file transfer services on an IT server, automates client IP addressing, and enforces a targeted security restriction that blocks **ICMP** (`ping`) traffic from the Sales network to the IT network while preserving access to the IT server's **HTTP** and **FTP** services.

## Network Topology

The network uses a star-like hierarchical topology centered around a **Cisco 2911 Router** (`Router0`) connecting three separate **Cisco 2960-24TT Switches** (`Switch0`, `Switch1`, and `Switch2`), each dedicated to a specific department:

* **IT Department (`192.168.10.0/24`)**: Connected to `Router0` via `GigabitEthernet0/0` (`192.168.10.1`). `Switch0` (`Fa0/4`) links to `Server0` (`192.168.10.2`, connected to `Fa0/1`), `PC0` (`192.168.10.11`, connected to `Fa0/2`), and `PC1` (`192.168.10.12`, connected to `Fa0/3`).


* **Sales Department (`192.168.20.0/24`)**: Connected to `Router0` via `GigabitEthernet0/1` (`192.168.20.1`). `Switch1` (`Fa0/3`) connects to `PC2` (`192.168.20.11`, connected to `Fa0/1`) and `PC3` (`192.168.20.12`, connected to `Fa0/2`).


* **HR Department (`192.168.30.0/24`)**: Connected to `Router0` via `GigabitEthernet0/2` (`192.168.30.1`). `Switch2` (`Fa0/3`) connects to `PC4` (`192.168.30.11`, connected to `Fa0/1`) and `PC5` (connected to `Fa0/2`).



## Tools and Technologies

* **Cisco Packet Tracer**: Network simulation and topology modeling environment


* **Cisco 2911 Router**: Inter-network routing, DHCP server pools, and Extended ACL enforcement (`Router0`)


* **Cisco 2960-24TT Switches**: Layer 2 departmental switching (`Switch0`, `Switch1`, `Switch2`)


* **Cisco Server-PT**: Centralized application hosting for **HTTP** and **FTP** services (`Server0`)


* **End Devices (PC-PT)**: Six client workstations (`PC0` through `PC5`) configured for dynamic IP addressing


* **Networking Protocols & Services**: **IPv4**, **DHCP**, **ICMP**, **HTTP**, **FTP**, and **Extended IP Access Control Lists (ACLs)**



## Configuration Steps

1. **Build the Network Topology**: Place `Router0` (Cisco 2911), three Cisco 2960 switches (`Switch0`–`Switch2`), `Server0`, and six client workstations (`PC0`–`PC5`) in Cisco Packet Tracer, and connect them using straight-through copper cables according to the departmental port assignments.


2. **Configure the IT Server (`Server0`)**: Assign a static IP address (`192.168.10.2`), subnet mask (`255.255.255.0`), and default gateway (`192.168.10.1`). Enable the **HTTP** service and customize `index.html` with the NetCore Systems welcome banner. Enable the **FTP** service and configure user credentials (`technova` with password `cisco123` and `RWDNL` permissions).


3. **Configure Router Interfaces**: Access the CLI on `Router0`, enter global configuration mode (`configure terminal`), and assign IP addresses to `GigabitEthernet0/0` (`192.168.10.1`), `GigabitEthernet0/1` (`192.168.20.1`), and `GigabitEthernet0/2` (`192.168.30.1`) with subnet mask `255.255.255.0`, bringing each interface up with `no shutdown`. Verify interface states using `show ip interface brief`.


4. **Configure DHCP Services**: On `Router0`, exclude the first ten addresses of each subnet (`192.168.10.1`–`10`, `192.168.20.1`–`10`, `192.168.30.1`–`10`) using `ip dhcp excluded-address`. Create three DHCP pools (`IT-Pool`, `SALES-Pool`, and `HR-Pool`) defining each network and `default-router`. Set all PCs (`PC0`–`PC5`) to **DHCP** mode and verify address lease assignment via `ipconfig`.


5. **Verify Baseline Connectivity & Services**: Run `ping` tests across departments to confirm full end-to-end connectivity prior to security filtering. Verify web access by navigating to `[http://192.168.10.2](http://192.168.10.2)` from client browsers and test file transfer access via `ftp 192.168.10.2`.


6. **Configure and Apply the Extended ACL**: Create a named extended ACL (`Sales-ACL`) on `Router0` using `ip access-list extended Sales-ACL`. Add rule `deny icmp 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255` to block pings from Sales to IT, followed by `permit ip any any` to allow all other traffic. Apply the ACL inbound on the Sales gateway interface (`GigabitEthernet0/1`) using `ip access-group Sales-ACL in`.


7. **Validate Security Policy & ACL Hits**: Re-test ICMP pings between all departments, verify that Sales PCs can still reach `[http://192.168.10.2](http://192.168.10.2)` and `ftp 192.168.10.2`, and inspect packet match counters on `Router0` using `show access-lists` and `show ip interface gigabitEthernet 0/1`.



## Results and Findings

* **Dynamic IP Allocation**: All workstations successfully leased IPv4 addresses starting at `.11` within their respective departmental subnets (`192.168.10.11`–`12`, `192.168.20.11`–`12`, and `192.168.30.11`) while respecting the `.1`–`.10` static exclusion ranges.


* **Targeted ICMP Restriction**: After applying `Sales-ACL` inbound on `GigabitEthernet0/1`, `ping` attempts from the Sales subnet (`192.168.20.0/24`) to the IT subnet (`192.168.10.0/24`) resulted in `100% packet loss` (`Destination host unreachable`), registering 8 explicit deny matches in `show access-lists`.


* **Preserved Inter-Departmental & Application Traffic**: Bidirectional pings between HR (`192.168.30.0/24`) and IT (`192.168.10.0/24`), as well as between HR and Sales, succeeded with `0% packet loss`. Furthermore, Sales workstations retained full access to `Server0` (`192.168.10.2`) over **HTTP** (displaying the custom NetCore Systems webpage) and **FTP** (authenticating successfully as `technova`), generating 19 permit matches on the ACL.



## Author

**Sarah Ayenigbara**

* GitHub: [github.com/sarahayenigbara](https://www.google.com/search?q=https://github.com/sarahayenigbara)
* Email: sarah.ayenigbara@gmail.com
