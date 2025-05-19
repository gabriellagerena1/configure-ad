<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com/watch?v=wuIE4p4io7Q)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create a Resource Group in Azure 
- Create a Virtual Network and Subnet
- Create the Domain Controller Virtual Machine
- Create the Client Virtual Machine
- Set the Domain Controller's NIC Private IP Address to be static
- Disable Domain Conroller's Windows Firewall (for testing connectivity)
- Set the Client's VM DNS settings to the Domain Controller's Private IP Address
- Log into Client VM and ping the Domain Controller's Private IP Address
- On Powershell, run ipconfig /all
- Install Active Directory Domain Services in the Domain Controller VM
- Promote you Domain Controller VM as an actual Domain Controller
- Create Organizational Units (OU) in Active Directory
- Create a new employee
- Add new employee to the "Domain Admins" security group to make them an admin
- Log out of VM and log back in with new account
- Join Client VM to your Domain
- Verify the Client VM is in the Domain
- Create Organizational Unit (OU) called Clients
- Set up Remote Desktop for non-administrative users on Client VM
- Configure account lockout policy
- Dealing with account lockouts
- Enabling and disabling accounts
- 

<h2>Deployment and Configuration Steps</h2>

<p>
<img width="675" alt="Screenshot 2025-05-15 at 11 52 03 AM" src="https://github.com/user-attachments/assets/ead0f329-6b57-453b-919a-c718cbf4bb06" />
</p>
<p>
In Azure, type "Resource Group" and click "Create". For "Resource Group" you can name it what you want, for example: "Active-Directory-Lab". Choose to the region then click "Review+Create". 
</p>
<br />

<p>
<img width="665" alt="Screenshot 2025-05-15 at 11 55 44 AM" src="https://github.com/user-attachments/assets/6923397e-6cd2-4ce1-a7f7-127429e4dfae" />
</p>
<p>
In Azure, type "Virtual Networks" and click "Create". For "Resource Group" choose the one you just created. For "Virtual Network name" choose a name, for example: "Active-Directory-VNet". Choose the region and click "Review+Create". 
</p>
<br />

<p>
<img width="509" alt="Screenshot 2025-05-15 at 12 00 00 PM" src="https://github.com/user-attachments/assets/4febb326-c03a-49e0-aaa5-556a317b4de4" />
</p>
<p>
In Azure, type "Virtual Machines" and click "Create". For "Resource Group" choose the one you just created. For "Virtual machine name" choose a name, for example: "dc-1". Use same region as your Virtual Network. For "Image" use "Windows Server 2022 Datacenter: Azure Edition- x64Gen 2" For size, choose anything with 2 VCPUs. Choose a username and password that you would remember. Check the "Licensing" boxes at the bottom and then click "Review+Create" and "Create". 
</p>
<br />

<p>
<img width="509" alt="Screenshot 2025-05-15 at 12 00 00 PM" src="https://github.com/user-attachments/assets/4dd2062c-989c-4bb6-a39a-00a33adda28c" />
</p>
<p>
Create another VM. For "Resource Group" use the one you just created again. For "Virtual machine name" choose a name, for example: "client-1". For "Image" choose "Windows 10 Pro, version 22H2-x64Gen2." For "Size" choose anything with 2 VCPUs. Choose a username and password. Check the "licensing" box and then "Review+Create". 
</p>
<br />

<p>
<img width="1056" alt="Screenshot 2025-05-15 at 12 12 47 PM" src="https://github.com/user-attachments/assets/a5e1aecb-911b-4b87-961b-4fe41cae2ccf" />
</p>
<p>
Click on your Domain Controller VM. On the side, click on "networking" and then "Network Settings". Click "Network interface/IP configuration". You'll see "IP Settings", at the bottom under "Name" click "ipconfig1". Under "Private IP address settings" choose "Static" then at the bottom click "Save". This is so that the IP address won't change. 
</p>
<br />

<p>
<img width="1033" alt="Screenshot 2025-05-15 at 12 27 22 PM" src="https://github.com/user-attachments/assets/be09168a-c0cb-4dc4-ba4e-3fba7135a483" />
</p>
<p>
Copy the Domain Controller VM's public IP address. Go to remote desktop, click the "+" sign at the top and choose "Add PC". Paste the IP address for "PC Name". Choose a name for "Friendly name", for example: "dc-1". Click "Add" then double click it and log into it. Once in the VM, right click the start menu and click "run". Type "wf.msc". Under "Public Profile is Active" click "Windows Defender Firewall Properties". Under "Domain Profile", "Private Profile" and "Public Profile" make sure "Firewall state" is off. Then click "apple" and "ok". 
</p>
<br />

