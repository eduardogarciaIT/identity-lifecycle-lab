<h1 align="center">User Provisioning & Identity Lifecycle Management</h1>

This repository demonstrates the end-to-end identity lifecycle—covering the creation, configuration, and secure offboarding of user accounts in both legacy on-premises and modern cloud environments. 

## 1. Active Directory (On-Premises Identity Management)
*This section outlines traditional user lifecycle management utilizing Windows Server, Active Directory Users and Computers (ADUC), and PowerShell automation.*

### Step 1: Organizational Unit (OU) Architecture
To ensure proper application of Group Policy Objects (GPOs) and logical structuring, dedicated Organizational Units were created for distinct departments. 

> 📸 **[Drag and drop your ADUC OU structure screenshot here]**

### Step 2: User Account Provisioning
New employee accounts are provisioned with standard naming conventions, assigned to their respective department OUs, and configured with temporary passwords requiring a reset upon first logon to maintain security compliance.

**PowerShell Automation:**
```powershell
New-ADUser -Name "John Smith" `
-SamAccountName "jsmith" `
-UserPrincipalName "jsmith@yourdomain.com" `
-Path "OU=IT Department,DC=yourdomain,DC=com" `
-AccountPassword (ConvertTo-SecureString "TempP@ssw0rd!" -AsPlainText -Force) `
-ChangePasswordAtLogon $true `
-Enabled $true

## 2. Microsoft Entra ID (Cloud Identity Management)
*This section covers modern cloud-native identity management and automated provisioning workflows.*

* **Cloud User & Group Provisioning:** Creating cloud-native identities and assigning Microsoft 365 licensing.
* **Dynamic Group Memberships:** Configuring rules to automatically add or remove users from groups based on user attributes (e.g., department or job title).
* **Enterprise App Provisioning (SCIM):** Automating account creation and synchronization between Entra ID and third-party SaaS applications.
* **Cloud Offboarding:** Revoking sign-in sessions, resetting credentials, and removing SaaS application access immediately upon termination.
