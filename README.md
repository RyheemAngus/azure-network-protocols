<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- 
- 
- 
- 

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
Screenshot: Wireshark with restored ICMP traffic
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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br />

<p>
<h2>RDP Traffic Patterns</h2>

Monitor RDP

Wireshark Filter: tcp.port == 3389

Observation:
Continuous TCP packets (RDP maintains persistent connection)
<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
</p>
<br />

