<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Video Demonstration</h2>

- ### [YouTube: Azure Virtual Machines, Wireshark, and Network Security Groups](https://youtu.be/jun-A2F2CnI)


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (ICMP, SSH, DHCP, DNS, RDP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>List Format</h2>

- Step 1:Let's begin by setting up a Resource Group and deploying two Virtual Machines (VM) in Microsoft Azure. One VM will be running Windows 10, while the other will be running Ubuntu.
- Step 2:To establish a connection with our Windows 10 Virtual Machine, we will utilize Remote Desktop. Once connected, we can proceed to install Wireshark. 
- Step 3:Launch Wireshark and apply a filter to display only ICMP traffic. Obtain the private IP address of the Ubuntu VM and initiate a ping from the Windows 10 VM to the Ubuntu VM. Monitor the ping requests and replies within Wireshark to observe the network activity.
- Step 4: Start an uninterrupted ping from your Windows 10 VM to your Ubuntu/Linux VM. Access the Network Security Group associated with your Ubuntu VM and deactivate the incoming (inbound) ICMP traffic. Return to the Windows 10 VM and analyze the ICMP traffic within Wireshark, as well as monitor the command line ping activity.
Re-enable ICMP traffic for the Network Security Group assigned to your Ubuntu VM. Return to the Windows 10 VM and analyze the ICMP traffic within Wireshark while also monitoring the command line ping activity. You should observe that the ping activity has resumed successfully. Finally, stop the ongoing ping activity.
- Step 5: Using your Windows 10 VM, establish an SSH connection to your Ubuntu Virtual Machine by using its private IP address. Once connected, observe the network traffic within Wireshark. After observing the traffic, exit the SSH session.
- Step 6:In your Windows 10 VM, try renewing the IP address by using the command line command "ipconfig /renew." While performing this action, monitor the DHCP traffic within Wireshark to observe the network activity.
- Step 7: Within the command line of your Windows 10 VM, execute the "nslookup" command to retrieve information about "www.disney.com" and observe the DNS traffic within Wireshark.
- Step 8: Observe all of the ongoing RDP traffic (tcp.port == 3389) in Wireshark
- Clean up and delete all made resources in order to not eat up your Azure credit.

<h2>Actions and Observations</h2>
<p>
<p>

<h3>Step 1: Create Resource Groups and Virtual Machines</h3>

As part of this lab's prerequisites, we need to set up a Resource Group and deploy two (2) virtual machines (VMs) in Azure. VM1 will be a Windows 10 machine, while VM2 will be a Linux machine.<br/>

<p></p>


<p></p>

<p></p>


<img src="https://i.imgur.com/jIrniNI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />

<h3>Step 2: Download Wireshark on Windows Virtual Machine (VM1) </h3>
<p></p>

Use Remote Desktop to connect to your Windows Virtual Machine using the Public IP address and Install [Wireshark](https://www.wireshark.org/download.html).
<p

<p></p>

<img src="https://i.imgur.com/Vq6wpon.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h3>Step 3: Observe ICMP Traffic</h3>
After successfully downloading and installing Wireshark on the Windows 10 VM (VM1), I launched the application and applied a filter to display only ICMP traffic. Next, using PowerShell and the private IP address of the Ubuntu VM (VM2), I initiated a ping from within the Windows 10 VM. Consequently, I observed both the ping requests and replies within Wireshark, monitoring the network activity of both virtual machines.

<br />
<p>
</p>
<p>
<h3>Step 4: </h3>
<p></p>
 
Having initiated an uninterrupted ping from our Windows 10 VM to the Ubuntu/Linux VM, we will now access the Network Security Group associated with the Ubuntu VM. From there, we will disable the incoming (inbound) ICMP traffic. As we disable the ICMP traffic, we will closely monitor both the ICMP traffic within Wireshark and the command line Ping activity.
<br/>
<img src="https://i.imgur.com/VLuPiCJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
Observe how the ping request times out after the firewall rule was put in place (*note - The ping request timed out due to the ICMP traffic being denied as the firewall rule blocked the traffic).
<br />

</p>
<p>
Returning to VM2's Network Security Group, we will modify the Inbound Security Rule that was previously set to deny, allowing incoming ICMP traffic once again. By re-enabling ICMP traffic for the Network Security Group on the Ubuntu VM, we can observe the resumption of ping requests and replies within Wireshark. Finally, we can stop the ongoing ping activity by pressing "Control" + "C".
<br />
<img src="https://i.imgur.com/Tcu7L1u.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h3>Step 5: Observe the behavior of SSH Traffic </h3>
<p></p>  
  
Next, I applied a filter in Wireshark to display only SSH (Secure Shell) traffic. Simultaneously, within the PowerShell terminal, I initiated an SSH connection to VM2. Through this SSH connection, I executed commands, generating SSH packets that were observable in Wireshark. To conclude the SSH session, I used the "exit" command.
<br />
<img src="https://i.imgur.com/gD7kvlG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<h3>Step 6: Observe DHCP Traffic </h3>
<p></p>
 
To monitor DHCP (Dynamic Host Configuration Protocol) traffic, which handles the automatic assignment of IP addresses, we will apply a DHCP filter in Wireshark. Additionally, we will utilize the "ipconfig /renew" command within VM1 to attempt obtaining a new IP address. While the private IP address may not have changed, Wireshark will reveal both the request and acknowledgment, indicating the generation of DHCP traffic.
<br />
<img src="https://i.imgur.com/1COIRiA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<p>
<h3>Step 7: Observe DNS Traffic </h3>
<p></p>
Within Wireshark, I applied a filter to display DNS (Domain Name System) traffic. I then executed the "nslookup" command, specifically for www.nike.com. This command essentially queries our DNS server to retrieve the IP address associated with the domain name "nike.com". DNS is the network protocol responsible for transforming Fully Qualified Domain Names (FQDNs) into their respective assigned IP addresses.
<br />
<p>
<h3>Step 8: Observe RDP Traffic </h3>
<p></p>
Lastly, I applied a filter in Wireshark to display RDP (Remote Desktop Protocol) traffic, specifically by utilizing the TCP port number (tcp.port==3389). RDP is the protocol that enables remote connections to other computers, granting full control over the Graphical User Interface (GUI). Throughout the observation, a consistent generation of RDP traffic was observed.
<br />
</p>
Thank you for checking out this tutorial, I hope it helped you understand traffic between computers more!


**REMEMBER TO DELETE YOUR RESOURCES AS TO NOT EAT UP YOUR CREDIT  **
