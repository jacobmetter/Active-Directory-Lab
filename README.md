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

First, go to your Domain Controller and inside settings go to Network and Internet>Network connections. There you will see two Ethernets.

<img width="1076" height="890" alt="image" src="https://github.com/user-attachments/assets/709a8ed1-aad4-42d2-84ca-f0c036174f3e" />

For clarity, rename "Ethernet" to "External Internet" and "Ethernet 2" to "Internal Network" for clarity. Click on "Internal Network" and go to properties>Internet Protocol Version 4 (TCP/IPv4) and manually give it an IP address of 176.16.0.1 with a /24 subnet mask. For the perferred DNS server use loopback address of 127.0.01. 

<img width="1075" height="891" alt="image" src="https://github.com/user-attachments/assets/595d6f62-36c0-4f52-91aa-2dd4142e879b" />

### External Network
Our external network is already connected because from the VirtualBox settings we have it configured so Adapter 1 uses NAt. There is no further configuration for our External Network

<img width="943" height="922" alt="image" src="https://github.com/user-attachments/assets/91b9daf1-bd94-4322-b9db-df53147bed67" />

## Step 3:  Configuring Active Domain/Active Directory Domain Services
Go to the server manager dashboard and click on "Add roles and features". Click next through everything and select "Active Directory Domain Services" and install it onto the server.

<img width="1071" height="895" alt="image" src="https://github.com/user-attachments/assets/9566c384-cf6f-46a3-aea6-bb148c261683" />

Then go to the flag at the top left and promote this server to the domain controller. Then go to add new forest and create a new root domain. You can name this whatever you want. Then add a password once prompted. Click through next for everything else.

<img width="1075" height="895" alt="image" src="https://github.com/user-attachments/assets/6f85118d-0aa9-4b76-aea3-776d04bfb2cb" />

<img width="1072" height="893" alt="image" src="https://github.com/user-attachments/assets/a1011d84-584b-4a07-8469-6d29aac6779b" />

### Creating Domain Admin Account
Once your DC is done restarting we are going to create our own dedicated domain admin account. At the bottom left, go to settings>Windows Administration Tools> Active Directory Users and Computers. Under mydomian.com create an organization and call it "ADMINS". Create a uder with your login credentials as shown.

<img width="1073" height="893" alt="image" src="https://github.com/user-attachments/assets/5be03f56-ab8c-42f0-bf41-f947cae0e5d4" />

Then right click on your name, go to properties>Member of>Add and in the box type "Domain Admins" and click add and apply. Then sign out of the current account and sign into the new admin account we just created.

<img width="1073" height="889" alt="image" src="https://github.com/user-attachments/assets/c2b32bf5-497b-48af-8273-589d125b5c94" />

<img width="1075" height="891" alt="image" src="https://github.com/user-attachments/assets/cfccc80c-88b7-41d0-874a-a3aecadd4486" />

## Step 4: Configure RAS and NAT

RAS and NAT will allow our client VM to connect to the external internet from our domain controller. Go to the Server manager and click "Add roles and fatures">Remote Access>Routing> then click next and install everything.

<img width="1070" height="885" alt="image" src="https://github.com/user-attachments/assets/0dc8fed9-44c9-43a8-91db-8c2b74c10bb6" />

Once that is done downloading go to "Tools" at the top right and go to "Routing and Remote Access." Right click on your domain controller and click "Configure and Enable Routing and Remote Access". Click through and select NAT. and verify the networks are correct.

<img width="1074" height="895" alt="image" src="https://github.com/user-attachments/assets/b4c695ae-9935-41dd-9502-fba80a00d9d7" />

## Step 5: Configure DHCP

To set up our DC as a DCHP server, go to "add roles and features">DHCP>next through everything to begin installation. As a DHCP server. our DC will be assigneing IP addresses automatically to users when they connect to the network. We need to set up our range of IPs so the DC can assign one to a user from the pool of IP addresses. From our initial set up, we want our range of IPs to be from 172.16.0.100-200. So we will have a pool of 100 IP address to lease to our users.

<img width="1070" height="891" alt="image" src="https://github.com/user-attachments/assets/f3908441-1d01-467b-a169-538900e40ffc" />

Go to "Tools">DHCP then open up our domain, right click on IPv4 and click "New Scope". Give it a name and input the information shown.

<img width="1073" height="891" alt="image" src="https://github.com/user-attachments/assets/f15dbc7b-98b0-4717-9c0e-757a61b3b419" />

Click next through the options until you get to Default gateway. We want this to be 172.168.0.1 this is our domain controller's IP address and our DNS server to be 172.16.0.1. Finally ignore WINS server set up.

<img width="1072" height="890" alt="image" src="https://github.com/user-attachments/assets/61982bdf-4585-45dc-9660-62447fd7974d" />

Fianlly, authorize your DHCP pool in the domain, refresh and your IPv4 should turn green.
<img width="755" height="564" alt="image" src="https://github.com/user-attachments/assets/ee506488-c032-4c26-87a4-354e323b21d0" />

## Step 6 Generate Users




