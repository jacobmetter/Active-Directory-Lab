# Active-Directory-Lab

## Objective
The objective of this lab is to set up a Windows 2019 server and configure Active Directory where we will create users, groups and manage permissions to gain proficency in Windows Server and Active Directory.

## Set up
<img width="1736" height="1032" alt="image" src="https://github.com/user-attachments/assets/49d34c13-08c8-4f3e-83ef-b54985aaab03" />
The set up will look something like this. In this lab we will be deploying two Virtual Machines (VMs) One is our Server, the other is a client. I will be using Oracle Virtual Box, but any VM software will work. Our Domain Controller (DC) will have two NICs, one is set up to face inside to our private network and will be connected to our client VM. Our second NIC will be facing externally to the internet. This will be connected to our host machine which we will configure DNS and DHCP to give our VM actauly IP addresses.

On our DC we will also configure a DCHP scope for our internal network "users" which we will be using a powershell script to generate users to simulate what it would be like to manage permissions of an Office of 1,000+ employees.

## Step 1: Create Virtual Machines

Fist thing you need to do is get the ISOs from Microsoft's website https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019 One thing to note is you will need to create an account with Microsoft to download the ISO. Do this with both Windows Server and Windows 10 ISO. 

Next we need to go into Virtual Box and Create our two Machines.
### Domain Controller
<img width="942" height="939" alt="image" src="https://github.com/user-attachments/assets/2960b215-a04b-4c2e-8897-f6d8bafc8d0f" />
This is the set up I have for my Domain Controller. You can adjust the RAM for more or less depending how fast you want it to run, but make sure you have at least 20Gb of hard disk space. Take a look at the Network section. I added two Network adaptors. One is NAT which is facing the outside internet and is connected to my host machine. The other is facing the internal network which will be connected to our client machine when we build it.

<img width="633" height="466" alt="image" src="https://github.com/user-attachments/assets/be032e26-06c3-45ee-911a-ebc136c964de" />
When you start up Windows Server for the first time you will have to go through initial configuration. Make sure to select the "Desktop Experience" as shown. This gives you a nice GUI to work with instad of having to do everything in command line. Which makes it much easier. Go through all the Windows and select the most appropriate options. Generate an easy to remember password. and Let the DC go through all the inital bootup processes.

### Client Machine
<img width="943" height="926" alt="image" src="https://github.com/user-attachments/assets/46a21928-684f-473e-b26a-a3a345bedd7b" />
This is the set up I have for my client machine. Feel free to adjust the amount of RAM and processors dedicated to your devcie as neccesary. Make sure to go to settings>network and change the network adapter to the internal network. This will allow it to only connect and get IPs from the DC that we just created. 

Start up the machine and go through all the initial configuration steps and set up an easy to remember password. Click through and ignore all the promotional stuff for various Microsoft products.

<img width="1021" height="831" alt="image" src="https://github.com/user-attachments/assets/2bf3499b-309c-4db5-9c7d-b3ba83c4d274" />
After several minutes we finally have our client VM created. For now, close this VM as we will come back to it later.

## Step 2: Confiure Networks on Domain controller
### Internal Network

