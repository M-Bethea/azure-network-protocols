<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


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

- Observe ICMP
- Observe SSH
- Observe DHCP
- Observe DNS
- Observe RDP

<h2>Actions and Observations</h2>

<p>
(Create Resources) <br>
A. Create a Resource Group <br>
&nbsp; i. Create a Windows 10 Pro Virtual Machine (VM), Version 22H - x64 Gen2: Virtual Machine -> Create -> Azure Virtual Machine <br>
&nbsp; &nbsp; ii. While creating the VM, select the Resource Group you just created and the same Region. Size: 2 VPU's, 16GIB Memory <br>
&nbsp; &nbsp; &nbsp; iii. While creating the VM, it will create a new Virtual Network (Vnet) and Subnet in the Network tab. Checkmark licensing  <br>
B. Create a Linux (Ubuntu) 20.04 LTS x64 Gen2 VM. Under Administrator Account -> Authentication Type-> Password -> Create Username and Password <br>
&nbsp; i. While create the VM, select the same Resource Group and Vnet as your Windows VM <br>
&nbsp; &nbsp; ii. Observe the Virtual Network within Network Watcher
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/29dcf385-0740-478d-8ca1-462d3e204013" height="70%" width="70%" alt="VM Setup"/>
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/4a137f0f-cdae-4657-b4c3-2b2810dc48e9" height="70%" width="70%" alt="VM Setup"/>
</p>
<br />

<p>
(Observe ICMP Traffic) <br>
A. Use Remote Desktop to connect to your Windows 10 Virtual Machine <br>
&nbsp; i. Within your Windows 10 Virtual Machine, Install Wireshark by using the browser and searchin "Wireshark download" <br>
&nbsp; &nbsp; ii. Open Wireshark and filter for ICMP traffic only. Under file button:-> "Start Capturing Packets" -> type ICMP into search box -> Enter <br>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/ca7e1b8c-fcef-41e5-89c8-63705bd70a26" height="70%" width="70%" alt="Observing ICMP Traffic"/>
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/94cf9728-9c36-458d-83c1-a310f6d2f7d7" height="70%" width="70%" alt="Observing ICMP Traffic"/>
</p>
B. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM suing CMD: ping (private IP address) <br>
&nbsp; i. Observe ping requests and replies within WireShark <br>
&nbsp; &nbsp; ii. From The Windows 10 VM, open command line (CMD) or PowerShell and attempt to ping a public website (such as www.google.com) and observe the traffic in WireShark <br>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/ee3c6b20-2fbb-4cf6-b08a-7930a500ce91" height="70%" width="70%" alt="Observing ICMP Traffic"/>
</p>
C. Initiate a non-stop ping from your Windows 10 VM to your Ubuntu VM <br>
&nbsp; i. Open the Network Security Group your Ubuntu VM is using and disable incoming (inbound) ICMP traffic: Ubuntu VM -> Network -> Add Inbound Port Rule -> ICMP -> Deny -> Add  <br>
&nbsp; &nbsp; ii. Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity by pinging Ubuntu VM again <br>
&nbsp; &nbsp; &nbsp; iii. Re-enable/Update ICMP traffic for the Network Security Group your Ubuntu VM is using <br>
&nbsp; &nbsp; &nbsp; &nbsp; iv. Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working) <br>
&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; V. Stop the ping activity
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/aa99536a-0b4f-4f54-ac45-cdcb2b20beba" height="70%" width="70%" alt="Observing ICMP Traffic"/>
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/1e81aeb7-2603-46a0-a50c-38a900fe6f71" height="70%" width="70%" alt="Observing ICMP Traffic"/>
</p>
<p>
<br />

<p>
(Observe SSH Traffic) <br>
A. In Wireshark, filter for SSH traffic only: Type "SSH" into search box -> enter -> start capturing packets <br>
&nbsp; i. From your Windows 10 VM, “SSH into” your Ubuntu Virtual Machine (via its private IP address): ssh username@privateIPAddress -> yes (if prompted) -> enter -> password <br>
&nbsp; &nbsp; ii. Type commands (rmdir, mkdir, pwd, etc) into the linux SSH connection and observe SSH traffic spam in WireShark <br>
&nbsp; &nbsp; &nbsp; iii. Exit the SSH connection by typing ‘exit’ and pressing [Enter]
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/6a7858a1-636d-4933-8de5-40b0f3cb6e9b" height="70%" width="70%" alt="Observing SSH Traffic"/>
</p>
<br />

<p>
(Observe DHCP Traffic) <br>
A. In Wireshark, filter for DHCP traffic only <br>
&nbsp; i. From your Windows 10 VM, attempt to issue your VM a new IP address from the command line (ipconfig /renew) <br>
&nbsp; &nbsp; ii. Observe the DHCP traffic appearing in WireShark 
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/c6fa9098-3a4d-442a-9317-13d7a1dba3a5" height="70%" width="70%" alt="Observing DHCP Traffic"/>
</p>
<br />

<p>
(Observe DNS Traffic) <br>
A. In Wireshark, filter for DNS traffic only <br>
&nbsp; i. From your Windows 10 VM within a command line, use nslookup to see what google.com and disney.com’s IP addresses are <br>
&nbsp; &nbsp; ii. Observe the DNS traffic being show in WireShark
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/a3809977-528b-4e60-b235-2067ab4d99f8" height="70%" width="70%" alt="Observing DNS Traffic"/>
</p>
<br />

<p>
(Observe RDP Traffic) <br>
A. In Wireshark, filter for RDP traffic only (tcp.port == 3389) <br>
&nbsp; i. Oserve the immediate non-stop spam of traffic? Why do you think it’s non-stop spamming vs only showing traffic when you do an activity? <br>
&nbsp; &nbsp; ii. Answer: because the RDP (protocol) is constantly showing you a live stream from one computer to another, therefor traffic is always being transmitted. Even by just moving your mouse around
</p>
<p>
<img src="https://github.com/M-Bethea/azure-network-protocols/assets/139162550/ec987acc-1e23-44e6-978f-ae05aa2fe551" height="70%" width="70%" alt="Observing RDP Traffic"/>
</p>
<br />

<p>
A. Cleanup (DON’T FORGET THIS IF YOU WANT TO AVOID BEING CHARGED) <br>
&nbsp; i. Close your Remote Desktop connection <br>
&nbsp; &nbsp; ii. Delete the Resource Group(s) created at the beginning of this lab <br>
&nbsp; &nbsp; &nbsp; iii. Verify Resource Group Deletion
</p>
<br />


