<h1 align="center">User Provisioning & Identity Lifecycle Management</h1>

This repository demonstrates the end-to-end identity lifecycle—covering the creation, configuration, and secure offboarding of user accounts in both legacy on-premises and modern cloud environments. 

## 1. Active Directory (On-Premises Identity Management)
*This section outlines traditional user lifecycle management, demonstrating both the Active Directory Users and Computers (ADUC) graphical interface and PowerShell automation.*

### Step 1: Organizational Unit (OU) Architecture

To ensure proper application of Group Policy Objects (GPOs) and logical structuring, dedicated Organizational Units were created for distinct departments using PowerShell and verified via the Active Directory Users and Computers (ADUC) GUI.

**Command Executed:**
```powershell
New-ADOrganizationalUnit -Name "IT Department" -Path "DC=lab,DC=local"
New-ADOrganizationalUnit -Name "HR Department" -Path "DC=lab,DC=local"
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/c38bcfd2-bec0-415a-9470-d09010de48f9" />


### Step 2: User Account Provisioning

New employee accounts are provisioned with standard naming conventions, assigned to their respective department OUs, and configured with temporary passwords requiring a reset upon first logon to maintain security compliance.

**Command Executed:**
```powershell
New-ADUser -Name "John Smith" `
-SamAccountName "jsmith" `
-UserPrincipalName "jsmith@lab.local" `
-Path "OU=IT Department,DC=lab,DC=local" `
-AccountPassword (ConvertTo-SecureString "TempP@ssw0rd!" -AsPlainText -Force) `
-ChangePasswordAtLogon $true `
-Enabled $true
```

<img width="1364" height="760" alt="image" src="https://github.com/user-attachments/assets/a190d5f0-494b-4dfb-89f7-0ce1620429f8" />

### Step 3: Role-Based Access Control (RBAC)

To maintain the principle of least privilege, access to network resources is managed through Security Groups rather than individual user assignments. Users are added to global security groups based on their department and role.

**Command Executed:**
```powershell
New-ADGroup -Name "IT_Share_Access" -GroupCategory Security -GroupScope Global -Path "OU=IT Department,DC=lab,DC=local"
Add-ADGroupMember -Identity "IT_Share_Access" -Members "jsmith"
```


## 2. Microsoft Entra ID (Cloud Identity Management)
*This section covers modern cloud-native identity management and automated provisioning workflows.*

* **Cloud User & Group Provisioning:** Creating cloud-native identities and assigning Microsoft 365 licensing.
* **Dynamic Group Memberships:** Configuring rules to automatically add or remove users from groups based on user attributes (e.g., department or job title).
* **Enterprise App Provisioning (SCIM):** Automating account creation and synchronization between Entra ID and third-party SaaS applications.
* **Cloud Offboarding:** Revoking sign-in sessions, resetting credentials, and removing SaaS application access immediately upon termination.
