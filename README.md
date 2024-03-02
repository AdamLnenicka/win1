**Project Documentation: Implementation of IT Infrastructure for Web inc.**

**1. Introduction:**
   Web inc. is a company with 50 employees distributed across various departments including sales, accounting, IT, production, and management. This project aims to implement an IT infrastructure to streamline operations and improve efficiency within the organization. The implementation includes setting up domain services, user management, security configurations, web hosting, and network installation capabilities.

**2. Infrastructure Setup:**
   - **Server Installation:** 
     - One virtual machine (VM) running Windows Server 2019.
     - Role of Active Directory Domain Services (AD DS) added.
   - **Client Machine:**
     - One virtual machine running Windows 10 for accounting purposes.

**3. Domain Configuration:**
   - The Windows Server hosts the domain controller.
   - Desktop OS joined to the domain with a suitable domain name.

**4. Organizational Unit (OU) and User Management:**
   - OU created for each department within the domain.
   - EndpointAdmins group created to manage all computers within the domain, except the domain controller.
   - Accounts created for department administrators and accountants.

**5. Remote Desktop Protocol (RDP) Access:**
   - Configuration applied to all machines in the accounting department to allow RDP access for accountants.
   - The configuration is applied globally to all existing and future machines in the accounting department.

**6. Web Hosting:**
   - Public-facing web pages created using Internet Information Services (IIS).
   - Both HTTP and HTTPS protocols enabled for communication.
   - Self-signed certificate used for HTTPS communication.
   - Web pages accessible using the www prefix before the domain name.

**7. Network Installation Service:**
   - Service set up on the server to facilitate Windows installation over the network.
   - Configuration ensures that the installer is only offered to known machines.
   - Added machines automatically start the installation process without user intervention.
