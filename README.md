# Network-Security-Groups-NSGs-and-Inspecting-Network-Protocols
In this tutorial, we will observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create a Resource Group
- Create a Virtual Machine
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic

<h2>Actions and Observations</h2>
</br>
</br>
<h3 align="center">
  Set up the virtual environment
</h3>
</br>
<p>
  First, I created a Resource Group in my Azure subscription to organize all related resources.
</p>
<p>
  <img src="https://i.imgur.com/dOAeXqs.png" height="75%" width="100%" alt="Resource Group"/>
</p>
<p>
 Then, I deployed a Windows 10 virtual machine, selecting the previously created Resource Group and allowing Azure to generate a new Virtual Network (VNet) and Subnet. I used the password authentication option for the administrator account.
</p>
<p>
  <img src="https://i.imgur.com/PHOwjLh.png" height="75%" width="100%" alt="Windows VM"/>
</p>
<p>
 Next, I created a Linux (Ubuntu) virtual machine, again using the same Resource Group and allowing Azure to manage the networking components. Password authentication was selected here as well.
</p>
<p>
  <img src="https://i.imgur.com/N5zwQUH.png" height="75%" width="100%" alt="Ubuntu VM"/>
</p>
<p>
  Observe Your Virtual Network within Network Watcher:
</p>
<p>
  <img src="https://i.imgur.com/Pn02GXF.png" height="75%" width="100%" alt="Network Watcher"/>
</p>
<br />
<br />
<h3 align="center">
  Monitor ICMP Traffic with Wireshark
</h3>
<br />
<p>
 I connected to the Windows VM via Remote Desktop, installed Wireshark, and applied a filter to display ICMP traffic only.
</p>
<p>
  <img src="https://i.imgur.com/0BsfNiS.jpg" height="75%" width="100%" alt="Microsoft Remote Desktop - Mac"/>
</p>
<p>
 From the Windows machine, I located the private IP address of the Ubuntu VM and sent ping requests to it. I was able to observe both requests and replies in Wireshark, confirming successful communication over the private network.
</p>
<p>
  <img src="https://i.imgur.com/yYGKuAy.png" height="75%" width="100%" alt="Ubuntu private IP"/>
  <img src="https://i.imgur.com/3h9QSEY.png" height="75%" width="100%" alt="ICMP traffic - private IP"/>
</p>
<p>
 I also pinged a public site like www.google.com, and Wireshark displayed ICMP packets showing traffic going out to the internet and returning responses.
</p>
<p>
  <img src="https://i.imgur.com/YduMvc7.png" height="75%" width="100%" alt="ICMP traffic - public IP"/>
</p>
<p>
 To simulate continuous monitoring, I initiated a perpetual ping to the Ubuntu VM from the Windows command line and watched the constant flow of ICMP packets in Wireshark.
</p>
<p>
  <img src="https://i.imgur.com/bihftKK.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
</p>
<p>
 I then navigated to the Ubuntu VM’s Network Security Group in the Azure portal and disabled inbound ICMP traffic by adding a new rule. Back on the Windows VM, I observed that ping replies stopped appearing, and Wireshark confirmed the lack of incoming traffic.
</p>
<p>
  <img src="https://i.imgur.com/ovGk5dq.png" height="75%" width="100%" alt="ICMP traffic - perpetual ping"/>
  <img src="https://i.imgur.com/NjuUANI.png" height="75%" width="100%" alt="ICMP traffic - ICMP denied"/>
</p>
<p>
  After that, I removed the ICMP block rule in the NSG, and the ping started responding again, which was visible both on the command line and in Wireshark.
</p>
<p>
  <img src="https://i.imgur.com/nZbl2sA.png" height="75%" width="100%" alt="ICMP traffic - ICMP re-enabled"/>
</p>
<br />
<br />
<h3 align="center">
Observe SSH Traffic</h3>
<br />
<p>
 In Wireshark, I changed the filter to display SSH traffic only. From the Windows VM, I SSH'd into the Ubuntu machine using its private IP. As I typed commands like ls and pwd, I saw SSH packet activity appear in Wireshark. I ended the SSH session by typing exit, closing the secure shell connection.
</p>
  <img src="https://i.imgur.com/6YEDJKu.png" height="75%" width="100%" alt="SSH traffic"/>
<p>
<br />
<br />
<h3 align="center">
Analyze DHCP Traffic</h3>
<br />
<p>
 I updated the Wireshark filter to capture DHCP traffic.
 On the Windows VM, I opened the command prompt and typed ipconfig /renew to request a new IP address from the DHCP server.
 In Wireshark, I observed the DHCP request and response process, showing the renewal of the machine’s IP address.
</p>
<p>
  <img src="https://i.imgur.com/mKyAHFr.png" height="75%" width="100%" alt="DHCP traffic"/>
</p>
<br />
<br />
<h3 align="center">
  Inspect DNS Traffic
</h3>
<br />
<p>
  With Wireshark now set to display DNS packets, I used the nslookup command in the Windows command line to query the IP addresses of google.com and disney.com.
Each query generated visible DNS traffic in Wireshark, showing resolution requests and the resulting IP addresses.
</p>
<p>
  <img src="https://i.imgur.com/mYZ8CAK.png" height="75%" width="100%" alt="DNS traffic"/>
</p>
<br />
<br />
<h3 align="center">
Review RDP Traffic
</h3>
<br />
<p>
  Lastly, I filtered Wireshark to show RDP traffic only using the expression tcp.port==3389.
  I noticed a constant stream of RDP packets, which is expected because Remote Desktop Protocol continuously sends data to maintain a live visual connection between the 
  host and client.
</p>
<p>
  <img src="https://i.imgur.com/hNlhTVp.png" height="75%" width="100%" alt="RDP traffic"/>
</p>
<p>
 Clean up Azure Environment
Once all observations were complete, I closed the Remote Desktop session, returned to the Azure portal, and deleted the Resource Group to ensure no lingering resources would incur extra charges.
</p>
