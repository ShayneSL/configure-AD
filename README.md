# On-premises Active Directory Deployment in Azure Virtual Machines

![Microsoft Active Directory Logo](https://i.imgur.com/pU5A58S.png)

This tutorial provides a guide on how to deploy on-premises Active Directory within Azure Virtual Machines. The aim is to simplify the process and make it as easy to use as possible. We will focus on setting up Active Directory Domain Services (AD DS) on a Windows Server 2022 machine and joining a Windows 10 client to the domain. Additionally, we will cover creating user accounts, enabling remote desktop access, and other related tasks.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used

- Windows Server 2022
- Windows 10 (21H2)

## High-Level Deployment and Configuration Steps

1. **Setup Resources in Azure**

   - Create a Resource Group and Virtual Network (VNet) in Azure.
   - Create a Windows Server 2022 Virtual Machine (VM) named "DC-1" in the Resource Group and VNet.
   - Set the NIC Private IP address of the DC-1 VM to static.
   - Create a Windows 10 VM named "Client-1" in the same Resource Group and VNet.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/52348787-edea-4be5-8a0e-03cc7f4899f9)



2. **Ensure Connectivity between the Client and Domain Controller**

   - Connect to Client-1 using Remote Desktop and ping DC-1's private IP address.
   - If the ping fails, log in to DC-1 and enable ICMPv4 in the local Windows Firewall.
    ![image](https://github.com/ShayneSL/configure-AD/assets/88577075/8d411d4e-6216-426b-a357-bfff0ec69e9b)

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/81c8026b-a378-46a7-9f60-fe304525ca56)


   - Verify that the ping from Client-1 to DC-1 succeeds.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/8eaa7c96-6740-4e77-ba9c-348f58b51432)


3. **Install Active Directory**

   - Log in to DC-1 and install Active Directory Domain Services.
   - Promote DC-1 as a domain controller and set up a new forest with a domain name (e.g., mydomain.com).

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/373dab73-ca62-4ad5-bab0-b8fbcfe616fd)

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/1a641fdc-43b0-4fda-9275-0dc05bc0a1da)

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/a9d6126c-2306-4484-89d6-1742b81cc557)



4. **Create an Admin and Normal User Account in AD**

   - Open Active Directory Users and Computers (ADUC) on DC-1.
   - Create an Organizational Unit (OU) called "_EMPLOYEES" and "_ADMINS" within ADUC.
   - Create a new employee account named "Jane Doe" with the username "jane_admin" and add her to the "Domain Admins" Security Group.
   - Log out and log back in to DC-1 as "mydomain.com\jane_admin".

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/6b6192c8-5a8d-4e8f-a66f-ca9845bdf5d8)



5. **Join Client-1 to the Domain**

   - Set Client-1's DNS settings to the private IP address of DC-1.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/332f649c-dbb4-4286-a2ef-9b247c1cf42d)


   - Restart Client-1 and log in as the local admin user.
   - Join Client-1 to the domain and wait for the machine to restart.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/0df02337-834b-4643-a62e-9f0c0a518970)


     
   - Log in to DC-1 and verify that Client-1 appears in ADUC under the "Computers" container.
   - Create a new OU named "_CLIENTS" and move Client-1 into it.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/068ab3c2-35be-448b-9d87-ec501d01c580)



5. **Setup Remote Desktop for Non-Administrative Users on Client-1**

   - Log in to Client-1 as "mydomain.com\jane_admin" and open system properties.
   - Click on "Remote Desktop" and allow "domain users" access to Remote Desktop.
     
![image](https://github.com/ShayneSL/configure-AD/assets/88577075/df10ad6b-fe8f-4e3f-bf9d-4f8c303ee334)


        - Non-administrative users can now log in to Client-1 using Remote Desktop.

5. **Create Additional User Accounts and Test Login**

   - Log in to DC-1 as "mydomain.com\jane_admin".
   - Open PowerShell ISE as an administrator and run the provided PowerShell script to create additional user accounts.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/253cd50e-0a14-472c-bc45-6c75f2bc0c66)


     
   - Verify that the accounts are created in the appropriate OU within ADUC.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/f0206455-c4b9-415f-859b-e12ecbcb9c94)


     
   - Attempt to log in to Client-1 using one of the newly created user accounts.

![image](https://github.com/ShayneSL/configure-AD/assets/88577075/f33fe239-56fe-4157-9c5c-b2c16b91d542)



By following these steps, you will successfully configure on-premises Active Directory within Azure Virtual Machines and establish a domain environment for user and resource management.

## Summary

This project involved the setup and configuration of on-premises Active Directory within Azure Virtual Machines. We created a Windows Server 2022 VM as a domain controller and a Windows 10 VM as a client. The deployment process included configuring network settings, ensuring connectivity, installing Active Directory, creating user accounts, joining the client to the domain, enabling Remote Desktop access, and testing user logins.
