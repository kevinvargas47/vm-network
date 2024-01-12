<p align="center">
<img src="https://github.com/ColtonTrauCC/vm-network/assets/147654000/2cb238ff-4e46-4a75-8967-7ef5d124ab74" height="15%" width="15%" alt="Microsoft Azure logo"/>
</p>

<h1 align = "center">Virtual Machine</h1>
This tutorial outlines how to set up a virtual machine network in Microsoft Azure and doing some exercises observing traffic.

</p>

<h3>Environments and Technologies </h3>

<li>Microsoft Azure (Virtual Machines/Compute)</li>
<li>Microsoft Remote Desktop</li>
<li>Windows Command Prompt</li>
<li>Wireshark</li>
</li>
</p>
<b>Network Protocols</b></li>:
<li>DNS:  Domain Name Systems</li>
<li>ICMP: Internet Control Message Protocol</li>
<li>SSH:  Secure Shell</li>
<li>DHCP: Dynamic Host Configuration Protocol</li>
<li>RDP:  Remote Desktop Connection</li>
<p>
</ul>

<h3>Operating Systems </h3>

<li>Windows 10 (21H2)</li>
<li>Linux (Ubuntu 20.04)</li>
</ul>


<h3>List of Prerequisites</h3>
<ol>
<li>Microsoft Azure Account and Subscription</li>
<p>
<li>Access to Microsoft Remote Desktop Connection. For MacOS users, follow <a href = "https://www.youtube.com/watch?v=0lllpAhgAJs&ab_channel=TheHostingVideos">this video</a> to use Remote Desktop on Mac</li>
<p>
<li>Notepad (Optional): to record log in information for our Virtual Machines</li>
<p>
</ol>
</p>
</ol>

<h3>Installation Steps</h3>

<h3>Creating a Resource Group and Virtual Machines</h3>

<p>

</li>
</p>
<b>Resource Group</b>: In the Azure Portal, go to resource groups to create a resource group and name it RG-Lab-01. Take note of the region of the resource group as it'll come in use when setting up the virtual machines. When finished, click on Review + Create.</li>
<p>
<img src="https://i.imgur.com/VhKtvlv.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<b>Virtual Machine</b>: Access Azure Portal, go to Virtual Machines to create an azure virtual machine. Select the resource group we've created (RG-Lab-01) and name it VM-1. Make sure the Region is the same as the resource group previously created. To set the Availability Options, set it to No Infrastructure and Security Type to Standard.</i>
</p>
<b>Image</b> (Operating System): set it to Windows 10 Pro, Version 22H2, x64 Gen2</li>
</p>
<b>Size</b>: the general processing power and RAM of the VM, set it to Standard_E2s_V3 which provides 2 virtual cpus and 16 GBs of RAM</li>
<p>
<img src="https://i.imgur.com/RQit8af.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<p>
Set the username and password of your VM for logging in and make sure to check the box for licensing agreements
<p>
Go to the Network tab, notice the Virtual Network created by the Virtual Machine by the Resource Group. It will be made automatically by the Virtual Machine
<p>
<img src="https://i.imgur.com/oa1kwem.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
Click on Review + Create to deploy the virtual machine. Wait approximately five to ten minutes to fully deploy before continuing
</p>
<b>Virtual Machine 2</b>: same process as Virtual Machine 1 except name it VM-2 and set the image to Ubuntu Server 20.04 LTS x64 Gen2. Ubuntu by default has their Administrator account authentication as SSH public key, so we must set it as Password for logging in through Remote Desktop
</p>
<img src="https://i.imgur.com/mfJoqUy.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
</ul>
</p>

<br />

<h3>Logging into a Virtual Machine using Remote Desktop Connection</h3>

<p>

<p>
In Azure Portal go to VM-1, go to Overview, go to Public IP Address section and copy it. Open Remote Desktop Connection and paste it there, then click Connect. Log in using the username and password you set up for VM1 (a pop up may show for verification, click "Yes" if it does)
<p>
<img src="https://i.imgur.com/5cadoWL.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
You have successfully logged into the Virtual Machine
<p>
<img src="https://i.imgur.com/8liNZe7.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
</p>

<br />

<h3>Observing Traffic in Virtual Machines</h3>
</p>
</p>

