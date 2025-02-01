<h1> Create a Domain Name User in a Domain </h1>
In this tutorial, we create a domain name user using Active Directory Users and Computers console. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop

<h2>Operating Systems Used </h2>

- Windows 10 (22H2) ***Client-1***
- Windows (Windows Server 2022 Datacenter Azure Edition) ***DC-1***

<h2>High-Level Steps</h2>

 ***- Prereq: Create a Windows VM and Windows Server VM in Azure. Select the link to learn how to [create VM in Azure](https://github.com/joann-camacho/create-windows-virtual-machine). Remote login to Windows Server VM (DC-1) and install ADDS. Select the link to learn how to [install Active Directory](https://github.com/joann-camacho/install-active-directory)*** 

***Create a Domain Admin user within the domain***
  
- Step 1: Create Organization Units (OU) named: _EMPLOYEES, _ADMINS, and _CLIENTS in Active Directory Users and Computers (ADUS) 
- Step 2: Create a new employee named “Jane Doe”. The username is: “jane_admin”. Add jane_admin to the “Domain Admins” Security Group.
- Step 3: Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”.

 ***Join Client-1 to your domain (mydomain.com)***

- Step 1: Log in to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
- Step 2: Log in to the Domain Controller and verify Client-1 shows up in ADUC


***Setup Remote Desktop for non-administrative users on Client-1 and Create a bunch of additional users and attempt to log into client-1 with one of the users***

- Step 1: Log into **Client-1** as mydomain.com\jane_admin. Open system properties and set up Remote Desktop configuration to allow “domain users” access to the remote desktop.
- Step 2: Log in to **DC-1** as jane_admin.
- Step 3: Open PowerShell_ISE as an administrator. Create a new File and paste the contents of the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it. Run the script and observe the accounts being created.
- Step 4: Open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES). Attempt to log into Client-1 with one of the accounts (take note of the password in the script)



<h2>Actions and Observations</h2>

