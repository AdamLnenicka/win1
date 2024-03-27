**Installation of Active Directory Domain Services (AD DS) Role on Windows Server 2019:**

1. Log in to the Windows Server 2019 using an account with administrative privileges.
2. Open Server Manager by clicking on the "Server Manager" icon in the taskbar or by running "ServerManager.exe" from the Start menu.
3. In Server Manager, click on "Manage" in the top right corner and select "Add Roles and Features."
4. Follow the wizard to add roles and features, selecting "Active Directory Domain Services" when prompted for roles.
5. Proceed with the installation, confirming all default options. A server restart will be required upon completion of the installation.

**Promotion to Domain Controller and Selecting Forest:**

6. After the server restart, log in to the Windows Server 2019 using an account with administrative privileges.
7. Open Server Manager by clicking on the "Server Manager" icon in the taskbar or by running "ServerManager.exe" from the Start menu.
8. In Server Manager, click on "Manage" in the top right corner and select "Add Roles and Features."
9. Follow the wizard, and when prompted for deployment configuration, select "Add a new forest."
10. Enter the desired domain name, such as "web.inc" or "webinc.local," according to your requirements.
11. Complete the wizard and the installation process. A server restart will be required upon completion.

**Configuration of DHCP Server:**

12. After the server restart, configure the DHCP service.
13. Open Server Manager and click on "Tools" in the top menu.
14. Select "DHCP" from the tools menu.
15. In DHCP Manager, right-click on the server name and select "New Scope."
16. Follow the wizard to create a new DHCP scope, setting parameters such as IP address range, default gateway, DNS servers, and others as needed.
17. After configuring the new DHCP scope, wait for a few moments for automatic IP address assignment to clients. If clients do not receive IP addresses, troubleshoot network connectivity issues, including checking if ping is successful between client and server.
18. If necessary, restart the DHCP service or the entire server to ensure proper functioning.

**Joining a Desktop Operating System to the Domain:**

19. Log in to the Windows 10 desktop computer using an account with administrative privileges.
20. Open "Control Panel" and click on "System and Security."
21. Click on "System" and then on "Change settings" in the "Computer name, domain, and workgroup settings" section.
22. In the "System Properties" window, navigate to the "Computer Name" tab and click on the "Change" button.
23. Select the "Domain" option and enter the domain name chosen during the AD DS installation.
24. After entering the domain name, click the "OK" button, and enter credentials with permissions to add the computer to the domain.
25. After successfully joining the domain, restart the computer to complete the process.