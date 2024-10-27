<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell
- [Script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) (Generate names and users)

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Connect Client to Domain Controller
- Install Active Directory
- Create User Accounts in Active Directory
- Connect Client-1 to our Domain
- Create additional users and log into one

<h2>Setup Resources in Azure</h2>

<p>
<img src="https://i.imgur.com/8H3ABIK.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Create a Resource Group: Start by creating a new Resource Group to organize and manage your Azure resources. Choose an appropriate name for the group, such as "AD-Lab"

2. Create the Domain Controller VM (DC-1):
   - Specify the name of the VM such as "DC-1" during the creation process.
   - Choose "Windows Server 2022" as the operating system for the VM.
   - Ensure that you select the Resource Group and ensure that the Virtual Network is created.
   - After the VM is provisioned, locate the Network Interface Card (NIC) associated with the Domain Controller.
   - Set the NIC's Private IP address to a static value. This will provide consistent connectivity for the Domain Controller.

3. Create the Client VM (Client-1):
   - Create a new VM named "Client-1."
   - Select "Windows 10" as the operating system.
   - Make sure to choose the same Resource Group and Virtual Network used for the Domain Controller.

</p>
<br />

<h2>Connect Client to Domain Controller</h2>

<p>
<img src="https://i.imgur.com/SEdlbha.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Remote Desktop into Client-1: Use Remote Desktop to connect to the Client-1 VM. Ensure that you have the necessary login credentials and access rights to establish the remote connection.

2. Ping DC-1's Private IP address: Open the Command Prompt or PowerShell on Client-1 and execute the command "ping <DC-1's Private IP address> -t" (replace "<DC-1's Private IP address>" with the actual IP address of DC-1). This will initiate a continuous ping to the Domain Controller.

3. Observe timeout: Initially, you will notice that the ping requests are timing out, indicating a lack of connectivity between the two VMs.

4. Enable ICMPv4 in the Local Windows Firewall (DC-1):
   - Remote Desktop into the DC-1 VM using the appropriate credentials.
   - Access the Windows Firewall settings.
   - Locate the rule for ICMPv4 (Internet Control Message Protocol version 4).
   - Enable the rule to allow ICMPv4 traffic.

5. Wait for a few moments: After enabling ICMPv4, wait for a brief period to allow the changes to take effect.
</p>
<br />

  <h2>Install Active Directory</h2>
  
<p>
<img src="https://i.imgur.com/8l3r5un.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Log in to DC-1: Use Remote Desktop or any other preferred method to log in to the DC-1 VM using the appropriate credentials.

2. Install Active Directory Domain Services:
   - Open the Server Manager application.
   - Click on "Add roles and features" to launch the wizard.
   - Follow the prompts and select the appropriate options until you reach the "Server Roles" section.
   - Locate and select "Active Directory Domain Services."
   - Proceed with the installation, accepting the default settings unless specific customization is required.
   - Complete the installation process.

3. Set up a new forest: Once AD DS is installed, proceed with setting up a new forest with the domain name "mydomain.com":
   - Open the Server Manager application.
   - Click on the "Promote this server to a domain controller" option in the notification area.
   - In the Active Directory Domain Services Configuration Wizard, select "Add a new forest" and enter "mydomain.com" as the root domain name.
   - Configure the forest and domain functional levels based on your requirements.
   - Set a Directory Services Restore Mode (DSRM) password.
   - Proceed with the remaining steps, reviewing and confirming the settings.
   - Once the configuration is complete, AD DS will be installed, and the new forest "mydomain.com" will be established.

4. Restart and log back in as "mydomain.com\labuser":
   - Restart the DC-1 VM to apply the changes.
   - After the restart, log in to DC-1 using the username "mydomain.com\labuser" and the corresponding password.

</p>
<br />
<h2>Create User Accounts in Active Directory</h2>

<p>
<img src="https://i.imgur.com/gD6pTwE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Log in to DC-1 as "mydomain.com\labuser": Use Remote Desktop or any other preferred method to log in to the DC-1 VM as the user "mydomain.com\labuser" (the previously mentioned username).

2. Open Active Directory Users and Computers: Launch the Active Directory Users and Computers console, which is typically found in the Administrative Tools or by searching for "Active Directory Users and Computers" in the Start menu.

3. Create Organizational Units:
   - Right-click on the domain name ("mydomain.com") and select "New" > "Organizational Unit."
   - Enter "_EMPLOYEES" as the name for the first OU and click "OK."
   - Repeat the above steps to create a second OU named "_ADMINS."

