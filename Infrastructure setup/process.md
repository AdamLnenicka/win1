Windows Editions:
- Home: This edition lacks some security features, does not support a wide range of management and deployment techniques. For instance, machines with this system cannot be enrolled in Active Directory (AD) services, do not support Remote Desktop, etc.
- Pro: Deployment in corporate networks emphasizing fundamental security technologies, centralized management, and remote deployment support.
- Enterprise/Education: Includes all features found in the Pro edition, with additional implementations such as DirectAccess, AppLocker, virtualization platforms App-V, UE-V, credential guard, device guard, Windows To Go support, and BranchCache support.
- Windows Server 2016 Standard: One purchased license allows coverage for 2 virtual machines and one Hyper-V host.
- Windows Server 2016 Datacenter: One purchased license covers an unlimited number of virtual machines, but always only one Hyper-V host.

Autoinstallation:
- To generate an unattend file, use the website https://www.windowsafg.com/win10x86_x64.html.
- Upload it to the root directory of the installation media.
- It's a part of the Windows System Image Manager.

Creating Custom WinPE:
- To create custom WinPE, you need to have Windows ADK + WinPE additions installed.
- ADK stands for Windows Assessment and Deployment Kit (https://learn.microsoft.com/en-us/windows-hardware/get-started/adk-install).
- Run `copype amd64 c:\work\winpe`.
- Run `makewinpemedia /ISO c:\work\winpe c:\work\winpe\winpe.iso`.

WIM Files:
- boot.wim: Contains WinPE.
- install.wim: Directory structure of the system disk—system files.

Image Information:
- Run `dism /Get-ImageInfo /imagefile:C:\test\images\install.wim`.

Adding Files to Image:
- Run `dism /mount-image /imagefile:"c:\work\newimage\install.wim" /Index:1 /mountdir:"c:\work\newimage\mount"`.
- Run `dism /unmount-image /mountdir:"c:\work\newimage\mount" /commit`.

Adding Drivers to Image:
- Run `dism /image:"c:\work\newimage\mount" /Add-Driver /Driver:C:\work\newimage\driver /Recurse`.

Capturing Image for Installation:
- Run `dism /Capture-Image /ImageFile:D:\install.wim /CaptureDir:C:\ /Name:"my_image"`.

Local Settings - Administrative Tools:
- msconfig.exe
- Event Viewer: 
  - Application: Logs of system applications.
  - Security: Records events related to permissions or logins.
  - Setup: Logs events from OS installation.
  - System: Overview of OS logs.
  - Forwarded Events: Special category for logs sent from other systems.
- Device Management: Drivers, hardware component status, software enabling hardware control, advanced settings of some hardware (e.g., network cards, sound, etc.).
- gpedit.msc: Local policies, various settings grouped in one user interface, often registry settings.
- Windows Security
- MMC for Firewall
- Local Users and Groups: User management, account creation, account settings.
- MMC snap-in secpol.msc: e.g., prohibition of running certain applications, password properties settings, etc.
- User Account Control
- Registry, COM, WMI

Disabling Secure Boot:

Before beginning the installation of all virtual machines, ensure that the Secure Boot feature is turned off. You can find this setting in the settings of virtualization software such as VMWare Workstation or VirtualBox.
Installation of Windows Server 2019:

1. Select a virtual machine for Windows Server 2019 installation (e.g., WS2019_1).
2. In the virtualization software menu, insert the Windows Server 2019 installation ISO image.
3. Start the installation and follow the prompts. Choose the "Standard with desktop experience" option.
Installation of Windows 10:

1. Select a virtual machine for Windows 10 installation (e.g., W10_1).
2. Again, in the virtualization software menu, insert the Windows 10 installation ISO image.
3. Start the installation and follow the prompts.