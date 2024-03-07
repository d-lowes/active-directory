<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the step-by-step process of deploying an on-premises Active Directory (AD) within Azure Virtual Machines (VMs), allowing for the management of domain services in a cloud environment. By following this guide, you'll learn how to set up a domain controller, create user accounts, configure DNS settings, and establish file sharing with various permissions.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure: Create the Domain Controller (DC) and Client VMs within a single Vnet to ensure connectivity.
- Ensure Connectivity: Verify network communication between VMs.
- Install Active Directory: Install and configure Active Directory Domain Services on the DC VM.
- User and Group Management: Create user accounts and organizational units (OUs).
- Domain Joining: Join the Client VM to the domain.
- Setup Remote Desktop Access: Configure remote desktop settings for non-administrative users.

<h2>Deployment and Configuration Steps</h2>

### Setup Resources in Azure

**1.** Navigate to the Azure portal and create a new virtual machine named `DC-1` with Windows Server 2022. Note the Resource Group and Virtual Network (Vnet) automatically created during this process. Set the NIC's private IP address of `DC-1` to static for stable network configuration.

**2.** Within the same Resource Group and Vnet, create another VM named `Client-1` using Windows 10. Ensure both VMs are within the same Vnet for seamless network connectivity.

### Ensure Connectivity

**1.** Log in to `Client-1` using Remote Desktop and initiate a perpetual ping to `DC-1`'s private IP address using `ping -t <ip address>`.

**2.** On `DC-1`, enable ICMPv4 in the Windows Firewall to allow ping requests.

**3.** Observe successful ping responses on `Client-1`.

### Install Active Directory

**1.** Log into `DC-1` and install Active Directory Domain Services via Server Manager.

**2.** Promote the server to a domain controller by setting up a new forest named `mydomain.com`.

**3.** Restart `DC-1` and log back in as `mydomain.com\labuser`.

### Create User Accounts in AD

**1.** On `DC-1`, use Active Directory Users and Computers (ADUC) to create an OU named `_EMPLOYEES`.

**2.** Create another OU within `_EMPLOYEES` named `_ADMINS`.

**3.** Create a user account named `Jane Doe` with the username `jane_admin` and add it to the `Domain Admins` group.

**4.** Use `jane_admin` as your admin account moving forward.

### Join Client-1 to the Domain

**1.** In the Azure portal, update `Client-1`'s DNS settings to point to `DC-1`'s private IP address.

**2.** Restart `Client-1` and log in using Remote Desktop as the local administrator.

**3.** Join `Client-1` to `mydomain.com`. The computer will restart post-joining.

**4.** Verify that `Client-1` appears in ADUC on `DC-1` under the "Computers" container.

### Setup Remote Desktop for Non-Administrative Users

**1.** Log into `Client-1` as `mydomain.com\jane_admin` and enable remote desktop access for "domain users" through system properties.

### Bulk User Creation

**1.** On `DC-1`, as `jane_admin`, run the PowerShell script from [Generate-Names-Create-Users.ps1](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) to create additional user accounts.

**2.** Verify the new accounts in the appropriate OU in ADUC.


## IMPORTANT
When you are finsished with the lab, delete your Azure resources. This is crucial, as you will be charged credits for having resource groups. Best practice is to delete all resource groups and double check that they are deleted properly.


## Conclusion

Congratulations! You have successfully set up an Active Directory environment using Azure Virtual Machines, including a domain controller and client machine configuration. You've also performed basic AD management tasks, such as user creation and domain joining. This foundation enables further exploration of AD features and services in a cloud environment.
