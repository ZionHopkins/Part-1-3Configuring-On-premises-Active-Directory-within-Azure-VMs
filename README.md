# Configuring-On-premises-Active-Directory-within-Azure-VMs

Setup Resources in Azure

Create the Domain Controller VM (Windows Server 2022) named “DC-1”

Take note of the Resource Group and Virtual Network (Vnet) that get created at this time

Set Domain Controller’s NIC Private IP address to be static
<p>
<img src="https://i.imgur.com/rMZIb2e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/YZeGtv1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Rg1NY6j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in
  
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher

Ensure Connectivity between the client and Domain Controller
  
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
  
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall

<p>
<img src="https://i.imgur.com/aX8tYw4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Check back at Client-1 to see the ping succeed

Install Active Directory
  
Login to DC-1 and install Active Directory Domain Services
<p>
<img src="https://i.imgur.com/NaVLgh4.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/Xkg1ggr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
  
Restart and then log back into DC-1 as user: mydomain.com\labuser
<p>
<img src="https://i.imgur.com/BZd30v0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
  Create an Admin and Normal User Account in AD
  
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
  
Create a new OU named “_ADMINS”
  
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
  
Add jane_admin to the “Domain Admins” Security Group
  
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
  
User jane_admin as your admin account from now on

Join Client-1 to your domain (mydomain.com)
  
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
<p>
<img src="https://i.imgur.com/wSS22E6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

From the Azure Portal, restart Client-1
  
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
  
Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” 
container on the root of the domain
  
Create a new OU named “_CLIENTS” and drag Client-1 into there

Setup Remote Desktop for non-administrative users on Client-1
  
Log into Client-1 as mydomain.com\jane_admin and open system properties
  
Click “Remote Desktop”
  
Allow “domain users” access to remote desktop
  
You can now log into Client-1 as a normal, non-administrative user now
  
Normally you’d want to do this with Group Policy that allows you to change MANY systems at once (maybe a future lab)

Create a bunch of additional users and attempt to log into client-1 with one of the users
  
Login to DC-1 as jane_admin
  
Open PowerShell_ise as an administrator
  
Create a new File and paste the contents of the script(https://github.com/ZionHopkins/Employee-Creation-Script) into it
  
Run the script and observe the accounts being created
  
When finished, open ADUC and observe the accounts in the appropriate OU
  
attempt to log into Client-1 with one of the accounts (take note of the password in the script)
