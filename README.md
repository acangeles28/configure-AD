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

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<h2>Step 1: Setup Resources in Azure</h2>
<ul>
  <li>Create the Domain Controller VM (Windows Server 2022) named “DC-1”.</li>
  <li>Take note of the Resource Group and Virtual Network (Vnet) created for the Domain Controller.</li>
  <li>Set DC-1's NIC Private IP address to static to ensure consistent network connectivity.</li>
  <li>Create the Client VM (Windows 10) named “Client-1” in the same Resource Group and Vnet as DC-1.</li>
  <li>Ensure both VMs (DC-1 and Client-1) are in the same Vnet. You can verify this with Azure's Network Watcher topology tool.</li>
</ul>

<h2>Step 2: Ensure Connectivity between Client and Domain Controller</h2>
<ul>
  <li>Login to Client-1 using Remote Desktop and run a continuous ping to DC-1’s private IP address (<code>ping -t &lt;ip_address&gt;</code>).</li>
  <li>Login to DC-1 and enable ICMPv4 (ping) in the local Windows Firewall settings.</li>
  <li>Check Client-1 to confirm that the ping now succeeds, ensuring connectivity between the Client and Domain Controller.</li>
</ul>

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

