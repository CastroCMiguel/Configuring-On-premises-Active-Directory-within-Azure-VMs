<p align="center">
<img src="https://i.imgur.com/UEoyF27.png" alt="1"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>

This lab offers a comprehensive guide on deploying on-premises Active Directory within Azure Virtual Machines, with a focus on simplifying the process and making it user-friendly. The primary objectives include setting up Active Directory Domain Services (AD DS) on a Windows Server 2022 machine and connecting a Windows 10 client to the domain. Additionally, the tutorial covers essential tasks such as creating user accounts, enabling remote desktop access, and other related configurations. Visual guidance is provided through a video, and additional notes and screenshots are available to highlight key aspects of the material.

<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure](https://www.youtube.com/watch?v=DOF0VgkVoD0)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 

<h2>Deployment and Configuration Steps</h2>

- Here are the steps to set up the Azure resources:

1. **Create a Resource Group and Virtual Network (VNet) in Azure:**

   - Log in to the Azure Portal (https://portal.azure.com/).
   - In the left sidebar, click on "Resource groups."
   - Click the "+ Add" button to create a new resource group.
   - Give the resource group a name and select the appropriate region.
   - Click "Review + create" and then "Create" to create the resource group.
   - Once the resource group is created, go to "Resource groups" and select the one you just created.
   - Inside the resource group, click "+ Add" to create a new Virtual Network (VNet).
   - Provide a name for the VNet, select the region, and configure the other settings as needed.
   - Click "Review + create" and then "Create" to create the VNet.

2. **Create a Windows Server 2022 Virtual Machine (VM) named "DC-1":**

   - In the same resource group where you created the VNet, click "+ Add" to create a new virtual machine.
   - Choose the "Windows Server 2022 Datacenter" image.
   - Configure the VM settings, including name ("DC-1"), size, authentication, and other options.
   - When configuring the networking settings, connect the VM to the VNet you created earlier.
   - Ensure that you assign a static private IP address to the VM during the networking setup.
   - Complete the rest of the VM creation wizard, review the settings, and click "Create" to provision the VM.

3. **Create a Windows 10 VM named "Client-1" in the same Resource Group and VNet:**

   - Similarly, in the same resource group and VNet, click "+ Add" to create another virtual machine.
   - Choose the "Windows 10" image.
   - Configure the VM settings, including name ("Client-1"), size, authentication, and other options.
   - Connect the VM to the same VNet as the previous VM.
   - Complete the VM creation wizard, review the settings, and click "Create" to provision the Windows 10 VM.

Now you should have a Resource Group, a Virtual Network, a Windows Server 2022 VM named "DC-1," and a Windows 10 VM named "Client-1" all set up within Azure, ready for further configuration and domain setup.

<p align="center">
<img src="https://i.imgur.com/30y7u51.png" alt="1"/>
</p>

<h2></h2>

- To ensure connectivity between the Windows 10 client "Client-1" and the Windows Server 2022 Domain Controller "DC-1" in Azure, follow these steps:

1. **Connect to Client-1 using Remote Desktop:**

   - In the Azure Portal, locate your "Client-1" virtual machine.
   - Under the "Settings" section of "Client-1," click on "Connect" to download the Remote Desktop Protocol (RDP) file.
   - Open the RDP file and connect to "Client-1" using the provided credentials.

2. **Ping DC-1's Private IP Address from Client-1:**

   - Once you are connected to "Client-1" via Remote Desktop, open the Command Prompt.

   - Run the following command to ping the private IP address of "DC-1" (replace `DC-1-Private-IP` with the actual private IP address of DC-1):

     ```
     ping DC-1-Private-IP
     ```

   - If the ping is successful, you should see replies from "DC-1." If it fails, proceed to the next step.

<p align="center">
<img src="https://i.imgur.com/j2Cyr4X.png" alt="1"/>
</p>

3. **Enable ICMPv4 in the Local Windows Firewall on DC-1:**

   - On "DC-1," which is the Windows Server 2022 machine, you might need to enable ICMPv4 (ping) in the Windows Firewall to allow incoming ping requests. Here's how:

     - Open the "Server Manager" on "DC-1."
     - Click on "Windows Defender Firewall with Advanced Security" in the left pane.

     - In the "Windows Defender Firewall with Advanced Security" window, click on "Inbound Rules."

     - Locate the rule named "File and Printer Sharing (Echo Request - ICMPv4-In)" in the list.

     - Right-click on the rule and select "Enable Rule" to enable ICMPv4 (ping) requests.
    
<p align="center">
<img src="https://i.imgur.com/UkNLbgM.png" alt="1"/>
</p>

4. **Verify Ping from Client-1 to DC-1:**

   - Go back to your "Client-1" machine via Remote Desktop and try to ping "DC-1" again using the same command as in step 2:

     ```
     ping DC-1-Private-IP
     ```

   - If you've successfully enabled ICMPv4 in the Windows Firewall on "DC-1," the ping should now succeed, and you should receive replies from "DC-1."

With ICMPv4 enabled in the Windows Firewall on "DC-1," you should now have successful connectivity between "Client-1" and "DC-1" within your Azure environment.

<p align="center">
<img src="https://i.imgur.com/sq2s9tG.png" alt="1"/>
</p>

<h2></h2>

- To install Active Directory Domain Services (AD DS) on "DC-1" and promote it as a domain controller with a new forest, follow these steps:

1. **Log in to DC-1:**

   - Remote Desktop into "DC-1" using the credentials you configured during the VM setup.

2. **Install Active Directory Domain Services (AD DS):**

   - Once logged in, open the "Server Manager" by clicking on the Windows icon in the taskbar and selecting "Server Manager."

   - In the "Server Manager" dashboard, click on "Manage" in the top right corner and select "Add Roles and Features."

   - Follow the "Add Roles and Features Wizard":
     - On the "Before you begin" page, click "Next."
     - On the "Select installation type" page, choose "Role-based or feature-based installation" and click "Next."
     - On the "Select destination server" page, select "DC-1" as the destination server and click "Next."
     - On the "Select server roles" page, select "Active Directory Domain Services." A dialog box will appear, asking to add required features, click "Add Features" to add them, and then click "Next."

<p align="center">
<img src="https://i.imgur.com/iP9o8uD.png" alt="1"/>
</p>

   - On the "Select features" page, click "Next."

   - On the "Active Directory Domain Services" page, read the information, and click "Next."

   - On the "Confirm installation selections" page, review your selections and click "Install."

   - Wait for the installation to complete. Once it's done, click "Promote this server to a domain controller."

3. **Promote DC-1 as a Domain Controller:**

   - In the "Active Directory Domain Services Configuration Wizard," choose "Add a new forest" since you're setting up a new forest with a new domain name.

   - Enter your Root domain name, e.g., "mydomain.com," in the "Root domain name" field.

<p align="center">
<img src="https://i.imgur.com/fhAri4w.png" alt="1"/>
</p>

   - Configure the forest and domain functional levels as per your requirements. The default values are often suitable.

   - Set a Directory Services Restore Mode (DSRM) password and confirm it. This password is used to recover Active Directory in case of emergencies.

   - Click "Next" and review the NetBIOS domain name that's generated based on your root domain name. You can customize it if needed.

   - Choose the paths for the Active Directory database, log files, and SYSVOL, or keep the default locations.

   - Review the settings on the "Review Options" page and click "Next."

   - The wizard will run a prerequisite check. If everything is green, click "Install" to begin the installation.

4. **Complete the Promotion:**

   - After the installation completes, "DC-1" will automatically restart.

5. **Verify Domain Controller Setup:**

   - Log back in to "DC-1" once it has restarted.

   - Open "Server Manager" and go to "Tools" > "Active Directory Users and Computers." You should now see your new domain and forest.

You have now successfully installed Active Directory Domain Services on "DC-1" and promoted it as a domain controller with a new forest and domain name. Your new domain, e.g., "mydomain.com," is ready for further configuration and user management.

<h2></h2>

- To create an Admin and a Normal User account in Active Directory on "DC-1," follow these steps:

1. **Open Active Directory Users and Computers (ADUC):**

   - Log in to "DC-1."

   - Open the "Server Manager."

   - Click on "Tools" in the top-right corner and select "Active Directory Users and Computers."

2. **Create Organizational Units (OUs):**

   - In the ADUC console, right-click on the "mydomain.com" (or your domain name) and choose "New" > "Organizational Unit."

   - Create two OUs: "_EMPLOYEES" and "_ADMINS."

3. **Create an Admin Account (Jane Doe):**

   - In the "_ADMINS" OU, right-click and choose "New" > "User."

   - Follow the wizard to create the user account for Jane Doe with the following details:
     - First Name: Jane
     - Last Name: Doe
     - User Logon Name: jane_admin (this will create the username "jane_admin@mydomain.com")
     - Set a password for the account.

   - Complete the wizard to create the user account.

4. **Add Jane to the "Domain Admins" Group:**

   - In ADUC, navigate to "Users," and find the "jane_admin" user you just created.

   - Right-click on "jane_admin" and select "Properties."

   - Go to the "Member Of" tab and click "Add."

   - Type "Domain Admins" and click "Check Names" to validate it.

   - Click "OK" to add Jane to the "Domain Admins" Security Group.

<p align="center">
<img src="https://i.imgur.com/Jr9EflK.png" alt="1"/>
</p>

5. **Log Out and Log In as "jane_admin":**

   - Log out of the current session on "DC-1."

   - Log back in as "mydomain.com\jane_admin" using the password you set during account creation.

Now, you've created an Admin account (Jane Doe) and added her to the "Domain Admins" Security Group. You can perform administrative tasks on "DC-1" using this account. Additionally, you've created Organizational Units (_EMPLOYEES and _ADMINS) to organize your Active Directory structure.

<h2></h2>

- To join "Client-1" to the domain, set its DNS settings to the private IP address of "DC-1," move it to a new OU named "_CLIENTS," and verify its presence in Active Directory Users and Computers (ADUC) on "DC-1," follow these steps:

1. **Set DNS Settings on Client-1:**

   - Log in to "Client-1" as the local administrator.

   - Open the "Control Panel."

   - Go to "Network and Sharing Center."

   - Click on "Change adapter settings" on the left side.

   - Right-click on the active network connection (e.g., Ethernet or Wi-Fi) and select "Properties."

   - In the "Properties" window, scroll down and select "Internet Protocol Version 4 (TCP/IPv4)."

   - Click the "Properties" button.

   - In the "Internet Protocol Version 4 (TCP/IPv4) Properties" window, select the option "Use the following DNS server addresses."

   - Set the "Preferred DNS server" to the private IP address of "DC-1," which is the DNS server for your domain.

   - Leave the "Alternate DNS server" blank.

   - Click "OK" to save the DNS settings.
  
<p align="center">
<img src="https://i.imgur.com/eCHotNn.png" alt="1"/>
</p>

2. **Join "Client-1" to the Domain:**

   - After setting the DNS server to "DC-1," you can now join "Client-1" to the domain:

     - Right-click on "This PC" or "Computer" (depending on your Windows version) and select "Properties."

     - In the System Properties window, click on the "Change settings" link next to "Computer name, domain, and workgroup settings."

     - In the Computer Name/Domain Changes window, select the "Domain" option.

     - Enter the name of your domain (e.g., mydomain.com) and click "OK."

     - You'll be prompted to enter credentials. Provide the username and password of an account with permission to join computers to the domain. This could be an administrator account, such as "jane_admin."

     - After successfully joining the domain, you'll be prompted to restart "Client-1." Click "OK" to restart the computer.
    
<p align="center">
<img src="https://i.imgur.com/RTgqN8K.png" alt="1"/>
</p>

3. **Verify "Client-1" in ADUC on DC-1:**

   - Log in to "DC-1."

   - Open "Active Directory Users and Computers" (ADUC) as an administrator.

   - Navigate to the "Computers" container. You should see "Client-1" listed there, indicating that it has successfully joined the domain.

4. **Create and Move "Client-1" to a New OU:**

   - In ADUC on "DC-1," right-click on your domain name (e.g., mydomain.com) and select "New" > "Organizational Unit."

   - Name the new OU as "_CLIENTS" and click "OK."

   - In ADUC, locate "Client-1" under the "Computers" container.

   - Right-click on "Client-1" and select "Move."

   - Browse to and select the "_CLIENTS" OU you just created and click "OK" to move "Client-1" to the new OU.

   - "Client-1" is now within the "_CLIENTS" OU in Active Directory.

<p align="center">
<img src="https://i.imgur.com/D9i1jCL.png" alt="1"/>
</p>

With these steps, you've successfully joined "Client-1" to the domain, placed it in the "_CLIENTS" OU, and verified its presence in ADUC on "DC-1."

<h2></h2>

- To set up Remote Desktop for non-administrative users on "Client-1," follow these steps:

1. **Log in to "Client-1" as "mydomain.com\jane_admin":**

   - Ensure you are logged in as "mydomain.com\jane_admin," which is an account with administrative privileges on "Client-1."

2. **Open System Properties:**

   - Right-click on "This PC" or "Computer" (depending on your Windows version) and select "Properties."

   - In the System Properties window, click on the "Remote" tab.

3. **Allow "Domain Users" Access to Remote Desktop:**

   - In the Remote tab of System Properties, you'll see the "Remote Desktop" section.

   - Click on the "Select Users" button.

   - In the Remote Desktop Users dialog, click the "Add" button.

   - In the Select Users or Groups dialog, type "Domain Users" and click "Check Names" to validate it.

   - Click "OK" to add "Domain Users."

   - You will now see "Domain Users" listed in the Remote Desktop Users dialog.

   - Click "OK" to close the Remote Desktop Users dialog.

   - Click "Apply" and then "OK" to apply the changes in the System Properties window.

<p align="center">
<img src="https://i.imgur.com/J51X2oY.png" alt="1"/>
</p>

4. **Non-Administrative Users Can Now Use Remote Desktop:**

   - With these settings, non-administrative users within your domain can now use Remote Desktop to connect to "Client-1."

   - They should use their own domain user credentials when connecting.

This configuration allows non-administrative users in the domain to access "Client-1" via Remote Desktop. Make sure that the non-administrative users have the necessary permissions to log in remotely to ensure a successful connection.

<h2></h2>

- To create additional user accounts, test login, and verify their placement in the appropriate OU within Active Directory Users and Computers (ADUC), follow these steps:

1. **Log in to "DC-1" as "mydomain.com\jane_admin":**

   - Ensure you are logged in as "mydomain.com\jane_admin," which is an account with administrative privileges on "DC-1."

2. **Open PowerShell ISE as an Administrator:**

   - In the Windows Start menu, search for "PowerShell ISE."

   - Right-click on "Windows PowerShell ISE" and select "Run as administrator" to open it with elevated privileges.

3. **Run the Provided PowerShell Script:**

   - In PowerShell ISE, open the provided PowerShell <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">script</a> that creates additional user accounts.

   - Execute the script. It will create the specified user accounts within Active Directory.

<p align="center">
<img src="https://i.imgur.com/rDJ6Aqq.png" alt="1"/>
</p>

4. **Verify User Accounts in ADUC:**

   - Open "Active Directory Users and Computers" (ADUC) on "DC-1."

   - Navigate to the appropriate Organizational Unit (OU) where the user accounts were supposed to be created (e.g., "_EMPLOYEES").

   - Verify that the newly created user accounts are listed within the correct OU.

<p align="center">
<img src="https://i.imgur.com/17nSQJD.png" alt="1"/>
</p>

5. **Test Login to "Client-1" with a Newly Created User Account:**

   - Attempt to log in to "Client-1" using one of the newly created user accounts. Ensure you use the following format for the username: "mydomain.com\username."

   - Test login for one or more of the newly created user accounts to confirm that they can access "Client-1."
   - 
<p align="center">
<img src="https://i.imgur.com/LDL10t6.png" alt="1"/>
</p>

By following these steps, you will create additional user accounts, verify their placement in the appropriate OU within ADUC, and test login functionality on "Client-1." This ensures that your on-premises Active Directory within Azure Virtual Machines is configured correctly for user and resource management.

<h2>Summary:</h2>

This project successfully involved the setup and configuration of on-premises Active Directory within Azure Virtual Machines. The key steps included:

1. Creation of a Windows Server 2022 VM as the domain controller ("DC-1").
2. Creation of a Windows 10 VM as the client ("Client-1").
3. Configuration of network settings, including a virtual network and resource group in Azure.
4. Ensuring connectivity between the client and domain controller.
5. Installation of Active Directory Domain Services (AD DS) on "DC-1."
6. Creation of user accounts, including administrative and non-administrative users.
7. Joining "Client-1" to the domain and setting up Remote Desktop for non-administrative users.
8. Verification of user account placement in the appropriate Organizational Units (OUs) within Active Directory Users and Computers (ADUC).
9. Testing user logins on "Client-1" to confirm domain access and functionality.

This comprehensive project established a functional on-premises Active Directory environment in Azure, enabling centralized user and resource management, authentication, and access control. It demonstrates the successful deployment of domain services within a virtualized cloud environment.
