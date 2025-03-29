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
- Windows 10 

<h2>High-Level Deployment and Configuration Objectives</h2>

- Create Resources
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (myadproject.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>Step 1: Setup Resources in Azure<p/>
  
  - Create a Resource Group, Virtual Network, and the Domain Controller VM (Windows Server 2022) named “DC-1”.

![create rg](https://github.com/user-attachments/assets/7f785de3-545f-4d82-8869-a412db4ba630)

  - Create Client VM (Windows 10) named “Client-1” in the same Resource Group and Vnet as DC-1.

![create vm](https://github.com/user-attachments/assets/c99d563d-cbcb-4bdf-8ae1-d399ab1e36f0)

- Set DC-1's NIC Private IP address to static to ensure consistent network connectivity.

![ipconfig to static](https://github.com/user-attachments/assets/d57edc04-33e1-474b-af33-90df25440727)

<p>Step 2: Ensure Connectivity between Client and Domain Controller</p>

  - Login to Client-1 using Remote Desktop and run a continuous ping to DC-1’s private IP address (ping -t &lt;ip_address&gt;).

![Ping 1](https://github.com/user-attachments/assets/fa6cfb3a-0093-47ef-b5ba-ccf5b36f3ddb)

  - Login to DC-1 and enable ICMPv4 (ping) in the local Windows Firewall settings.

![enable icmpv4](https://github.com/user-attachments/assets/1f008a84-3944-4c26-a5a1-d787dcf9f601)

  - Check Client-1 to confirm that the ping now succeeds, ensuring connectivity between the Client and Domain Controller

![ping connectivity 1](https://github.com/user-attachments/assets/465d300d-cda4-490a-b56a-6a7b630177a2)

<p>Step 3: Install Active Directory</p>

  - Login to DC-1 and install the Active Directory Domain Services (AD DS) role.

![install AD domain service](https://github.com/user-attachments/assets/544d3270-47d5-4515-967f-8a08559f009a)
![ad domain service 2](https://github.com/user-attachments/assets/11c5c796-a102-45e4-b1dc-7e3a2a71edb6)

  - Promote DC-1 to a Domain Controller and set up a new forest with a domain name 
  - Restart DC-1 after promotion and log in using the domain account: myadproject.com\labuser

<p>Step 4: Create Admin User and Organizational Units (OUs) in Active Directory<p/>

  - In Active Directory Users and Computers (ADUC), create 3 Organizational Units (OU)
  - Create 1st OU named <strong>_EMPLOYEES</strong> for employee users.
  - Create 2nd OU named <strong>_ADMINS</strong> for administrative users.
  - Create 3rd OU named <strong>_CLIENTS</strong> for client.

![create org unit 1](https://github.com/user-attachments/assets/e6ec369e-9bed-41b7-a8e1-0e3f02abae60)

  - Create a new user named “Jane Doe” with the username jane_admin and add her to Domain Admins security group.

![new user + add to domain admin](https://github.com/user-attachments/assets/05cc4fad-97a6-43ac-a360-98c9b061f700)

  - Log out and back in to DC-1 as myadproject.com\jane_admin to use this admin account moving forward.

<p>Step 5: Join Client-1 to the Domain<p/>

  - In the Azure Portal, set Client-1’s DNS settings to DC-1's Private IP address, then Restart

![join client to domain 1](https://github.com/user-attachments/assets/3d0b327d-b3b3-4a7f-85ea-38116d3c5727)

  - Login to Client-1 using the local admin account labuser, and join it to the domain myadproject.com
  - Allow “domain users” access to remote desktop
  - Once joined, restart Client-1 and verify that it appears in ADUC under the Computers container.
  - Create an OU called _CLIENTS and move Client-1 into this OU for organizational purposes.

![join client to domain](https://github.com/user-attachments/assets/4b997517-d69a-4982-b2ce-356bed71aa5c)

<p>Step 6: Create additional user accounts using a Powershell ISE script and verify access.</p>

  - Log into DC-1 as jane_admin and open PowerShell ISE as an administrator.
  - Use PowerShell to create multiple user accounts in AD, following a scripted process.

![scipt to create mulitples](https://github.com/user-attachments/assets/db0ac674-7052-46b7-a2ab-7358542028ec)

  - Once the script completes, check ADUC to ensure the new users are created in the appropriate OU.

![script to create muliptles 2](https://github.com/user-attachments/assets/8524c253-75b5-4426-9281-ba028b7651a6)

  - Attempt to log into Client-1 with one of the newly created users to verify the account works correctly

![newly created random user](https://github.com/user-attachments/assets/0a8429e0-9313-4cbe-8bd0-d36065e4fb86)


