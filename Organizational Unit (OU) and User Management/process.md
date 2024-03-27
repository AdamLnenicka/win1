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