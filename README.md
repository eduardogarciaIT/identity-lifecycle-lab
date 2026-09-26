<h1 align="center">Enterprise Active Directory Administration & Security</h1>

This repository demonstrates the administration of a Windows Server Active Directory environment deployed using Microsoft Hyper-V. The lab covers the complete identity lifecycle—from provisioning to secure offboarding—as well as Role-Based Access Control (RBAC), NTFS file share permissions, and Group Policy Object (GPO) deployment.

### Active Directory (On-Premises Identity Management)
*This section outlines traditional user lifecycle management, demonstrating both the Active Directory Users and Computers (ADUC) graphical interface and PowerShell automation.*

### Organizational Unit (OU) Architecture

To ensure proper application of Group Policy Objects (GPOs) and logical structuring, dedicated Organizational Units were created for distinct departments using PowerShell and verified via the Active Directory Users and Computers (ADUC) GUI.

**Command Executed:**
```powershell
New-ADOrganizationalUnit -Name "IT Department" -Path "DC=lab,DC=local"
New-ADOrganizationalUnit -Name "HR Department" -Path "DC=lab,DC=local"
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/c38bcfd2-bec0-415a-9470-d09010de48f9" />


### User Account Provisioning

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

### Help Desk Operations (Password Reset & Account Recovery)

To simulate resolving a common end-user escalation, a user account password was reset. As a best practice, the account was cleared of any potential lockouts, a new secure temporary password was generated, and the user was forced to reset their credentials upon next logon.

**Command Executed:**
```powershell
Unlock-ADAccount -Identity "jsmith"
Set-ADAccountPassword -Identity "jsmith" -Reset -NewPassword (ConvertTo-SecureString "NewTempP@ssw0rd!" -AsPlainText -Force)
Set-ADUser -Identity "jsmith" -ChangePasswordAtLogon $true
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/f704159b-c292-4eb6-8b8d-610a2c24d498" />


### Role-Based Access Control (RBAC)

To maintain the principle of least privilege, access to network resources is managed through Security Groups rather than individual user assignments. Users are added to global security groups based on their department and role.

**Command Executed:**
```powershell
New-ADGroup -Name "IT_Share_Access" -GroupCategory Security -GroupScope Global -Path "OU=IT Department,DC=lab,DC=local"
Add-ADGroupMember -Identity "IT_Share_Access" -Members "jsmith"
```
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/d41b93ff-5e53-4182-bd7d-5a3fcb703fe9" />


### Network File Shares & Permissions (NTFS)

To facilitate secure departmental collaboration, a network file share was created and restricted using NTFS and Share permissions. Access is exclusively granted to the corresponding Role-Based Security Group, adhering to the principle of least privilege.

**Command Executed:**
```powershell
New-Item -Path "C:\IT_Department_Data" -ItemType Directory
New-SmbShare -Name "IT_Data" -Path "C:\IT_Department_Data" -ChangeAccess "lab\IT_Share_Access"
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/e14abe9e-2808-4c1e-8728-e763d421ccbe" />

### Group Policy Objects (GPOs)

To automate workspace configuration and standardize the end-user experience, a Group Policy Object (GPO) was created and linked directly to the IT Department OU. This specific policy is designed to automatically map the secure IT network share as a local drive upon user logon.

**Command Executed:**
```powershell
New-GPO -Name "IT_Drive_Mapping"
New-GPLink -Name "IT_Drive_Mapping" -Target "OU=IT Department,DC=lab,DC=local"
```

<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/1f26a205-9191-4bc7-a968-ce47578c8f38" />

### Group Policy Configuration (Drive Mapping)

The linked GPO was edited to include a Group Policy Preference (GPP) that automatically maps the network share to the "T:" drive for all users in the IT Department OU. A manual policy refresh was then executed to ensure immediate application across the domain.

**Command Executed:**
```powershell
gpupdate /force
```

<img width="1366" height="764" alt="image" src="https://github.com/user-attachments/assets/025790ac-76d9-445d-898c-8970775f4114" />

### Secure Deprovisioning (Offboarding)

Upon employee termination, accounts are immediately disabled rather than deleted to preserve security audit logs and SID history. All security group memberships are stripped to instantly revoke access to network resources.

**Command Executed:**
```powershell
Disable-ADAccount -Identity "jsmith"
Remove-ADGroupMember -Identity "IT_Share_Access" -Members "jsmith" -Confirm:$false
```
<img width="1365" height="766" alt="image" src="https://github.com/user-attachments/assets/8727d4a8-9bd0-4ca1-bbf1-71f25bc300d9" />
