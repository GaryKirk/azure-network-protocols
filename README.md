<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups and Inspecting Traffic Between Azure Machine</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Inspect DNS rescords on the server
- Create A-records on the server and observe them from the client
- Delete records from the server and clear the client DNS cache
- Observe C-NAME records
- Explore Network File Sharing and Permissions

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

<p>9. To continue, make sure to connect to DC-1 as an admin. Log into Client-1 as an individual user. On DC-1, on the C-drive, create four folders: "read-access", "write-access", "no-access", and "accounting".   <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/aa8d9448-63c3-44bf-b5da-c83d19aa005c" alt="folders" width="500" length="500"/><br /><br />

<p>10. next, change the permissions for each new folder. To do this, right-click on the folder name, select Properties, Sharing, Share, and set the share settings to match the following: <br />
<ul>
  <li>Folder: read-access, Group: Domain Users, Permissions: Read</li>
  <li>Folder: write access, Group: Domain Users, Permissions: Read/Write</li>
  <li>Folder: no-access, Group: Domain Users, Permissions: Read/Write</li>
</ul>
</p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/a21f4d1b-4fb9-4c28-92bb-59d104bec98d" alt="permissions" width="500" length="500"><br /><br />

<p>11. In Client-1, open File Explorer. In the search bar, type "\\dc-1".  Double-click to open the no-access folder and notice that you're aren't able to open it. Next, open read-access. Right-click and click to create a new folder. You should notice that you're unable to create a new folder. This is because, as an individual user, you're only able to read what is in this folder. You're unable to create anything new. Open the write-access folder and complete the same steps. You will be able to create new files and folders in this folder because of the permissions settings of write.<br /></p>-
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/514a6c08-f6cc-44f1-b089-14e7b60c7fe4" alt="no access" width="500" length="500"/><br /><br />

<p>12. In DC-1, open Active Directory USer and Computers. Under mydomain.com, right-click and create a new Organizational Unit called "_SECURITY_GROUPS". Refresh Active Directory and you will see the new Organizational Unit. Open _SECURITY_GROUPS, right-click, and create a new group called "ACCOUNTANTS". Make sure this is a 'Security' Group.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/db59ba9c-b0f0-4d46-a864-4661c3b84619" alt="accountants" width="500" length="500"/><br /><br />

<p>12. On DC-1 C-Drive, right-click the accountants' folder. Click Permissions, Sharing, Share. Search for the 'ACCOUNTANTS" security group and give that group read and write access to this folder.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/e38cd54f-482a-4bf8-a1ba-c1d5c2fb2b19" alt="accountants permissions" width="500" length="500"/><br/><br/>

<p>13. In Client-1, on the C-drive, search for "\\dc-1". You will notice that the 'accounting' folder is available. Double-click to open it. You should receive a message that you don't hav permission to access the folder. This is because the Client-1 user is not part of the 'ACCOUNTANTS' security group.<br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/48b18ba5-6712-4c46-a450-435bca87fb12" alt="no access" width="500" length="500"/><br/><br/>

<p>13. On DC-1, in Active Directory Users and Computers, go to the 'ACCOUNTANTS' security group. Double-click to open 'ACCOUNTANTS'. Go to the 'Members' tab and add the Client-1 user as a member of the the group. Click to apply the changes. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/77ba4194-9917-4f1c-940e-bc194f980324" alt="update access" width="500" length="500"/><br/><br/>

<p>13. On Client-1, log out of the current user. Use the credentials to log back in to a session. The reason you have to do this is because a users permissions are updated each time they log on to the network. Navigate to the 'accounting' folder on the C-drive and this user will be able to access the folder. <br /></p>
<img src="https://github.com/GaryKirk/azure-network-protocols/assets/137613637/86af8380-95c2-4bf7-85de-07a0b2332205" alt="able to access" width="500" length="500"/><br/><br/>

<p>By following this tutorial, you should know have a better understanding of DNS and Network Security Groups. <br /></p>
