<div align="center">

**![68747470733a2f2f692e696d6775722e636f6d2f705535413538532e706e67](https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/935f9cdd-c512-4d0a-905f-11fbd9b5be2b)** 
# Introduction to Active Directory VM Lab  </div>

Active Directory (AD) is a critical component of modern IT infrastructure, enabling centralized management of users, computers, and other resources in a networked environment. For IT professionals and enthusiasts, setting up an AD lab at home provides a hands-on learning experience, helping to develop skills essential for managing enterprise environments.
# Overview of the Lab Setup
![images](https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/d8524630-0266-4727-abe1-954368d778b4)

In this project, you will set up a virtualized environment that includes:

**Windows Server Virtual Machine:** This VM will serve as your domain controller, hosting AD DS (Active Directory Domain Services).

**Windows Client Virtual Machines:** These VMs will join the domain and be used to simulate a typical networked environment.

**Network Configuration:** Ensure communication between the server and client VMs, configure DNS, and also set up DHCP.

**Active Directory Configuration:** Promote the server to a domain controller, create a new AD forest, and manage users, groups, and policies.

 **We will also set up the following as well**

- Show the creation of an Organizational Unit for better user management.
- Delve into the intricacies of Routing and Remote Access to emulate a corporate intranet.
- Configure the Network Interface Card (NIC), ensuring local and internet connectivity.
- Demonstrate mass user management by leveraging PowerShell to batch-add over 1000 users.
- Interact with our client machine, simulating a login process using one of our newly added users.

By the end of this project, you will have a fully functional AD environment in a virtual lab, enabling you to explore and experiment with various AD features and configurations.

# **Technologies Used:**

- Oracle VirtualBox
- ISO Files for Windows Server 2019
- Windows 10 pro
- Powershell
# **INSTALLATIONS**
**Install Windows Server** 

Prerequisites 

- Oracle VirtualBox with the following preset.
- Assign at least 4GB of RAM and 2 processors.
- Attach the Windows Server ISO as a virtual CD/DVD drive.
- Set up networking (preferably using a bridged network adapter). 
 <details><summary>Click here for details</summary> 
  
  **Install Windows Server:**
  
Boot from the ISO and follow the installation prompts. Choose the appropriate edition (Standard/Datacenter) and select “Desktop Experience” for a GUI.
# <div align="center">
<img width="515" alt="Screenshot 2024-06-22 184854" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/0bd941dd-74f7-4da0-b33c-65630b06242c"></div>


# **Initial Configuration:**

Set a strong administrator password.

# **![image](https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/fee169aa-994a-4888-be4f-c0256269ee25)**
 

Configure network settings for the internal network (static IP recommended).

**<img width="200" alt="Screenshot 2024-06-23 170245" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/0626c031-3ff3-4317-8fbc-cd1f2e099f34">**


# **Configure Active Directory**

Promote the Server to a Domain Controller:

Open Server Manager and select "Add roles and features." Choose "Role-based or feature-based installation."

**<img width="706" alt="Screenshot 2024-06-23 170527" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/4401f5d3-91f9-4799-9a53-84453b0e4a83">**
**<img width="392" alt="Screenshot 2024-06-23 170748" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/0d83773c-c21c-4dcd-be80-7ee44792355d">**

Select the server and then the "Active Directory Domain Services" role.
Follow the prompts to install the role. 

# **Create a New Forest:**
After installation, promote the server to a domain controller.
Create a new forest using using domain name (e.g., "example.com").
Set the forest and domain functional levels.
**![image](https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/e6bade5b-5c23-4b7d-935b-830f393580f7)**



# **DNS Configuration:**

- Ensure the AD DNS zone is created.
- Add necessary DNS records (A, PTR, etc.).

  
 # **Configuring DHCP SERVER**
- Add Roles and features
- Select Role-based or feature-base installation
- click "Next" until the "Role" sections
- Select DHCP Server
- Click next until installed.

  # **Create and Manage AD Objects**
  
  **Create Organizational Units (OUs)**
- Use Active Directory Users and Computers (ADUC) to create OUs.
  <img width="379" alt="Screenshot 2024-06-23 180350" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/69e224ac-544d-4af4-b0bf-31d6da37fd44">
- Select New
- Click on "Organizational Unit"
- Create OU name of your choice

# **Create User Accounts via PowerShell:**

**Script Overview**
The script is designed to create multiple Active Directory (AD) user accounts. It reads user names from a text file, constructs usernames, and creates AD user accounts with a specified password.
# ![image](https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/382559a8-5438-4898-b93c-f70c3f17800c)


**Variable Definitions.**

   # ----- Edit these Variables for your own Use Case ----- #
$PASSWORD_FOR_USERS   = "Password1"
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
- $PASSWORD_FOR_USERS: Specifies the password to be used for all the created user accounts.
- $USER_FIRST_LAST_LIST: Reads the content of names.txt, which contains a list of user first and last names.
  
# **------- Convert Password to Secure String ---------**
  $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
  - Converts the plaintext password into a secure string, which is required by the New-ADUser cmdlet.

# -----------  Create Organizational Unit  ----------#

New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false
- Creates an Organizational Unit (OU) named _USERS in AD.
- The -ProtectedFromAccidentalDeletion $false parameter allows the OU to be deleted without additional confirmation.

# Loop Through User List and Create Users

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
               
# <img width="454" alt="Screenshot 2024-06-23 183512" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/bb7c18dd-f937-430f-90b2-9d7c24eff5fb">

This script simplifies the process of bulk-creating AD user accounts by automating user creation based on a list of names, making it an efficient tool for administrators managing large environments in real-world scenarios.



# Join Client Machines to the Domain

**Set Up Client VMs:**

- Install Windows 10/11 on client VMs.
- Ensure network configuration allows communication with the AD server.
  
  **Join the Domain:**

- Open system properties and change the computer name.
- Select “Domain” and enter your domain name (e.g., "example.com").
- Provide domain credentials when prompted.

Setting up an Active Directory (AD) home lab provides practical experience in network and system administration by 
understanding AD basics, installing and configuring a domain controller, managing users and groups, configuring DNS, using PowerShell for automation, and implementing security practices. This prepares you for IT roles, relevant certifications, and managing AD-related projects. Future exploration can include advanced features like ADFS, integrating on-premises AD with Azure AD, developing advanced scripts with tools like Ansible, implementing AD backup and recovery, and enhancing security and compliance. This hands-on experience prepares you for real-world IT environments and continuously expands your skill set, making you a versatile IT professional.

</details>