<p>
<img width="1050" alt="Screenshot 2025-05-15 at 12 45 38 PM" src="https://github.com/user-attachments/assets/f9e94dbe-4c00-4c45-b87c-896a8a9da48a" />
</p>
<p>
On Azure, click on your Domain Controller VM. Copy the private IP address. Then click on your Client VM. Go to "Networking", "Network Settings" and click on the box titled "Network interface/IP configuration". On the left, click "DNS servers". Under "DNS servers" choose "custom" and then paste the private IP address from the Domain Controller VM. At the top, click "Save". Go back to "Virtual Machines", check the box for the Client VM and at the top click "restart" and "yes". 
</p>
<br />

<p>
<img width="981" alt="Screenshot 2025-05-15 at 12 58 07 PM" src="https://github.com/user-attachments/assets/e4a08c77-ad75-49dd-8325-aaa7d7d7ec8d" />
</p>
<p>
Copy the Client VM's private IP address. Open remote desktop, click the "+" at the top and choose "Add PC". Paste the IP address for "PC Name" and choose a name for "Friendly name", for example: "client-1". Double click it and log into it. Go back to Azure. Go to the Domain Controller VM, click on it and copy the private IP address. Go back into the Client VM. Click the start menu and type "Powershell". Open it. Type "ping" and then paste the Domain Controller VM's private IP address you just copied. Hit enter. It should look like the above picture. 
</p>
<br />

<p>
<img width="1240" alt="Screenshot 2025-05-15 at 1 05 20 PM" src="https://github.com/user-attachments/assets/df5cfa85-c095-400b-8d74-ebba214af881" />
</p>
<p>
Back on Powershell, type "ipconfig /all" and hit enter. Find "DNS Servers" and make sure it shows the Domain Controller's private IP address. 
</p>
<br />

<p>
<img width="628" alt="Screenshot 2025-05-15 at 1 10 12 PM" src="https://github.com/user-attachments/assets/c4f65067-f178-4b08-860e-679ff1b4110b" />
</p>
<p>
In the Domain Controller VM, click the start menu and go to "Server Manager". Under "Configure this local server" click "Add roles and features". On the window that pops up, click "Next", "Next", "Next" and then where it says "Roles" find "Active Directory Domain Services". Check the box. On the window that pops up click "Add features" at the bottom. Then click "Next", "Next" and "Next" again. Check the box at the top that says "Restart the destination server automatically if required" then click "Install" at the bottom. Once it's finished, click "Close". 
</p>
<br />

<p>
<img width="1234" alt="Screenshot 2025-05-15 at 1 17 30 PM" src="https://github.com/user-attachments/assets/bc3adcb3-4df2-414c-b775-be40ded6bbde" />
</p>
<p>
In the Domain Controller VM, go to the start menu then to "Server Manager". At the top, click on the flag and hit "Promote this server to a domain controller". On the window that pops up, choose "Add a new forest". For "Root domain name" type "mydomain.com" then click "Next". For 'Password" choose a password but you're probably never going to use it. Click "Next". Uncheck the box that says "Create DNS delegation" then click "Next" and "Next" again. Click "Next" then "Next" again. Click "Install" A window will pop up saying "You're about to be signed out", click "Close". It'll restart your VM. Log back into your Domain Controller VM. Since it's a domain controller now, you must type "mydomain.com\" and then your username. For example: "mydomain.com\labuser". And then put your password and log in. 
</p>
<br />

<p>
<img width="1440" alt="Screenshot 2025-05-15 at 1 38 17 PM" src="https://github.com/user-attachments/assets/42df314f-6c11-4f24-bf7c-abac0322b13d" />
</p>
<p>
Back in the Domain Controller VM, got to the start menu then click "Windows Administrative Tools" and choose "Active Directory Users and Computers". Make the screen bigger. On the left, right click "mydomain.com", go to "New" and click "Organizational Unit". For name, type "_EMPLOYEES" then click "OK". Right click "mydomain.com" again and create another OU. For name, type "_ADMINS" This is where your employees and admins accounts will be. Doing this you can also create any other Organizational Units like this. 
</p>
<br />

<p>
<img width="1307" alt="Screenshot 2025-05-18 at 11 22 32 AM" src="https://github.com/user-attachments/assets/ab230245-fea1-42c5-92d8-58d62332687c" />
</p>
<p>
Go to the "_ADMINS" folder you just created and right click the empty space. Go to "New" and choose "user." Fill out the info of the person. Create a "user logon name" that they will us to log in. Click "Next". Create a password. Unclick all of the boxes and hit "Next" and then "Finish". 
</p>
<br />

<p>
<img width="1028" alt="Screenshot 2025-05-18 at 11 27 39 AM" src="https://github.com/user-attachments/assets/679e172d-364f-4499-8352-6311333e68ce" />
</p>
<p>
Right click the account you just created in the "_ADMINS" folder and go to "Properties". Click on "Member of" and hit "Add". Type "domain admins" and click "Check Names" then "OK". Click "Apply" then "OK". Now this account is an admin. 
</p>
<br />

