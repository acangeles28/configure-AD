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

<h2>Step 2: Ensure Connectivity between Client and Domain Controller</h2>

  - Login to Client-1 using Remote Desktop and run a continuous ping to DC-1’s private IP address (<code>ping -t &lt;ip_address&gt;</code>).

![Ping 1](https://github.com/user-attachments/assets/fa6cfb3a-0093-47ef-b5ba-ccf5b36f3ddb)

  - Login to DC-1 and enable ICMPv4 (ping) in the local Windows Firewall settings.

![enable icmpv4](https://github.com/user-attachments/assets/1f008a84-3944-4c26-a5a1-d787dcf9f601)

  - Check Client-1 to confirm that the ping now succeeds, ensuring connectivity between the Client and Domain Controller

![ping connectivity 1](https://github.com/user-attachments/assets/465d300d-cda4-490a-b56a-6a7b630177a2)

<h2>Step 3: Install Active Directory</h2>
<ul>
  <li>Login to DC-1 and install the <strong>Active Directory Domain Services</strong> (AD DS) role.</li>
  <li>Promote DC-1 to a Domain Controller and set up a new forest with a domain name (e.g., <code>mydomain.com</code>).</li>
  <li>Restart DC-1 after promotion and log in using the domain account: <code>mydomain.com\labuser</code>.</li>
</ul>

<h2>Step 4: Create Admin User and Organizational Units (OUs) in Active Directory</h2>
<ul>
  <li>In Active Directory Users and Computers (ADUC), create 3 Organizational Units (OU)</li>
   <li>Create 1st OU named <strong>_EMPLOYEES</strong> for employee users.</li>
   <li>Create 2nd OU named <strong>_ADMINS</strong> for administrative users.</li>
   <li>Create 3rd OU named <strong>_CLIENTS</strong> for client.</li>
  <li>Create a new user named “Jane Doe” with the username <code>jane_admin</code> and add her to the <strong>Domain Admins</strong> security group.</li>
  <li>Log out and back in to DC-1 as <code>mydomain.com\jane_admin</code> to use this admin account moving forward.</li>
</ul>

<h2>Step 5: Join Client-1 to the Domain</h2>
<ul>
  <li>In the Azure Portal, set Client-1’s DNS settings to DC-1's Private IP address.</li>
  <li>Restart Client-1 from the Azure Portal.</li>
  <li>Login to Client-1 using the local admin account <code>labuser</code>, and join it to the domain <code>mydomain.com</code>.</li>
  <li>Allow “domain users” access to remote desktop</li>
  <li>Once joined, restart Client-1 and verify that it appears in ADUC under the <strong>Computers</strong> container.</li>
  <li>Create an OU called <strong>_CLIENTS</strong> and move Client-1 into this OU for organizational purposes.</li>
</ul>

<h2>Step 6: Create additional user accounts using a Powershell ISE script and verify access.</h2>
<ul>
  <li>Log into DC-1 as <code>jane_admin</code> and open PowerShell ISE as an administrator.</li>
  <li>Use PowerShell to create multiple user accounts in AD, following a scripted process.</li>
  <li>Once the script completes, check ADUC to ensure the new users are created in the appropriate OU.</li>
  <li>Attempt to log into Client-1 with one of the newly created users to verify the account works correctly.</li>
</ul>

<h2>Conclusion</h2>
<p>In this tutorial, we successfully deployed and configured an operational Active Directory environment. This involved setting up a Domain Controller and a client machine in Azure, establishing communication between them, and installing Active Directory Domain Services. After creating a new domain, we organized it by adding organizational units and user accounts, including an administrator account, and connected the client to the domain.</p>

<p>We also enabled Remote Desktop access for domain users and used a PowerShell script to create additional user accounts. To complete the lab, we tested logging into the client using one of the newly created accounts. This lab provided valuable hands-on experience with key Active Directory setup and configuration tasks.</p>