<p>
Step 1: Connect to the Domain Controller (DC-1). Select the Windows (Start) icon and select Windows Administrative Tools > Active Directory Users and Computers.
  
 ![image](https://github.com/user-attachments/assets/5b058ff6-a43e-4b7f-8e7c-a9b0cb30442b)

</p>
 Select and right-click on mydomain.com forest. Select 'New'. Select 'Organization Units' (OU). Create and name 3 different OUs: _EMPLOYEES, _ADMINS, and _CLIENTS
 
![image](https://github.com/user-attachments/assets/648b0515-1d94-4b4d-8c5e-b86a01e63f9d)
</p>

![image](https://github.com/user-attachments/assets/cdce1236-72b3-42d5-b863-28e331c68e89)
</p>

![image](https://github.com/user-attachments/assets/2fd5be47-74f6-4367-90a2-c926e7d009b1)

</p>

<br />

<p>
Step 2: Right-click on the 'Users' file. Select 'New'. Select 'User'.

![image](https://github.com/user-attachments/assets/fac8f07d-dffd-4b93-821e-63cc7d85bd6f)

</p>

Create a new employee named “Jane Doe”. The username is: “jane_admin”. 
  
![image](https://github.com/user-attachments/assets/025091aa-c176-4690-befc-bc67311c8c52)

![image](https://github.com/user-attachments/assets/9ecd1a61-3da8-4fec-9b08-96d05e54b145)

*Notice* when creating a new user **always** select the **'user must change password at next logon'** for security purposes. However, in this tutorial, I will not for simplicity. Select 'Next' and 'Finish'. Observe that the user was created in the file.
</p>

![image](https://github.com/user-attachments/assets/c18c157f-5ad4-437c-831a-fb6e5a8bae97)

</p>

Select Jane Doe's file. Select the 'Member of' tab. Select the 'Add' button. Type: Domain Admin, and select 'Check Names'. Once found in the domain- add jane_admin to the “Domain Admins” Security Group. Select 'Apply' and 'Ok'.

![image](https://github.com/user-attachments/assets/42228f98-57da-496d-b892-2ca978c132d5)
![image](https://github.com/user-attachments/assets/faaed4bb-6cba-4034-88df-e6afbc38682f)

<br />

<p>
Step 3: In Wireshark, expand 'Ethernet II,Src' (Level 2 of the OSI model), and notice the MAC address given by Azure software. In Powershell type: 'ipconfig /all' to verify Windows VM MAC address are the same. This is important to analyze the source and destination MAC address it belongs to, which in this case Windows is the source.
</p>
<p>
  
![image](https://github.com/user-attachments/assets/e2fee6ee-7a54-4b28-ab0e-cfce5d2f8471)
  
</p>
<br />

**Configuring Firewall (NSG)**

 <p>
Step 1: (Continue on your Windows VM) In the Powershell command line, Type 'ping 10.1.0.5 -t'. To initiate a non-stop Ubuntu VM ping. Observe that the ping is perpetual or non-stop and you can also see in Wireshark the data is non-stop as well.
</p>
<p>
  
![image](https://github.com/user-attachments/assets/0fc2f695-d26e-4626-b6c6-5bd2c6ad63b4)
</p>

Step 2: In Azure select your Ubuntu VM. In the Overview section > Networking > Network Settings > Scroll down and Select: ubuntu-vm-linux-nsg.
![image](https://github.com/user-attachments/assets/130f4403-ea7c-4f00-bc51-ccf736c7ec9c)

  Select Settings on the left-hand side and select 'Inbound security rule'. I am attempting to set a rule to disable incoming (inbound) ICMP traffic. Select 'Add'. In 'Destination port range' change to an asterisk (*), since ICMP does not have a port number, the asterisk defaults to all ports. In 'Protocol' select ICMPv4. In 'Action' select Deny. In 'Priority' type: 290. This means that it will go before any other rules set for a higher number (for example SSH at 300).
<p>
  
![image](https://github.com/user-attachments/assets/dd05ff2e-0b6c-4a63-bb4a-11ad98cb8a5e)

![image](https://github.com/user-attachments/assets/76cb82c8-8e22-4fac-b0c1-92366be3905e)

notice back in the Windows VM - the ongoing ping timed out in Powershell AND only Request is shown in Wireshark. This means the NSG rule worked!

![image](https://github.com/user-attachments/assets/dc73c60a-bb90-4176-984d-63c5a6de4714)
</p>

Delete the NSG rule in Azure and notice the ongoing ping is now functioning again. 

![image](https://github.com/user-attachments/assets/603f320e-efa7-4298-a090-aad7eec8e56f)

![image](https://github.com/user-attachments/assets/e3cb8464-a5b8-43d8-b8e4-537b78754ae7)


</p>
<br />     
      
***C- SSH Network Protocol***

<p>
Step 1: In the Windows VM, start a packet capture using  [Wireshark](https://www.wireshark.org). Use the filter search and type: SSH.
</p>
<p>
 
![image](https://github.com/user-attachments/assets/723c962a-ae7d-4f8e-b7bc-865d9c3bfc83)  
</p>

(After installing Wireshark licensing terms) Open Wireshark. Double-click on 'Ethernet' to view IP traffic. This traffic is the IP packets reciprocating in the background while using the computer. 
<p>
  
![image](https://github.com/user-attachments/assets/8a7ba9ea-4008-4fce-9bd1-5a96153ddd3c)  
</p>
<br />

<p>
Step 2: In Powershell "SSH into" the Linux VM by typing: ssh labuser1@<private IP address>. (the private IP address must be the Linux VM, which is 10.1.0.5). Notice that communication (secured traffic) is observed on the Wireshark analyzer. Enter the Linux VM password you created. (When typing the password, no letter will appear for security reasons.) You will see that more traffic was analyzed in Wireshark. Once the password is validated and confirmed notice the green hostname is now the Linux VM name.
</p>
<p>
 
![image](https://github.com/user-attachments/assets/cffd25bf-8678-4188-b3ac-7b68ad2be7bb)
  
</p>
<br />

<p>
Step 3: Type: 'hostname', to verify the server. Type 'id', to verify the user name.  Type 'exit' to exit out of the Linus VM server. 
</p>
<p>
  
![image](https://github.com/user-attachments/assets/2e23ebd1-0da0-410c-accd-51af635320d4)
  
</p>
<br />

