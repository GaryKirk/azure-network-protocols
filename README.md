<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Inspecting Traffic Between Azure Virtual Machines</h1>
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
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/9816eb31-e700-486d-93e7-de6e47dc9de2" alt="ping" width="500" length="500"/><br /><br />

<p>2. On DC-1, click Start and open Server Manager. In Server Manager, go to Tools, and open DNS. Expand the DC-1 option. In Forward Lookup Zone, open mydomin.com and notice the record for Client-1 and DC-1. Right-click and select New Host. Call this "mainframe" and use the same IP address as DC-1. Finally, click to Add Host and press OK. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/30416bd0-dc59-4609-9269-7c9c141498f6" alt="Add Host" width="500" length="500"/><br /><br />

<p>3. Next, connect to Client-1, open a Command Prompt, and type "ping mainframe". You will see that the ping is now successful because you created the A-record in DC-1.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/f579a017-21a7-4ff6-9d92-eba70016e571" alt="ping check" width="500" length="500"/><br /><br />

<p>4. In the same Command Prompt, now type "ipconfig /displaydns". Scroll through the records to find the 'mainframe' record. You will notice that 'dc-1.mainframe.com' is now saved in the local cache. This means that the next time you visit 'mainframe', your system will use this record rather than going to the DNS server. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/da25ad84-84da-4550-abc4-709cb6980e99" alt="displaydns" width="500" length="500"/><br /><br />

<p>5. In DC-1, go back to the 'mainframe' record. Click to open the A-record. In the IP address line, change the number to "8.8.8.8". On Client-1, in a Command Prompt, type "ping mainframe". You will notice that the ping is successful even though the IP address has been changed. The reason for this is because the DNS record on Client-1 has not been updated to reflect the new IP address.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/903c94de-b282-4bfa-8b1e-eaa08af65cc3" alt="ping check" width="500" length="500"/><br /><br />

<p>6. On Client-1, open a Command Prompt as an Administrator. Type "ipconfig /flushdns". After this, type "ipconfig /displaydns". You will notice that the record is now empty. Now type "ping mainframe". The ping should be successful show the connection to 8.8.8.8. Type "ipconfig /displaydns". You will notice that the records now is updated to mainframe at IP address 8.8.8.8. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/56f6dbc8-e0fd-45e9-abeb-587a4a3893d8" alt="flushdns2" width="500" length="500"/><br /><br />

<p>7. In a Client-1 Commpand Prompt, type "ping search". You will notice that there is no record of search. In Dc-1, go to Server Manager, under 'mainframe', right-click and click to creata a 'New Alias (CNAME)'. Name this "search: and under 'Fully qualified domain name', type "www.google.com". Click to create this record.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/a4cfce6b-0ba5-4f8f-a772-ec4e423ac7e3" alt="Alias" width="500" length="500"/><br /><br />

<p>8. In Client-1, open a web browser and type "search.mydomain.com". You will notice that the browser will try to load Google but the connection is unsuccessful. The reason for this is becuase Google's SSL certificate doesn't recognize search.mydomain.com as a valid match. Open Command Prompt and type "ipconfig /flushdns". After this, type "ping search" and then "ipconfig /displaydns". You will notice that search connects to Google, which then connects to a correct Google IP address.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/5df5c37d-44f9-4e15-812a-530dea322a0d" alt="search" width="500" length="500"/><br /><br />


