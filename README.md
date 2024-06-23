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

Prerequisites <details><summary>Click here for details</summary>

- Oracle VirtualBox with the following preset.
- Assign at least 4GB of RAM and 2 processors.
- Attach the Windows Server ISO as a virtual CD/DVD drive.
- Set up networking (preferably using a bridged network adapter). </details>
  **Install Windows Server:**
  
Boot from the ISO and follow the installation prompts. Choose the appropriate edition (Standard/Datacenter) and select “Desktop Experience” for a GUI.
<div align="center">
<img width="515" alt="Screenshot 2024-06-22 184854" src="https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/0bd941dd-74f7-4da0-b33c-65630b06242c"></div>


# **Initial Configuration:**

Set a strong administrator password.

 **![image](https://github.com/sunny4lab-project/Active-Directory-VM-Lab/assets/139194279/fee169aa-994a-4888-be4f-c0256269ee25)**

Configure network settings (static IP recommended).
Install updates and necessary drivers.
3. Configure Active Directory
Promote the Server to a Domain Controller:

Open Server Manager and select "Add roles and features."
Choose "Role-based or feature-based installation."
Select the server and then the "Active Directory Domain Services" role.
Follow the prompts to install the role.
Create a New Forest:

After installation, promote the server to a domain controller.
Create a new forest (e.g., "example.com").
Set the forest and domain functional levels.
Configure DNS (leave default settings unless specific changes are required).
Set Directory Services Restore Mode (DSRM) password.
Complete the Setup:

The server will restart to complete the promotion.
4. Configure DNS and DHCP (Optional)
DNS Configuration:

Ensure the AD DNS zone is created.
Add necessary DNS records (A, PTR, etc.).
DHCP Configuration (if not using static IPs):

Install the DHCP server role.
Configure DHCP scope and options.
