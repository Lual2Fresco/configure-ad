<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)


<h2>Deployment and Configuration Steps</h2>


<p>
Step 1: In Azure, create two Virtual Machines (VMs) - one running Windows Server 2022 (as the Domain Controller) and the other running Windows 10. Ensure they share the same Resource Group and Virtual Network.
</p>
<br />

<p>
<img src="https://i.imgur.com/NkEkkQi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/vl0nWGZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 2: In Azure, access the Domain Controller's Network settings, then navigate to the NIC configuration. Change the Private IP address settings from Dynamic to static by selecting "ipconfig1" under IP configurations.
</p>
<br />

<p>
<img src="https://i.imgur.com/FGtFZ3q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/Tn4kyhl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/FFugH91.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
</p>
<p>
We need to ensure both VMs can connect to each other. To test this, log in to Windows 10 (referred to as Client-1). The connection should fail.

</p>
<br />
<p>
<img src="https://imgur.com/mz9zqst.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/PSQEIPT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/NGIKMae.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 3: Upon logging into the Domain Controller's VM, Server Manager opens automatically. Access Windows Defender Firewall with Advanced Security and sort Inbound Rules by Protocol.

</p>
<br />
<p>
<img src="https://imgur.com/nM6KkAC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/xWOPw6R.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/3A1PSWo.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Enable the ICMPv4 rules shown in the image below. Try pinging the Domain Controller from Client-1 again. The ping should now be successful.

</p>
<br />
<p>
<img src="https://i.imgur.com/pczGtFb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/BSEkjj7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 4: In Server Manager, select "Add Roles and Features" to initiate the installation of Active Directory Domain Services (AD DS).

</p>
<br />
<p>
<img src="https://i.imgur.com/mo0oUGx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/r9hKuFm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once installed, ensure the flag at the top right appears as shown in the images. Proceed to promote the server to a Domain Controller. During setup, choose to set up as a new forest and name your domain. The VM should restart automatically. Upon logging back in, use the username format (domainname)\(username) or (username)@(domainname).
  
</p>
<br />
<p>
<img src="https://imgur.com/zXmUt30.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/5DykGCb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/0jVYvdZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 5: After the Domain Controller restarts, in Server Manager, go to "Tools" > "Active Directory Users and Computers". Create new Organizational Units (OUs) by right-clicking on (DomainName). In the NIC settings, change the Domain Controller's Private IP Address in DNS servers. Restart Client-1. For example, create "_EMPLOYEES" and "_ADMINS".

</p>
<br />
<p>
<img src="https://i.imgur.com/jdGLTdJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/EiJ4n2d.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/Q2IsOs0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 6: Right-click the "_Admins" OU to add a new user. Then, access Properties for the created user. Under the "Members of" tab, add the user to "Domain Admins". Logout of the Domain Controller and try logging back in with the new user.

</p>
<br />
<p>
<img src="https://imgur.com/U7oPfTl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/w3hccY0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/JbyIGrS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/XXtl8f3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 7: To add Client 1 to the Domain, return to its page in Azure. Access Client 1's Network Settings to reach the NIC. In the NIC settings, navigate to DNS servers and change the Domain Controller's Private IP Address. Restart Client-1.

</p>
<br />
<p>
<img src="https://imgur.com/FeyFzH6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/tdyVk4Q.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/EcnaOQt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 8: Log in to Client-1 with the original local admin account. Navigate to the "About" section of System Settings and click "Rename this PC (Advanced)". Press "Change" to add it to the Domain created through the Domain Controller. Double-check in Active Directory Users and Computers on the Domain Controller to ensure Client-1 is listed under "Computers".

</p>
<br />
<p>
<img src="https://imgur.com/cThLDoR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/Lom3elZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 9: Login to Client-1 as the Domain Controller Admin. Navigate to System Settings > Remote Desktop > User Accounts. Add Domain Users to grant access for any account under Domain User to Client-1.

</p>
<br />
<p>
<img src="https://imgur.com/ShRpWV9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/M52wiuD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Step 10: Launch Windows PowerShell ISE as an administrator and open a new script.

</p>
<br />
<p>
<img src="https://i.imgur.com/d33egTq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/rlYBflg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Paste the script [https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1] into PowerShell ISE and run it using the play button or F5. This script creates 10,000 users, placing them in the "_EMPLOYEES" OU. You can adjust the number of users created on line 3 and change their destination on line 43.

</p>
<br />
<p>
<img src="https://i.imgur.com/sQRjRyv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/jq0gHnz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://imgur.com/g9gvBbO.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once the code has been executed, the users should be visible in Active Directory.

</p>
<br />
<p>
<img src="https://imgur.com/KK1A86e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Now, any of those users will have access to the Client-1 VM.

</p>
<br />
<p>
<img src="https://i.imgur.com/Jo9yJsc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/> <img src="https://i.imgur.com/aN5ShPb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
