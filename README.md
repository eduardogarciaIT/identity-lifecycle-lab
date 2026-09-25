<h1 align="center">User Provisioning & Identity Lifecycle Management</h1>

This repository demonstrates the end-to-end identity lifecycle—covering the creation, configuration, and secure offboarding of user accounts in both legacy on-premises and modern cloud environments. 

## 1. Active Directory (On-Premises Identity Management)
*This section covers traditional user lifecycle management utilizing Windows Server and Active Directory Users and Computers (ADUC).*

* **Account Creation & Configuration:** Creating standard user and administrative accounts, configuring profile properties, and setting initial password policies.
* **Organizational Units (OUs):** Structuring user accounts into logical departments for targeted Group Policy application.
* **Role-Based Access Control (RBAC):** Assigning users to specific security groups to grant access to file shares and local network resources.
* **Secure Deprovisioning:** Safely offboarding employees by disabling accounts, stripping group memberships, and archiving home directory data to prevent unauthorized legacy access.

## 2. Microsoft Entra ID (Cloud Identity Management)
*This section covers modern cloud-native identity management and automated provisioning workflows.*

* **Cloud User & Group Provisioning:** Creating cloud-native identities and assigning Microsoft 365 licensing.
* **Dynamic Group Memberships:** Configuring rules to automatically add or remove users from groups based on user attributes (e.g., department or job title).
* **Enterprise App Provisioning (SCIM):** Automating account creation and synchronization between Entra ID and third-party SaaS applications.
* **Cloud Offboarding:** Revoking sign-in sessions, resetting credentials, and removing SaaS application access immediately upon termination.
