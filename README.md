# Active Directory

This project provides a basic step-by-step walkthrough setting up an Active Directory (AD) environment with Windows Server 2022 and Windows 10 clients. The goal of this Lab is to understand domain controller setups, DNS, DHCP, Organisational Units (OUs), user and group management, authentication, and Group Policy Objects (GPOs).

---

## 🧮 AD Overview

### What is Active Directory?  
**Active Directory (AD)** is a directory service developed by Microsoft that enables secure access to resources in a networked Windows environment. Primarily used for **authentication** and **authorisation**, AD allows IT admins to manage **users**, **computers**, and **resources** from a centralised location.

### Why is Active Directory Used?  
AD simplifies the intimidating task of managing hundreds or thousands of users, computers, printers, servers, and devices manually through:
- Centralised management of users, groups, and computers
- Enhanced security through access controls and authentication policies
- Scaling to businesses of all sizes with multiple domains and sites
- Integrating directly with Microsoft 365, Azure, and other enterprise tools

### Key Components
1. **Domain Controller (DC)**: acting as the central authority for user and device access - the server that stores the AD database and handles authentication requests
2. **Objects**: containing Users, Computers, Groups (collection of users with shared permissions), and Organisational Units (OUs - containers for organising objects)
3. **Forests**: highest level in an AD hierarchy that contains one or more domains
4. **Trees**: collection of related domains within a forest
5. **Domain**: logical grouping of AD objects under a single domain name (e.g., example.com)
6. **Group Policy**: tools to centrally enforce security settings and configurations, used to apply password policies, desktop settings, and software installations across multiple computers and users
7. **Lightweight Directory Access Protocol (LDAP)**: protocol used to access and manage information stored in AD
8. **Kerberos Authentication**: secure method used to authenticate users and devices in a network

---

## 🛠 Tools & Technologies Used
- VM Manager: Oracle VirtualBox
- Windows Server 2022 ISO
- Windows 10 ISO
- Active Directory Domain Services (AD DS)
- DNS / DHCP Server
- Group Policy Management Console (GPMC)
- PowerShell

---

## 🔧 Topology
- Domain Name: bane.com
- Server Name: DC01
   - OS: Windows Server 2022
   - Role: AD DS, DNS, DHCP
   - Static IP: `192.168.0.1/24`  
- Client Name: PC01
   - OS: Windows 10
   - DHCP Enabled  
- Server Name: DC02
   - OS: Windows Server 2022
   - Role: AD DS, DNS, DHCP
   - Static IP: `192.168.1.1/24`  

---

## 🚀 Walkthrough

### Step 1: Server Setup and Domain Controller Configuration

1. Create a new virtual machine in VirtualBox using the **Windows Server 2022 ISO**
   !(images/step.png)

2. Rename the **Computer name** to `DC01`, requiring a restart
   !(images/step.png)

3. Assign a **static IP address** of `192.168.0.1/24` to the server
   !(images/step.png)

4. From **Server Manager**
   - Navigate to **Manage** > **Add Roles and Features**
   - Keep default configurations
   - Install the **Active Directory Domain Services** role and add features
   - !(images/step.png)
  
5. Promote this server to a **Domain Controller**
   - Select **Add a new forest**, setting **Root domain name**: `bane.com`
   - Complete the setup with default options and define the **DSRM password**
   - Once the **Prerequisite Check** passess successfully, click **Install**
   - Once installed, a restart will be required
   - !(images/step.png)

---

### Step 2: DNS Configuration

1. From **Server Manager**
   - Navigate to **Tools** > **DNS**
   - **DNS Configuration** is automatically handled (Forward Lookup Zone)
   - !(images/step.png)
  
2. Manually create a **Reverse Lookup Zone**
   - Right-click **Reverse Lookup Zone** and select **New Zone...**
   - Keep the default configurations
   - Enter **Network ID**: `192.168.0` and click Next
   - !(images/step.png)
  
3. From **Forward Lookup Zones**
   - Navigate to `bane.com` > **dc01** and open properties
   - Tick the **Update associated pointer (PTR) record box
   - !(images/step.png)
  
4. Click **Refresh** to see the **Pointer (PTR) appear
   !(images/step.png)

---

### Step 3: DHCP Configuration

1. From **Server Manager**
   - Navigate to **Manage** > **Add Roles and Features**
   - Keep default configurations
   - Install the **DHCP Server** role and add features
   - !(images/step.png)
  
2. Complete **DHCP** configuration
   - Finish setup with default configurations
   - !(images/step.png)
  
3. Create a new **IPv4 Scope** for IP distribution from **Server Manager**
   - Navigate to **Tools** > **DHCP**
   - Right-click **IPv4** and select **New Scope...**
   - **Name**: `BaneIPv4`
   - Enter the desired **IP Address Range**
   - Keep default configurations
   - Set the **Router (Default Gateway)** to `192.168.0.254`
   - !(images/step.png)
  
---

### Step 4: Joining a Client to the Domain

1. Create a new virtual machine in VirtualBox using the **Windows 10 ISO** on the same virtual network
   !(images/step.png)

2. Verify network settings with `ipconfig /all` in **Command Prompt**, ensuring **DNS** and **DHCP** are working correctly
   !(images/step.png)

3. Join the `bane.com` domain from **System Settings**
   - Navigate to **Advanced system settings** > **Computer Name** > **Change...**
   - Join `bane.com` domain
   - Enter domain administrator credentials
   - Restart the virtual machine
   - !(images/step.png)

---

### Step 5: Creating Organisational Units (OUs)

1. From **Server Manager**
   - Navigate to **Tools** > **Active Directory Users and Computers**
   - Right-click `bane.com` > **New** > **Organisational Unit**
   - **Name**: Bane
   - !(images/step.png)

2. Create a **Nested OU**
   - Right-click `Bane` > **New** > **Organisational Unit**
   - **Name**: IT
   - !(images/step.png)
  
2. Create another **Nested OU**
   - Right-click `Bane` > **New** > **Organisational Unit**
   - **Name**: Staff
   - !(images/step.png)

---

### Step 6: Creating AD Users

1. From **Server Manager**
   - Navigate to **Tools** > **Active Directory Users and Computers**
   - Expand `Bane`
   - Right-click `IT` > **New** > **User**
   - Enter credentials
   - !(images/step.png)
  
2. Create a **second user**
   - Expand `Bane`
   - Right-click `Staff` > **New** > **User**
   - Enter credentialsll
   - !(images/step.png)
  
3. Give a user **Domain Admin** privileges
   - Right-click `Bruce Wayne` > **Properties** > **Member Of**
   - Click **Add...**
   - Enter `Domain Admins` and **Check Names**
   - !(images/step.png)

---

## 🎯 Key Takeaways

- Point 1  
- Point 2  
- Point 3