<p>
<img width="903" alt="Screenshot 2025-05-18 at 11 34 41 AM" src="https://github.com/user-attachments/assets/f1d746e2-dab5-4e34-8e41-8a1d7aa936d8" />
</p>
<p>
Log out of the Domain Controller VM. Then, log back into it using the username and password from the account you just created. Use "mydomain,com\" and then the username. For example: "mydomain.com\jane_admin". 
</p>
<br />

<p>
<img width="995" alt="Screenshot 2025-05-18 at 11 39 53 AM" src="https://github.com/user-attachments/assets/78c85553-4dc4-4bb9-88ef-14e05b697d43" />
</p>
<p>
Log in to you Client VM if you're not already. Once in, right click the start menu and go to "System." On the right, click "Rename this PC (advanced)". Under "Computer Name" click "change". Under "Member of" check "Domain" and type "mydomain.com". Click "OK". On the window that pops up, for username use what you log into the Domain Controller with. For example: "mydomain.com\jane_admin". Type the password for this and click "OK". On the window that pops up saying welcome, click "OK". When it says must restart click "OK". When it comes up, click "Restart Now". It'll restart and be a member of the domain. 
</p>
<br />

<p>
<img width="1035" alt="Screenshot 2025-05-18 at 11 48 16 AM" src="https://github.com/user-attachments/assets/de863103-f22f-4b57-b6a6-4b06857713fe" />
</p>
<p>
Go Back into the Domain Controller VM. Click the start menu and type "Active Directory Users and Computers". Open it. Expand "mydomain.com", click "Computers" and make sure you see the Client VM in there. 
</p>
<br />

<p>
<img width="1158" alt="Screenshot 2025-05-18 at 11 52 27 AM" src="https://github.com/user-attachments/assets/7e72b709-96f8-4b99-9146-3329e7714112" />
</p>
<p>
Right click "mydomain.com", go to "New" and choose "Organizational Unit". For name type "_CLIENTS". Then go back to "Computers" and drag the Client VM into the "_CLIENT" folder you just created. Click "Yes". 
</p>
<br />

<p>
<img width="1115" alt="Screenshot 2025-05-18 at 11 58 03 AM" src="https://github.com/user-attachments/assets/0f3240ef-af4c-4c6a-8fc1-e15c6155abbc" />
</p>
<p>
Log in to Client VM (using "mydomain.com\"). Once in, right click the start menu and go to "System". On the right, click "Remote Desktop". On the bottom, click "Select users that can remotely access this PC". Click "Add" and type "Domain Users". Click "Check Names" and "OK". Click "OK" again. Now you can log into the Client VM as a normal, non-administrative user. 
</p>
<br />

<p>
<img width="809" alt="Screenshot 2025-05-18 at 12 06 28 PM" src="https://github.com/user-attachments/assets/1cad2350-37bb-43a1-a433-0152d413be47" />
</p>
<p>
In the Domain Controller VM, right click the start menu, go to "Run" and type "gpmc.msc". You'll see the Group Policy Management console. Under "mydomain.com" right click "Default Domain Policy" and click "Edit". Click "Computer Configuration", "Policies" and expand "Window Settings". Expand "Security Settings" and 'Account Policies" and click "Account Lockout Policy". On the right, click "Account Lockout Duration". Check the box and type the number of minutes you would like accounts to be locked out for. Then click "OK". For "Account Lockout Threshold" set the number of attempts people have before getting locked out. Fill out the rest of the info in the other two apparent to you. Once done configuring this, you can close it. Log into Client VM to force this Group Policy to refresh. Once in, go to the start menu and type "cmd". Open it. Type "gpupdate /force" and hit enter. Now the policy is updated. 
</p>
<br />

<p>
<img width="273" alt="Screenshot 2025-05-18 at 12 16 52 PM" src="https://github.com/user-attachments/assets/22c383c1-0815-48ad-b0af-a16b63c4348e" />
</p>
<p>
In the Domain Controller VM, open "Active Directory Users and Computers". In "_EMPLOYEES" find the account that has been locked out and double click it. Or if you can't find it, you can right click "mydomain.com" and click "Find". Type the name of the account and hit enter. Once it comes up, double click it. Go to "Account" and check the box that says "Unlock Account". Then click "Apply" and it should be unlocked now. If the person needs to reset the password you can right click the account and go to "Reset Password". Here you can type the new password and unlock the account. 
</p>
<br />

<p>
<img width="160" alt="Screenshot 2025-05-18 at 12 26 50 PM" src="https://github.com/user-attachments/assets/122012b5-16b0-42bd-bba3-e302d6e836ac" />
</p>
<p>
To enable/disable accounts, in "Active Directory Users and Computers", find the account that needs aid. Right click it and choose "Disable Account". If you want to enablee it, right click the account and click "Enable Accountt". 
</p>
<br />
