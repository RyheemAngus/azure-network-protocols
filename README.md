<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines, Vnets, NSGs)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Powershell 7.x
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Azure Resource Group and Virtual Network
- Deploy Windows Server 2022 VM
- Deploy Windows 10 VM 
- Configure Active Directory Domain Services
-  ICMP Traffic Control
Configure NSG rules to block/allow ping requests
Observe real-time traffic changes in Wireshark
- SSH Session Inspection
Capture encrypted SSH handshake and commands
Analyze TCP port 22 traffic patterns
- DHCP Lease Process
Trigger IP renewal and capture 4-way handshake
- DNS Query Analysis
Monitor UDP port 53 during domain resolution
- RDP Stream Observation
Examine persistent TCP connection characteristics

<h1>Deployment and Configuration Steps</h1>

<p>
<h2>Azure VM Deployment</h2>

Create a Resource Group

Navigate to Azure Portal.
Resource Groups → Create → Name: Networking-Lab-RG → Region: (East US 2).

Create Virtual Network (VNet)

During Windows Server VM creation:
Virtual Network: Create new (AD-VNet)
Subnet: Default (10.0.0.0/24)
<p>
<img width="1370" height="495" alt="Image" src="https://github.com/user-attachments/assets/c1832eb6-22b4-4775-ae04-5eacc650db65" />
</p>
</p>
<br />

<p>
<h2>Virtual Machine Deployment</h2>

Windows 10 Client VM

Setting	Value
Name	win10-client
Image	Windows 10 Pro, 21H2
VNet	Lab2-Vnet
NSG Rules	Auto-created RDP (3389)
<p>
<img width="1052" height="725" alt="Image" src="https://github.com/user-attachments/assets/819b044d-a9e4-492b-9fbc-738118378cfc" />
</p>

Ubuntu Linux VM

Setting	Value
Name	ubuntu-test
Authentication	Username/Password
IP Assignment	Dynamic (10.0.0.x)
<p>
<img width="1353" height="998" alt="Image" src="https://github.com/user-attachments/assets/0ad0c7f5-ea5c-4081-8d8e-de4bb8850e40" />
</p>
</p>
<br />

<p>
<h2>Tool Installation & Network Validation</h2>

PowerShell 7 Installation

Wireshark Installation

This will enable us to observe the traffic coming from and going to the virtual machines

<p>
<img width="1145" height="647" alt="Image" src="https://github.com/user-attachments/assets/3519e008-9c0c-4d63-a5ff-dd635a827d1f" />
</p>

<p>
<h2>ICMP Traffic with NSG Rules</h2>

Initiate Ping Test

Windows 10 VM:
Test-Connection
<p><img width="2240" height="1260" alt="Image" src="https://github.com/user-attachments/assets/27fb81dd-da01-414e-bc5d-0b061677d3d5" />
</p>
Block ICMP in NSG:
Azure Portal → Ubuntu VM's NSG → Add inbound rule:
Deny ICMP (Priority: 1000)

Observe in Wireshark:
Filter: icmp

Expected: Timeout errors in PowerShell; dropped packets in Wireshark

Re-enable ICMP: Modify NSG rule to Allow ICMP

Expected: Successful replies resume
<p>
<img width="2239" height="1124" alt="Image" src="https://github.com/user-attachments/assets/5a8f50a2-f5d6-48d4-b2f3-2ff267dcbf87" />
</p>
</p>
<br />

<p>
<h2>SSH Traffic Inspection</h2>

Capture SSH Traffic

Wireshark Filter: ssh
Connect via PowerShell:

ssh labuser@10.0.0.5
Run commands (ls, pwd)

Key Observations: Encrypted packet exchanges during login/commands
<p>
<img width="2240" height="1260" alt="Image" src="https://github.com/user-attachments/assets/83d30599-7f05-486d-a041-1452ddf089ff" />
</p>
</p>
<br />

<p>
<h2>DHCP Traffic Analysis</h2>

Trigger DHCP Renewal

Wireshark Filter: dhcp

Windows 10 VM (Admin):
powershell
ipconfig /release
ipconfig /renew

Observe:
DHCP Discover, Offer, Request, ACK packets
<p><img width="2240" height="1260" alt="Image" src="https://github.com/user-attachments/assets/031bbdf2-d145-4383-b48f-9d21dc5afeaa" />
</p>
</p>
<br />

<p>
<h2>DNS Traffic Observation</h2>

Query DNS Servers

Wireshark Filter: dns
PowerShell:
powershell
nslookup google.com
nslookup disney.com

Expected:
UDP port 53 queries/responses
<p>
<img width="585" height="724" alt="Image" src="https://github.com/user-attachments/assets/1d4a458c-c185-4892-8206-63c978f48f2f" />
</p>
</p>
<br />

<p>
<h2>RDP Traffic Patterns</h2>

Monitor RDP

Wireshark Filter: tcp.port == 3389

Observation:
Continuous TCP packets (RDP maintains persistent connection)

RDP maintains a persistent TCP connection (port 3389) for:

- Screen refresh updates

- Input synchronization

- Keepalive packets
<p>
<img width="1399" height="769" alt="Image" src="https://github.com/user-attachments/assets/b0dd88eb-8711-4939-bdf0-6713899d565e" />
</p>
</p>
<br />
