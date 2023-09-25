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

- Inspect DNS rescords on the server
- Create A-records on the server and observe them from the client
- Delete records from the server and clear the client DNS cache
- Observe C-NAME records

<h2>Actions and Observations</h2>

<p>1. Using DC-1 and Client-1 from the previous lab, login into DC-1 as domain admin. Also, connect to Client-1 as an admin. From Client-1 try to ping "mainframe". To do this, on Client-1, open Command Prompt, and type "ping mainframe". You will notice that this fails. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/9816eb31-e700-486d-93e7-de6e47dc9de2"/><br /><br />

<p>2. On DC-1, click Start and open Server Manager. In Server Manager, go to Tools, and open DNS. Expand the DC-1 option. In Forward Lookup Zone, open mydomin.com and notice the record for Client-1 and DC-1. Right-click and select New Host. Call this "mainframe" and use the same IP address as DC-1. Finally, click to Add Host and press OK. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/30416bd0-dc59-4609-9269-7c9c141498f6"/><br /><br />

<p>3. Next, connect to Client-1, open a Command Prompt, and type "ping mainframe". You will see that the ping is now successful becuase you created the A-record in DC-1.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/f579a017-21a7-4ff6-9d92-eba70016e571"/><br /><br />

