!make sure network connection between client and server is working, might need to configure ping rules, check if ping is successfull!

Installation of the AD DS role on a Windows Server 2019:

1. Log in to the Windows Server 2019 using an account with administrative privileges.
2. Open Server Manager by clicking on the "Server Manager" icon in the taskbar or by running "ServerManager.exe" from the Start menu.
3. In Server Manager, click on "Manage" in the top right corner and select "Add Roles and Features."
4. Follow the wizard to add roles and features, selecting "Active Directory Domain Services" when prompted for roles.
5. Proceed with the installation and confirm all default options. A server restart will be required upon completion of the installation.

Joining a desktop operating system to the domain:

1. Log in to the Windows 10 desktop computer using an account with administrative privileges.
2. Open "Control Panel" and click on "System and Security."
3. Click on "System" and then on "Change settings" in the "Computer name, domain, and workgroup settings" section.
4. In the "System Properties" window, navigate to the "Computer Name" tab and click on the "Change" button.
5. Select the "Domain" option and enter the domain name you chose during the AD DS installation.
6. After entering the domain name, click the "OK" button, and you will be prompted to enter credentials with permissions to add the computer to the domain.
7. After successfully joining the domain, you will be prompted to restart the computer. Restart the computer to complete the process.