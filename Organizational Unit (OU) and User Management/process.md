### Domain Configuration Settings Check

dcdiag.exe

# User and OU Management

## Active Directory Users and Computers

Turn on View > Advanced Features

**Object Creation**

- OUs, users, computers, groups

Groups

- security/distribution — distribution for grouping only for exchange — emails. Security can be used for specifying permissions.
- Domain local — can contain objects from any domain. It can only be used for specifying permissions in the domain where it is created.
- Universal — objects from the same (or trusted) forest where it is created. It can be used for specifying permissions in the same (or trusted) forest where it is created.
- Global — objects from the same domain where it is created. It can be used for specifying permissions in the same (or trusted) forest where it is created.

**Object Management**

- General, Attribute editor,
- Account — account information, through which we log in, unlocking accounts, password policies, etc.
- Profile — storage of user settings, data directory.

## Active Directory Administration Center

- Management tool ADDS
- Active Directory Schema (must enable https://social.technet.microsoft.com/wiki/contents/articles/51121.active-directory-schema-update-and-custom-attribute.aspx#Step_3_Register_Schema_Management_Snap-in)

## Domain Policy Settings

In addition to managing resources (computers, network printers, shared disks, etc.), Active Directory also allows for the configuration of users and computers. This configuration is done through a set of rules that specify the behavior of the respective resource. This set of rules is then called a Group Policy and is stored in an entity called Group Policy Object (GPO). There are several thousand rules to choose from when defining a policy, divided into several categories. The basic division is according to the specification of the target object:

- Computer Configuration — this part of the policy is focused on computer configuration and is always applied after the computer starts (regardless of which user is logged in).
- User Configuration — applied after a user logs in (regardless of which computer they are on).

The settings themselves are usually done through registry modifications. If a particular configuration (for computer or user) is not used, the GPO can be set so that this option is not applied to the target container at all (thus not unnecessarily consuming domain controller system resources).

Policy application within AD on a specific machine can be several. Each machine also has a local policy, which is valid even if the machine is not in the domain at all. Unlike domain policies, local policy cannot be stored in the form of a GPO, so there is only one policy (or it cannot be done in a friendly way through the GUI).

Policy Configuration

As mentioned, policy settings on computers or users are done through the GPMC editor. Policies are created and applied to the appropriate containers in it. Policies can only be applied to OUs, domains, or sites. Therefore, policies cannot be applied directly to users or computers (or groups). The policy can either be created anew and edited, or an existing policy can be linked to the container, or GPO (GPO context menu). GPO editing is also done through the GPO context menu — in a separate window, all rules and settings can then be edited, and all changes are saved immediately (so there is no save function, as in other editors).

> Caution, policies with user settings need to be applied to OUs with users. However, if there is a way to bypass this and set user policies on an OU with a device — policy loopback. More at
> 
> 
> *https://docs.microsoft.com/en-GB/troubleshoot/windows-server/group-policy/loopback-processing-of-group-policy*
> 
> *.*
> 

After linking the policy to the container, it is necessary to wait for the update of this policy in the respective object. The update interval is approximately every 90 minutes (random value to prevent all policies from being updated at once and thus causing a large peak load). Policy update can be manually performed from the target computer by running commands:

```
gpupdate /force
```

in the Windows command line or in PowerShell by command:

```
Invoke-GPUpdate -force
```

Policy checks can be performed using the MMC snap-in **Resultant Set of Policy.**

**Inheritance and Policy Application Order**

Because the container structure is hierarchical, there is a concept of **inheritance** in policies — the policy of the parent object will automatically be applied to the subordinate object. However, inheritance can be **blocked** (Block Inheritance command from the container context menu). However, blocking inheritance can be "overridden" by using the **Enforced** command from the GPO context menu — this option changes the order of policy processing (or application) on the container.

The order of policy application is a very important element — since the set of rules that can be set is the same in each policy, it may happen that some policies will have the same rules applied with different settings. If this happens, the settings already changed will be overwritten by the following policy. If several policies change the same setting, what the last policy sets applies.

![Group Policy Loop](https://cdn-images-1.medium.com/max/1600/0*_RMOEExnutjkMfWC.jpeg)

More information about policies, including setup options, can be found on the Samurai website:

https://www.samuraj-cz.com/clanek/group-policy-rizeni-aplikace-politik/

Policy is also covered in detail in Lukáš Brázda's presentation:

https://www.wug.cz/zaznamy/GetFile.ashx?EventStoredFileID=344

## GPM Console

**GPO Management**

- Scope, filtering (security/WMI), GPO status.
- Adding, removing, editing, Settings
- StarterGPO — templates, only Administrative Templates part of GPO.

**Remote Update**

- Remote Scheduled Task Management (RPC), Remote Scheduled Task Management (RPC-EPMAP), and Windows Management Instrumentation (WMI-In)
- We will address this in the next lesson.

```
Preparation for today's exercise:1. Create users admin and accounting, set passwords for them, and add them to AD global groups "administration" (admin) and "accounting" (accounting) - you must create them as well.
2. Join computers ict01 and accounting01 to the domain.
3. Create two departments - IT and Finance, and divide computers and users into them.
```

# Selected AD Settings
Set different password security levels for IT department employees and other employees. IT admins must have a password of at least 12 characters, which must meet complexity requirements, and they must change it every six months (the system must remember at least 10 unique passwords). Regular users must have a password of at least 8 characters, meeting the complex password requirements, and the account must be secured against brute-force attacks (repeated password guessing).

**Login Security Settings**

Active Directory Administration Center->Domain->System->Password Settings Container->new policy

More info http://woshub.com/fine-grained-password-policy-in

**Windows Server 2012 R2/**

**Setting Administrative Rights for IT Administrators**

Configure the settings for all machines in your domain so that IT administrators have administrative permissions.

Computer Configuration -> Policies->Windows Settings->Security Settings->Restricted groups

**Issue: Users Can Log in Anywhere**

Users can log in anywhere (i.e., even on PCs they shouldn't have access to), such as servers, administrator computers, etc.

Limit logins to individual PCs.
Computer Configuration -> Policies->Windows Settings->Security Settings->User Rights Assignment->Allow log on locally -> add the correct group/user

**Issue: Anyone Can Join PCs to the Domain**

### **Solution 1**

After joining the domain, PCs are added to a part of the domain that will be isolated from other resources, and it will be necessary to reassign it to the correct network segment.

Use the program C:\windows\system32\redircmp
In CMD with the command:
redircmp “OU=OS2PC,DC=OS2,dc=local

### **Solution 2**

**Solution:** 

ADDS allows specifying users who can join computers to the domain on the domain controller.
Group Policy Management ->Domain Controler policy ->Computer Configuration->Policies->Windows Settings->Security Settings->Local Policy->User Rights Assignment->Add workstation to

**Issue: Unavailability of AD Server**

### **Solution 1**

**Caching** 
- If login caching is enabled, it is possible to log in to a machine that is not connected to the network! The condition is that the user must have logged into the machine at least once, and login caching must be enabled.
Computer Configuration->Policies->Windows Settings->Security Settings->Local Policy->Security Options->Interactive logon: Number of previous logons to cache (in case domain controller is not available).

### **Solution 2**

**Redundancy**
Install Core server + manage via Server Administration

New domain controller

Test — turn off one machine (may not work due to DNS).

**Issue: Enabling Roaming Profiles**
- create share — create directory (e.g., Profiles) — PTM on directory->ShareWith Specific people -> add Domain Users.
- Copy the network resource path
- Set the path for profiles in the profile tab for users

But we have two problems with this:

- Permissions are set rather clumsily
- The path to profiles must be edited manually for each user separately

**Modification:**

- Create a Roaming Profiles group and assign users to it
- create Roaming Profiles GPO and set the path to the share
- Computer Configuration/Administrative Templates/System/User Profiles” gpmc_settingName=”Set roaming profile path for all users logging onto this computer”
- Limit share permissions — CREATOR/OWNER only list dir and add dir

**Debugging**

If profile loading and saving are not working:

- try deleting all .bak keys from HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\ProfileList
- erase other user mentions from the registry
- delete local directories from C:\users

https://docs.microsoft.com/en-us/windows-server/storage/folder-redirection/deploy-roaming-user-profiles

**Issue: Installing Fonts into the System**
The company has developed a new unified visual style (JVS) and wants employees to use it in communication with customers and suppliers. Included in the JVS is a specific font. Users need to install the font on their computers.

Preferences->Windows Settings->Files->File (Target Path:\Windows\Fonts\Montserrat-Black.ttf)->Montserrat-Black.ttf (Order: 1)
- Source file \\share\fonts\Montserrat\Montserrat-Black.ttf
- Destination file C:\Windows\Fonts\Montserrat-Black.ttf

Preferences->Windows Settings->Registry>Montserrat Black (True Type) (Order: 1)- Hive: HKEY_LOCAL_MACHINE
- Key path: SOFTWARE\Microsoft\Windows NT\CurrentVersion\Fonts
- Value name: Montserrat Black (True Type)
- Value type: REG_SZ
- Value data: Montserrat-Black.ttf

**Issue: Lock Screen Background**
Setting the company logo as the background on all public login screens of information kiosks.

Computer Configuration->Administrative Templates->Control Panel->Personalization->Force a specific default lock screen image
\\share\lockscreeny\ui_screen_1680x1050.png or local path

**Issue: File Extensions and Hidden Files**
User Configuration (Enabled)->Preferences->Control Panel Settings->Folder Options->Folder Options (At least Windows Vista)

1. **Creating Organizational Units (OUs) for Departments**:
   - Log in to the server with the installed AD DS service using an account with administrative privileges.
   - Open "Active Directory Users and Computers" (ADUC). You can find it in the "Administrative Tools" menu in the Start menu.
   - In the ADUC window, right-click on the domain folder and select "New" -> "Organizational Unit".
   - Enter a name for the unit corresponding to one of the departments in the company, such as "Sales", "Accounting", "IT", "Production", "Management".
   - Repeat this process for each department in the company to create corresponding organizational units for each department.

2. **Creating Global Groups for Each Department**:
   - In the ADUC window, right-click on the organizational unit for the relevant department and select "New" -> "Group".
   - Enter a name for the group, such as "SalesDepartment", "AccountingDepartment", "ITDepartment", "ProductionDepartment", "ManagementDepartment".
   - When creating the group, set the group type to "Global" and scope to "Security".
   - Repeat this process for each organizational unit and create corresponding global groups for each department in the company.

3. **Assigning Users to Respective Departments**:
   - In the ADUC window, right-click on the global group corresponding to one of the departments and select "Add Members".
   - Select the users who belong to that department and click the "Add" button, then click "OK".
   - Repeat this process for each global group and assign respective users to individual departments in the company.