4. Create a new employee user account:
   - Right-click on the "_EMPLOYEES" OU and select "New" > "User."
   - Provide the necessary details for the employee account. In this example, use "Jane Doe" as the Full Name and "jane_admin" as the username.
   - Set a password for the user account and configure any additional required settings.
   - Complete the user creation process.

5. Add "jane_admin" to the Domain Admins Security Group:
   - Locate the "Domain Admins" Security Group within the "Builtin" container.
   - Right-click on the "Domain Admins" group and select "Properties."
   - In the Properties window, click on the "Members" tab.
   - Click on the "Add" button to add a new member.
   - Enter "jane_admin" in the "Enter the object names to select" field and click "OK."
   - Verify that "jane_admin" is now listed as a member of the "Domain Admins" group and click "OK" to close the Properties window.

6. Log out and log back in as Jane Doe:
   - Log out of the DC-1 VM session as "mydomain.com\labuser."
   - Log back in using the username "mydomain.com\jane_admin" and the corresponding password for the "jane_admin" user account.
</p>
<br />

<h2>Connect Client-1 to our Domain</h2>
<p>
<img src="https://i.imgur.com/Jyw1QM0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Access Microsoft Azure Portal

2. Locate Client-1 and modify DNS settings:
   - Navigate to the Virtual Machines section of the Azure Portal.
   - Locate and select the Client-1 VM.
   - In the Client-1 VM's settings, navigate to the Networking section.
   - Find the DNS servers configuration and set it to the Private IP address of the DC-1 VM.
   - Save the changes.

3. Restart Client-1: Restart the Client-1 VM to apply the new DNS settings.

4. Remote Desktop back into Client-1 and Domain Controller:
   - Use Remote Desktop to connect to Client-1 and the Domain Controller separately.
   - Ensure you have the necessary credentials to establish the remote connections.

5. Verify Client-1's appearance in Active Directory:
   - In the Active Directory Users and Computers console on the Domain Controller, navigate to the "Computers" container in the root of the domain.
   - Confirm that the Client-1 computer object is visible within the "Computers" container. This indicates successful integration with the Active Directory domain.

6. Create a new OU named "_CLIENTS" and move Client-1 into it:
   - Right-click on the domain name in the Active Directory Users and Computers console.
   - Select "New" > "Organizational Unit" and provide the name "_CLIENTS" for the new OU.
   - Locate the Client-1 computer object within the "Computers" container.
   - Right-click on the Client-1 computer object and select "Move."
   - Choose the newly created "_CLIENTS" OU as the destination for moving the Client-1 computer object.
   - Confirm the move operation.
</p>
<br />
<h2>Connect Client-1 to our Domain</h2>

<p>
<img src="https://i.imgur.com/jxZ5eKR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
1. Log in to Client-1 as "jane_admin":
   - Use Remote Desktop or any preferred method to log in to the Client-1 VM as "jane_admin" with the username "mydomain.com/jane_admin" and the corresponding password.

2. Allow "domain users" access to Remote Desktop:
   - Open the "System" settings on Client-1. You can access it by right-clicking on the "This PC" or "My Computer" icon and selecting "Properties."
   - In the System window, click on the "Remote settings" link on the left-hand side.
   - In the Remote tab, under Remote Desktop, click on the "Select users" button.
   - Click on the "Add" button to add users who should have Remote Desktop access.
   - Type "domain users" and click on the "Check Names" button to validate the entry.
   - Click on "OK" to add "domain users" as a group with Remote Desktop access.

3. Log in as a non-administrative user:
   - Log out of the Client-1 session as "jane_admin" (mydomain.com/jane_admin).
   - Log in to Client-1 using the credentials of a normal, non-administrative user within the domain.
</p>
<br />
<h2>Create additional users and login to one</h2>
<p>
<img src="https://i.imgur.com/2DRjuD8.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

1. Log in to DC-1 as "jane_admin":

2. Open PowerShell ISE as administrator:

3. Create a new file and paste the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1):
   - In PowerShell ISE, click on "File" and select "New" to create a new script file.
   - Paste the contents of the script into the newly created file.

4. Modify the script:
   - Within the script, locate the part where the number of accounts is defined.
   - Update the number of accounts to 100 or the desired value.

5. Run the script and observe the account creation:
   - Monitor the output in the PowerShell console as the script creates the user accounts. Take note of any important information displayed during the process.

6. Select and obtain the username and password:
   - Once the user accounts are created, review the output in the PowerShell console.
   - Locate the username and password information for one of the created accounts.
   - Make a note of this username and password for later use.

7. Log in to Client-1 using the obtained credentials:
   - Launch Remote Desktop and connect to the Client-1 VM.
   - Use the username and password obtained from the script to log in as the specific user.
</p>

