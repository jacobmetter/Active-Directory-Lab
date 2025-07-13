# Active-Directory-Lab

## Objective
The objective of this lab is to set up a Windows 2019 server and configure Active Directory where we will create users, groups and manage permissions to gain proficency in Windows Server and Active Directory.

## Set up
<img width="1736" height="1032" alt="image" src="https://github.com/user-attachments/assets/49d34c13-08c8-4f3e-83ef-b54985aaab03" />
The set up will look something like this. In this lab we will be deploying two Virtual Machines (VMs) One is our Server, the other is a client. I will be using Oracle Virtual Box, but any VM software will work. Our Domain Controller (DC) will have two NICs, one is set up to face inside to our private network and will be connected to our client VM. Our second NIC will be facing externally to the internet. This will be connected to our host machine which we will configure DNS and DHCP to give our VM actauly IP addresses.

On our DC we will also configure a DCHP scope for our internal network "users" which we will be using a powershell script to generate users to simulate what it would be like to manage permissions of an Office of 1,000+ employees.

## Step 1: Create Virtual Machines