<br />
<p>
Open a web browser (Microsoft Edge) in the virtual machine. Search for and install <a href="https://www.wireshark.org/download.html">Wireshark</a>. Download Windows x64 Installer.
<p>
</p>
<img src="https://i.imgur.com/tclz6eG.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
</p>

<h3>Observing ICMP Traffic</h3>
<p>
</p>
Open Wireshark, run as administrator and start capturing packets (blue fin icon). This lets you see the actual live traffic on the virtual machine. In the filter bar type icmp to filter incoming ICMP packets. ICMP is used for reporting errors and performing network diagnostics.
<p>
<img src="https://i.imgur.com/uLZKzCC.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
</p>
Go to the Azure Portal on the physical desktop, go to VM-2 and note its Private IP Address
<p>
<img src="https://i.imgur.com/KD0EE3P.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Open Windows Powershell in VM-1 and in the command line enter ping [VM2 Private IP]. Then ICMP packets should display in Wireshark.
<p>
<img src="https://i.imgur.com/VEGwlql.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
On Windows Powershell enter ping [VM-2 Private IP], then -t. This starts an effect of non stop ping between the Virtual Machines, resulting in nonstop ICMP packets displaying in Wireshark
</p>
<img src="https://i.imgur.com/j0P3UtO.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
Access the Azure Portal, go to VM-2 Network Security Group (NSG). It should be named VM-2-nsg, in order to stop the traffic
</p>
Go to inbound security rules and create a security rule that denies ICMPs. Click on Add to open a right side pop up to set the rule
<p>
Enter this information
<p>
<img src="https://i.imgur.com/Hxi5n87.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
Set the priority to 200. Name the rule deny_icmp_ping_from_anywhere, then click Add to finish
</p>
<b>Note</b>: priorities are inversely proportional meaning lower numbers have higher priority
</p>
When completed, Request timed out will display in Powershell in VM1, this means ICMP ping has halted from the security rule that was added
<p>
<img src="https://i.imgur.com/0dbd0E6.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
To restart the traffic, go to the Azure Portal and set the inbound security rule's action to Allow and save
<p>
</p>

<br />

<h3>Observing SSH Traffic</h3>

<p>
Go to Windows Powershell inside VM-1, type in ssh labuser@[VM2's Private IP], enter. Enter "yes" and it will ask for the password of VM-2
<p>
<b>Note</b>: we are accesssing the terminal of VM-2 (Linux's version of a command prompt) it doesen't display input and dots when typing a password. It is registering input when typing
<p>
Typing in commands such as uname -a, id, pwd, or sudo apt will display traffic on Wireshark. You can filter ssh traffic in Wireshark by typing in ssh in the filter bar, and observe the results
<p>
<img src="https://i.imgur.com/YMgANPk.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<p>
<img src="https://i.imgur.com/Kw9h3bt.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
When finished, you can enter the command exit to end session
<p>
</p>

<br />

<h3>Observing DHCP Traffic</h3>

<p>
Filter dhcp traffic in Wireshark by entering dhcp in the filter bar. DHCP assign IP Addresses to devices new to the network the moment the device joins the network. We can reassign an IP Address in the virtual machine by going to Powershell and entering the command ipconfig /renew
<p>
<img src="https://i.imgur.com/o3QRVhK.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
<img src="https://i.imgur.com/uh5eDFi.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>

<br/>

<h3>Observing DNS Traffic</h3>

<p>
Filter DNS traffic in Wireshark by entering dns in the filter bar. In Powershell type in nslookup www.google.com
<p>
<img src="https://i.imgur.com/cx0T23Z.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
<p>
<img src="https://i.imgur.com/CCO0tht.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>

<br/>

<h3>Observing RDP Traffic</h3>

<p>
Filter RDP traffic in Wireshark by entering tcp.port==3389 in the filter bar and you'll notice non-stop traffic. The RDP is constantly showing you a live stream from one computer to another, therefore traffic is always being transmitted
<p>
<img src="https://i.imgur.com/dgIWhy8.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>

<br/>

<h3>Clean Up</h3>
<p>
Open the windows command prompt, enter logoff to end session. It is best practice to delete all of the resources in Azure Portal after finished with the lab to not accumulate a billing cost
</p>
<img src="https://i.imgur.com/Dp0zcVo.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
This is the conclusion of this lab
