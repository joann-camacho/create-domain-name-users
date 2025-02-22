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
- Step 2: Create a new employee named “Jane Doe” (administrator). The username is: “jane_doe”. Add jane_doe to the “Domain Admins” Security Group.
- Step 3: Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_doe”.

 ***Join Client-1 to your domain (mydomain.com)***

- Step 1: Log in to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
- Step 2: Log in to the Domain Controller and verify Client-1 shows up in ADUC


***Setup Remote Desktop for non-administrative users on Client-1 and Create a bunch of additional users and attempt to log into client-1 with one of the users***

- Step 1: Log into **Client-1** as mydomain.com\jane_doe. Open system properties and set up Remote Desktop configuration to allow “domain users” access to the remote desktop.
- Step 2: Log into **DC-1** as jane_doe (admin). Open PowerShell_ISE as an administrator create a new File and paste the contents of the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it.
- Step 3: Run the script and observe the accounts being created.
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

Create a new employee named “Jane Doe”. The username is: “jane_admin”. Select 'Next'.
  
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
Step 3: Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”.
  
</p>
<p>
  
![image](https://github.com/user-attachments/assets/fc61b3dc-49be-4a86-aba4-cc0dba7f05c1)
  
![image](https://github.com/user-attachments/assets/0b09322a-d4ab-43a0-b4c1-ab204156f4fb)
  
</p>
<br />

 ***Join Client-1 to your domain (mydomain.com)***

 <p>
Step 1: Log in to Client-1 as the original local admin (labuser) 
<p>
  
![image](https://github.com/user-attachments/assets/d5b1b849-da97-4347-9c6b-246288338ee2)

<p>
Right-click on the Windows start menu and select 'System'. Select 'Rename this PC (advanced)'. 
<p>

![image](https://github.com/user-attachments/assets/9db5db32-bda3-4170-80df-f1555dfc0cf1)
<p>
'To rename this computer or change its domain', select 'Add'. Click on 'Member of- Domain', and Type: mydomain.com. select 'Ok'. 
</p>

![image](https://github.com/user-attachments/assets/2fd32726-44a8-46c8-b2a1-f311bbb6389d)

Enter username: mydomain.com\jane_doe (admin) and the password. Select 'Enter'. Then restart Cleint-1. 
<p>

 ![image](https://github.com/user-attachments/assets/10ca013c-d4ac-459f-b479-5c74c7dcc8e4)
 
</p>
</p>

Step 2: Log in to the Domain Controller with Jane Doe's credentials (admin).

![image](https://github.com/user-attachments/assets/130f4403-ea7c-4f00-bc51-ccf736c7ec9c)
 <p>
   When logged on. Go to Active Directory Users and Computers. Select the domain name> OU: computers. Observe that Client-1 is in the file.
 </p>
 
![image](https://github.com/user-attachments/assets/71747a57-25cc-485a-9884-fe6db733e827)

Select Client-1 file and drag and drop into the _CLIENTS organization unit.

![image](https://github.com/user-attachments/assets/208d757b-4925-4890-b338-df7544a5874c)

</p>
<br />     
      
***Setup Remote Desktop for non-administrative users on Client-1 and Create a bunch of additional users and attempt to log into client-1 with one of the users***

<p>
Step 1: Log into Client-1 as mydomain.com\jane_admin. Right-click on Windows icon at the bottom left and Open 'system'. Click 'Remote Desktop'.

![image](https://github.com/user-attachments/assets/c003a532-ec78-4965-aeba-c7bcac0d6ce3)

<p>
  
Under 'User accounts> Select users that can remotely access this PC'. Select 'Add'. Type: Domain Users' Select 'Check Names'. Select 'Ok'. 
(You can now log into Client-1 as a normal, non-administrative user now)

![image](https://github.com/user-attachments/assets/bb3da690-1e0a-443b-9eb7-53b6c6413191)
![image](https://github.com/user-attachments/assets/fc4fa469-e684-470a-bf27-f1fb4e24b654)

</p>
<p>

<br />

<p>

Step 2: Log in to **DC-1** as jane_doe (admin). Open Microsoft Edge and copy and paste the Script's link address. Open PowerShell_ise **as an administrator**- Create a new File, name it 'Users' save it on your desktop. Click on the 'copy raw file' which is the image of two boxes on the top right side of the file. Paste the contents of the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into the Powershell_ISE script. 

</p>
<p>
 
![image](https://github.com/user-attachments/assets/9f7c0a54-8a30-4205-9349-6e073bc4730d)
  
</p>
Step 3: 'Run' the script. Observe that there will be a running list of user names in blue. the password to all users is Password1 -per script.

![image](https://github.com/user-attachments/assets/336ebd2e-596c-4537-bbc4-67306ea16954)


<br />

Step 4: Open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES). 
<p>
 
![image](https://github.com/user-attachments/assets/24d032c9-2189-451f-9d18-ef3b49efa317)


Attempt to log into Client-1 with one of the accounts (take note of the password in the script)
</p>

![image](https://github.com/user-attachments/assets/c6ff20a2-1453-47d6-970f-b6004174c098)

<br />
