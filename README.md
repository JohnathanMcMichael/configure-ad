<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines how I implemented on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: Setup Resources in Azure
- Step 2: Ensure Connectivity between the client and Domain Controller
- Step 3: Install Active Directory
- Step 4: Create an Admin and Normal User Account in AD
- Step 5: Join Client-1 to your domain (mydomain.com)
- Step 6: Setup Remote Desktop for non-administrative users on Client-1
- Step 7: Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/Hy5RuOb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/></p>
<p>
<img src="https://i.imgur.com/5ODzEXV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/iBKVO3a.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”. 
Set Domain Controller’s NIC Private IP address to be static. 
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created previously. 
Ensure that both VMs are in the same Vnet. 
</p>
<br />

<p>
<img src="https://i.imgur.com/ata8n7Y.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/KGpMZ4N.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping). 
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall. 
Check back at Client-1 to see the ping succeed. 
</p>
<br />

<p>
<img src="https://i.imgur.com/c823e2i.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/bKYpkVh.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 and install Active Directory Domain Services. 
Promote as a DC: Setup a new forest as mydomain.com. 
Restart and then log back into DC-1 as user: mydomain.com\labuser. 
</p>
<br />

<p>
<img src="https://i.imgur.com/3Y18IMy.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”. 
Create a new OU named “_ADMINS”. 
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”. 
Add jane_admin to the “Domain Admins” Security Group. 
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”. 
Use jane_admin as your admin account from now on. 
</p>
<br />

<p>
<img src="https://i.imgur.com/NaCMrUA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/vuxyJyL.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address. 
From the Azure Portal, restart Client-1. 
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain. 
Login to the Domain Controller and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain. 
</p>
<br />

<p>
<img src="https://i.imgur.com/gieLB4U.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Log into Client-1 as mydomain.com\jane_admin and open system properties. 
Click “Remote Desktop”. 
Allow “domain users” access to remote desktop. 
You can now log into Client-1 as a normal, non-administrative user now. 
</p>
<br />

<p>
<img src="https://i.imgur.com/Xfak2F0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/C7dZ0Gd.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Login to DC-1 as jane_admin. 
Open PowerShell_ise as an administrator. 
Create a new File and paste the contents of this script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1). 
Run the script and observe the accounts being created. 
When finished, open ADUC and observe the accounts in the appropriate OU. 
Attempt to log into Client-1 with one of the accounts (take note of the password in the script). 
</p>
<br />
