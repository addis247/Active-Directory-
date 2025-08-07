<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

![image](https://github.com/user-attachments/assets/a64de9aa-1805-4536-9b9b-dade7933abc7)


<h1>Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the deployment and setup of Active Directory using Windows 11 and Microsoft Azure Virtual Machines. Feel free to try it for yourself at no cost since <a href="https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account">Microsoft Azure</a> can be used with a free subscription for 30 days and/or $200 worth of credits. Lets do it! <br />

<h2>💻 Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>👨‍💻 Operating Systems Used </h2>

- Windows 11 Pro
- Windows 10 (21H2)
- Windows Server 2022

<h2>🧰 Deployment and Configuration Steps</h2>

***Assuming you have a Microsoft Azure account already setup, we will first need to create a Resource Group, Virtual Network and Subnet, and the Domain Controller VM using Windows Server 2022. I will name it DC1 and create the username: labuser and password: Password1234 just to keep things simple.***

- In Microsoft Azure, type "resource" in the search bar at the top and click on "Resource groups"
- Click the "Create" button either towards top left or the blue one in the middle of the screen.
- I'll name it "ActiveDirectoryLab" and click "Create + Review" -> then click "Create"

![image](https://github.com/user-attachments/assets/d504359a-2c64-459b-82bd-abb7fe7f2c92)

![image](https://github.com/user-attachments/assets/9e74d2f7-b68f-4293-b75f-d1a4c626a880)

![image](https://github.com/user-attachments/assets/0365cc48-65e9-45b3-b0f5-54bd7e61289a)

![image](https://github.com/user-attachments/assets/c083bab4-c009-4e2d-b7d6-5131e22c92d6)

**Next, we'll create our virtual network.**

- Type "v" or "virtual network" in the search bar up top and click on "Virtual networks".

![image](https://github.com/user-attachments/assets/ae4c47f4-f1fb-4410-9bb0-31a6d73b2acb)

- Click "create" (just like we did with the resource group).
- Make sure it shows the resource group there, if not click the drop down and select it.
- I'll name the VN (Virtual Network) "ActiveDirectory-VNet". ***(Take note of the region and make sure any Virtual Machines we wind up creating are in that same region.)***
- Click Review + Create and then Create.
- Wait for deployment to complete.

![image](https://github.com/user-attachments/assets/c15aa17c-293e-4f9a-9275-8d231dfa3981)

- Once the VN has been created, type "v" or "virtual machine" in the search bar at the top and click on "Virtual machines"
- Click on "Create" -> Azure Virtual Machine
- Make sure the resource group is the one we created.
- I'll call this VM "DC1".
- Make sure its in the same region as the VN.

![image](https://github.com/user-attachments/assets/40d96467-cfd4-4139-a772-f6870975f944)

![image](https://github.com/user-attachments/assets/1514b29b-775b-4903-8542-831a25a7096d)

- For *Image use "Windows Server 2022 Datacenter: Azure Edition - x64 Gen2"
- For *Size use Standard_D2s_v3 - 2 vcpus, 8 GiB memory    

![image](https://github.com/user-attachments/assets/8485d598-97a2-44d8-ad8e-70f843eadbe4)

- I used username: labuser and password: Password1234
- Once you create your username and password for DC1, click "Next:Disks>" at the bottom and then "Next:Networking>" (the same button)

![image](https://github.com/user-attachments/assets/850fe8b2-27e9-4218-9be1-1d2d2f429a1d)

- Make sure in the Networking tab, it shows the Virtual network we created selected. ***(Sometimes it won't be there which means you need to wait a bit longer before being able to create the Domain Controller VM. In that case just click "home" in the top left and try again until you see the VN show up as an option.)***
- Then click on "Review + Create" -> "Create".

![image](https://github.com/user-attachments/assets/9f19b846-17c2-4885-85e8-8f07a6de5aff)

![image](https://github.com/user-attachments/assets/05fd4ba4-3035-4a4b-8d6f-1cacd4448172)

**Now we need to set the Domain Controller's Network Interface Card's (NIC) to be static instead of dynamic. Then we'll log into the DC1 VM in using Remote Desktop Connection and disable the Windows Firewall.**

- Click "Home" in the top left -> Virtual machine OR type "v" in the search box up top and click "Virtual machine"
- Click on the DC1 VM in the Virtual Machine screen within Azure.

![image](https://github.com/user-attachments/assets/ceb3c5af-d283-4016-adfd-1fd5c4f9286c)

- Click on "Network settings" in the left side panel of DC1.
- (Notice the Private IP address there underlined in green.) Click on the NIC / IP configuration underlined in red.

![image](https://github.com/user-attachments/assets/5a9bd38b-e58e-461e-9795-3b83a2d3c2c8)

- Click on "ipconfig1" towards bottom left of the IP configurations screen.
- Then, click on Static on the right side under Dynamic. (Take note of the Private IP address again here. We'll use that to test connectivity later.)
- Click "Save" on the bottom.

![image](https://github.com/user-attachments/assets/fd44ab47-1007-4c84-8e3d-0dd9837fff0d)

![image](https://github.com/user-attachments/assets/c5202f97-b96e-4fe8-82d8-8755de9a523e)

- Click "Home" in the top left -> Virtual machine OR type "v" in the search box up top and click "Virtual machine"
- Type "remote desktop connection" in the search box in the taskbar and pull up RDC
- Copy/paste the public IP address into the text box in RDC and click "Connect"

![image](https://github.com/user-attachments/assets/03bef0ff-deb1-4b9d-aa49-764348309092)

- Log into the Domain Controller using the Username and Password we created earlier when deploying DC1. (You may need to click on "More choices" on the bottom to change it from the prepopulated username.)
- (Decline any options it presents to you once logged into the Domain.) In the "Search" text box located in the bottom left on the taskbar, type "firewall" and open up Windows Defender Firewall.

![image](https://github.com/user-attachments/assets/cd3e60b3-666c-431f-a1e3-3f6c6463f188)

- Click on "Windows Defender Firewall Properties"

![image](https://github.com/user-attachments/assets/5bc94ea6-3551-4d81-a893-cee1a0932245)

- Across all three of the underlined tabs (Domain Profile, Private Profile, and Public Profile), change the "Firewall state" to Off
- Click "Apply" in the bottom right after all three have been turned off
- Then click "Ok" and close Windows Defender Firewall

![image](https://github.com/user-attachments/assets/b8d9e957-3935-4761-8f25-fa6bb2de073f)

**Next we well need to create the Client VM in Azure (Scroll up to find the steps on where to click to create a VM if you need to see those screenshots again.)**

- Minimize the DC1 VM and go back to Microsoft Azure
- Click on "Virtual machine" under recents on the home screen OR type "v" in the search box up top and click "Virtual machine"
- Click Create -> Azure Virtual Machine
- I will name it "Client1"
- Use Windows 10 Pro 22H2 for *Image
- Use 2 vcpus 8GiB Memory for *Size
- I will set Username: labuser  and  Password: Password1234
- ***Make sure it is in the same region and Virtual Network as DC1***
- Check the Licensing box
- Click "Next: Disks >" and then "Next: Networking >"

![image](https://github.com/user-attachments/assets/00c47443-fce3-47f9-a242-6d156b3c047d)

![image](https://github.com/user-attachments/assets/df49fe0e-1a71-4fd2-8972-bcd740623c31)

![image](https://github.com/user-attachments/assets/63f3018b-5d02-44fe-93f5-928adcd50a65)

- After verifying the VN is the same one we created for DC1, click "Review + Create" and then "Create"

![image](https://github.com/user-attachments/assets/43f6a90d-d6a1-44cf-92e1-c2f8d362ab2a)

**Now we need to set Client1's DNS settings to DC1's Private IP Address. Then, we will ping the Domain Controller to test the connection.**

- After Client1 has been successfully deployed, click on Client1 and go to "Network Settings"
- Click on the Network Interface/IP Configuration (client1559_z1 (primaty)/ ipconfig1 (primary)

![image](https://github.com/user-attachments/assets/0f5700f9-a6b3-4bd1-9689-717143363297)

- Click on "DNS servers" in the left side panel under the "Settings" drop down
- Change the DNS servers from "Inherit from virtual network" to "Custom"
- Type the Private IP address for DC1 in the text box (If you need to see how to find the private IP address for DC1, please scroll back up to find and review the steps.)
- Click Save

![image](https://github.com/user-attachments/assets/6948f03b-ec2d-48f5-9779-36fd221ce15e)

- Go back to the Virtual Machine screen in Azure
- Check the box next to "Client1" and restart Client1 by clicking the 3 dot menu next to "Start"

![image](https://github.com/user-attachments/assets/f5072dda-0c70-4f75-a5da-54edca879c97)

- Once the restart has completed, log into the Client1 VM using Remote Desktop. You'll be able to have multiple Remote Desktops open, so don't worry about still being logged into DC1. Simply pull up another Remote Desktop Connection from the Taskbar Search box. (If you need to see how to log into the Client1 VM using Remote Desktop, please scroll up and review the steps we took to log into DC1.)
- Type "cmd" in the search box on the taskbar and click on "Command Prompt" (Or use "Windows key + R" then type "cmd" -> hit Enter)
- Type "ping (DC1's private IP Address)" and observe the results

![image](https://github.com/user-attachments/assets/583bf8e7-45b5-40e4-a13c-fb3019bf2a5a)

![image](https://github.com/user-attachments/assets/bed00c95-0046-4f74-b200-e3876dd612d8)

**4 Replies show us that the Ping was successful!**

- Type "PowerShell" in the search box on the taskbar
- Open up PowerShell and type "ipconfig /all" and observe the DNS settings showing DC1's private IP Address

![image](https://github.com/user-attachments/assets/9206015d-11d4-423d-ae8d-cc301cfef11e)

![image](https://github.com/user-attachments/assets/52212f0a-a2e3-484e-be60-7c1248e134df)

**Now we are going to install Active Directory Domain Services.**

- Log into DC1 VM usig Remote Desktop (If you need to see how to log into the Client1 VM using Remote Desktop, please scroll up and review the steps we took to log into DC1.)
- In the Server Manager (It should already be started automatically when logging into the Domain) click on "Add roles and features"

![image](https://github.com/user-attachments/assets/128b1523-39c4-4562-9373-7e737072d6eb)

- You'll hit "Next>" 3 times 
- Check the box next to "Active Directory Domain Services" and click "Add Features"
- You'll hit "Next>" 3 more times
- Check the box next to "Restart the destination server automatically if required" and click "yes"
- Click "Install"
- Wait for installation to complete

![image](https://github.com/user-attachments/assets/5964e77d-4a6e-4976-bc54-0ac6efce4195)

![image](https://github.com/user-attachments/assets/3f78e325-7dcb-4aa9-b0b9-3d62053f8378)

![image](https://github.com/user-attachments/assets/03f861b3-f142-4464-9069-dd1cf6c0fe31)

![image](https://github.com/user-attachments/assets/f6749f5d-5961-4f9f-be91-bbb6689d3b19)

![image](https://github.com/user-attachments/assets/8e881d93-d519-4142-adbc-49505a028645)

![image](https://github.com/user-attachments/assets/fca70ada-c8fd-4bb4-9626-d6ebbc229fd3)

- Click "close" once installation completes

![image](https://github.com/user-attachments/assets/452247aa-143d-4df3-bd99-2f169542edc0)

- Click on the Flag icon with the yellow warning triangle by it
- Click "Promote this server to a domain controller

![image](https://github.com/user-attachments/assets/b65c8a7f-6d05-4b68-97c7-7a632462a465)

- Select "Add a new forest"
- Type "mydomain.com" as the Root domain name
- Click "Next>"

![image](https://github.com/user-attachments/assets/a6f24145-6ba6-4b10-8ce6-1d41767f21dc)

- Type "Password1" twice (You'll most likely never use this password)
- Click "Next>" 5 times (You may have to wait a bit on the third "Next" for the BIOS Domain name)
- Click "Install"
- After completion, the VM will be shut down

![image](https://github.com/user-attachments/assets/cfdc056b-4731-40e6-af5b-a830de05d5ba)

![image](https://github.com/user-attachments/assets/8da5d7f6-f9cf-475e-928b-7df5166b3290)

- Log back into DC1 with username: mydomain.com\labuser  and same password you've been using

![image](https://github.com/user-attachments/assets/00141521-f4f2-4b84-b889-06c71c90b9a6)

- Click the "Start" menu -> "Windows Administrative Tools" -> "Active Directory Users and Computers"

![image](https://github.com/user-attachments/assets/bbfd6105-4a40-45b4-b9f9-d67a41ca4cbe)

- Click the dro pdown next to "mydomain.com", then RIGHT click on "mydomain.com" -> select "New" -> click on "Organizational Unit"

![image](https://github.com/user-attachments/assets/44985932-5b7c-4563-9d74-98a8c7ce5bcd)

- Type "_EMPLOYEES" -> Click "OK"

![image](https://github.com/user-attachments/assets/f5c3793d-bd48-4c9a-b8cf-82a4e55fa55e)

- Right click on "mydomain.com" again -> New -> Organizational Unit
- This time name it "_ADMINS" and click "OK"

![image](https://github.com/user-attachments/assets/4b1827a7-75ec-4e3a-809e-bbf1f22c57e0)

*You should now see the Organizational Units you created in the left panel like this:

![image](https://github.com/user-attachments/assets/fd512c9b-febf-4245-803a-4628a31c6b66)

- Right click on "_ADMINS" -> New -> select "User"
- Create our Admin User by filling in the blanks. I'll create an admin user account with the name of "Jane Doe" and Username "jane_admin"
- Then click Next>

![image](https://github.com/user-attachments/assets/94212fc4-edc8-46c2-af63-7e66b70bc99c)

- To keep it simple for the sake of this tutorial, I'll use the password: "Password1234"
- **Uncheck the box next to "User must change password at next logon**

![image](https://github.com/user-attachments/assets/75a08d4f-f83c-4f80-b245-c12f3d9c1bd0)

*Now we can see Jane Doe in the _ADMINS Organizational Unit, but we still need to add her to the "Domain Admins" Security Group.

![image](https://github.com/user-attachments/assets/839b87a0-39b6-46c2-93d3-197b7e7d17c7)

- Right Click on Jane Doe -> Add to group
- Type "domain admins" in the text box and click on "Check Names" -> Click "OK"

![image](https://github.com/user-attachments/assets/860c12e8-2a94-4f32-b917-8d941f18801d)

![image](https://github.com/user-attachments/assets/da9f5a26-3415-449e-bab4-29034be7e25b)

- Log out of DC1 either form the start button or by typing "CTRL + R" -> "logoff" -> Enter
- Log back in with username: "mydomain.com\jane_admin" (We will use this as our admin account going forward)

![image](https://github.com/user-attachments/assets/f1010c5e-b85f-4fbc-8f35-2bcfbde01903)

**Now we need to join our Client1 VM to the domain (mydomain.com)**

- Login to Client1 as "labuser"
- Once logged in, right click on Start/Windows Icon -> Click on "System"
- Click "Advanced System Settings" (You could also just click on "Rename this PC (advanced)")
- Click on the "Computer Name" on top left -> Click "Change"

![image](https://github.com/user-attachments/assets/5ad1e453-14ac-423f-8ff8-9a8b343689c2)

![image](https://github.com/user-attachments/assets/905f789b-ba83-4963-90b3-d3497aa0ed01)

![image](https://github.com/user-attachments/assets/88f9fa66-d7a3-49db-8bb1-2eca220d1838)

- Under "Member of", select "Domain" and type "mydomain.com" in the text box
- Click "OK"
- Use the mydomain.com\jane_admin login info to join
- Move the System window or close it to see a pop up box stating you have successfully joined and click ok -> close out of the advanced settings
- Allow it to restart the VM

![image](https://github.com/user-attachments/assets/58f697d8-ef21-4ddd-a25f-1954ba624183)

![image](https://github.com/user-attachments/assets/39343e1b-47a0-4a0b-a1f0-482c086be93e)

![image](https://github.com/user-attachments/assets/796f5bc7-c30c-40c1-945a-9ee6278a7c4a)

**When you log back into Client1, it will be joined to the Domain**

- In the Domain Controller (DC1 VM), pull "Active Directory Users and Computers" back up
- Under "mydomain.com" in the left panel, click on the "Computers" organizational unit and verify that "Client1" shows up

![image](https://github.com/user-attachments/assets/88a13840-4137-4685-bb30-70d693445a1a)

- Right click on "mydomain.com" to create a new organizational unit called "_CLIENTS" (If you need to see how to do this, please scroll back up to where we created _EMPLOYEES and _ADMINS)
- Click and drag "Client1" from "Computers" and place it into "_CLIENTS"
- Click "Yes" when it asks if you're sure

![image](https://github.com/user-attachments/assets/22d5376d-cc98-45f4-9ee9-ee68895f4821)

![image](https://github.com/user-attachments/assets/800be9a6-5bc4-4478-a522-94b591aa2f5d)

**Now we will allow "domain users" access to remote desktop and create plenty of users/employees to practice with in Active Directory**

- Log into Client1 as mydomain.com\jane_admin
- Right click on the start button/Windows icon -> click "System"
- Click "Remote Desktop"
- Under "User accounts" -> click "Select users that can remotely access this PC"
- Click "Add"
- Type "domain users" and click "Check Names"
- Click "OK" and you should see MYDOMAIN\Domain Uswers added

![image](https://github.com/user-attachments/assets/6bbd3db7-af88-45fe-8ab5-8413bc5a6e6f)

![image](https://github.com/user-attachments/assets/5a719506-be45-481c-b8c7-8f2dbfc1ab09)

![image](https://github.com/user-attachments/assets/b1b10de0-3f3c-4b8b-8b98-aea45ce65cb1)

![image](https://github.com/user-attachments/assets/630404d6-6b86-420e-ad3f-0190071f4582)

![image](https://github.com/user-attachments/assets/54ec4c8a-4cad-481c-b3c0-3feab065cfeb)

- Log into DC1 as mydomain.com\jane_admin if you aren't already
- In the search box in the task bar type "powershell ise"
- Right click on "Windows PowerShell ISE" -> click "Run as Administrator"
- Click on the page symbol (New Script) in the top left
- Copy and paste <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">This Script </a> into the text area
- Click the green arrow/triangle pointing right (Run Script) to create a ton of users to work with in Active Directory
***This will take a bit. It will create 10,000 or so users so feel free to type "ctrl + C" or click on the red square (stop script) to stop it after a bit if you don't want to wait for it to finish. We don't need all 10,000 users so no worries if you stop it early.***

![image](https://github.com/user-attachments/assets/8a902c95-39dd-4e4b-a1b2-7e1fa35afc73)

![image](https://github.com/user-attachments/assets/caf4b1a8-7e0a-470b-9ae3-bff2a94fffbd)

![image](https://github.com/user-attachments/assets/064577c3-bcda-4845-a178-f549da594c42)

![image](https://github.com/user-attachments/assets/e8444bc3-0dd6-435b-b037-fa77b63e335b)

***The script shows that the password for the users we just created is "Password1". We'll need to keep that in mind for when we log in as one of the users.***

- Back in DC1, in Active Directory, click on the "_EMPLOYEES" organizational unit and observe the newly created users
- Attempt to log into Client1 with one of the accounts **(Remember to use "mydomain.com\user" and the password from the script (Password1)**

![image](https://github.com/user-attachments/assets/166ac5eb-ddb3-43b8-86e5-42a317638ca6)

![image](https://github.com/user-attachments/assets/4a2e988c-efe2-4f70-93ba-b8a6088340ff)

*And that completes this tutorial, but it doesn't end here! The next tutorial will walk you through and demonstrate how to implement group policy. If you are ready to hang it up for good, I highly reccomend deleting the resource groups in Microsoft Azure to save yourself some money, or at least stop the Virtual Machines if you plan to take a break. If you'd like to learn how to create and deploy Password Lockout Policies and Company Wallpaper, or even just want some exposure to Active Directory Group Policy in general, you are welcome to join me in the next one. See you there! : )*

<p>

</p>
<br />
