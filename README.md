# Lopez Creative Services — Active Directory Home Lab

## Project Overview

This project is a Windows Server 2022 Active Directory home lab designed to simulate the IT environment of a small creative/media services business. The lab demonstrates practical Help Desk, IT Support, and junior systems administration skills through Active Directory user management, Group Policy configuration, file-share permissions, service account restrictions, onboarding/offboarding workflows, and workstation access control.

The mock business used for this lab is **Lopez Creative Services**, a small creative/media company with departments for HR, Finance, Sales, IT, Operations, Contractors, and Admins.

---

## Lab Environment

| Component            | Details                  |
| -------------------- | ------------------------ |
| Virtualization       | VMware Workstation       |
| Server OS            | Windows Server 2022      |
| Client OS            | Windows 11 Enterprise    |
| Domain               | `DennisLopezVMlab.local` |
| Domain Controller    | `WIN-7GTDUQONLMF`        |
| Domain Controller IP | `192.168.70.128`         |
| Windows 11 Client    | `USER1WINDOWS11E`        |
| Client IP            | `192.168.70.129`         |
| Client DNS           | `192.168.70.128`         |

Environment screenshots:

* [VMware lab VM list](screenshots/01-environment/01_VMware_Lab_VM_List.png)
* [Windows 11 client joined to the domain](screenshots/01-environment/42_Client_Domain_Joined_Command_Line.png)
* [Client DNS configured to use the domain controller](screenshots/01-environment/43_Client_ipconfig_DNS_Server.png)

---

## Technologies and Tools Used

* Windows Server 2022
* Windows 11 Enterprise
* Active Directory Domain Services
* DNS
* DHCP
* Group Policy Management
* Active Directory Users and Computers
* File Server Resource Manager
* NTFS permissions
* Windows file sharing
* Remote Desktop testing
* VMware Workstation
* Command-line troubleshooting tools:

  * `gpupdate`
  * `gpresult`
  * `ipconfig`
  * `nslookup`
  * `ping`
  * `net localgroup`

---

## Business Scenario

Lopez Creative Services is a simulated small business with multiple departments and user roles. The lab was designed to model common IT administration tasks in a real organization, including user account creation, department-based access, contractor restrictions, service account controls, workstation policies, and help desk onboarding/offboarding.

Department structure:

* HR
* Finance
* Sales
* IT
* Operations
* Contractors
* Admins

Key screenshots:

* [Final cleaned Active Directory structure](screenshots/02-active-directory-structure/77_ADUC_Final_Clean_LopezCreative_OU_Structure.png)
* [Department OUs expanded](screenshots/02-active-directory-structure/06_ADUC_Departments_Expanded.png)
* [Lopez Creative Services OU structure](screenshots/02-active-directory-structure/05_ADUC_LopezCreative_OU_Structure.png)

---

## Active Directory Structure

The lab uses a structured OU layout to separate users, computers, groups, contractors, and departments. This makes Group Policy application and access management easier to understand and maintain.

The main Active Directory structure includes:

```text
DennisLopezVMlab.local
└── Lopez Creative Services
    ├── Admins
    ├── Computers
    │   ├── Servers
    │   └── Workstations
    ├── Contractors
    ├── Departments
    │   ├── Finance
    │   ├── HR
    │   ├── IT
    │   ├── Operations
    │   └── Sales
    ├── Groups
    └── Users
```

Screenshots:

* [Active Directory OU structure](screenshots/02-active-directory-structure/05_ADUC_LopezCreative_OU_Structure.png)
* [Final cleaned OU structure](screenshots/02-active-directory-structure/77_ADUC_Final_Clean_LopezCreative_OU_Structure.png)

---

## Users and Security Groups

Domain users were created for each department and assigned to security groups based on their role. Access was managed through security groups rather than assigning permissions directly to individual users.

Examples of security groups:

* `SG_HR_Users`
* `SG_Finance_Users`
* `SG_Sales_Users`
* `SG_IT_Users`
* `SG_Operations_Users`
* `SG_Contractors`
* `SG_IT_Admins`

Screenshots:

* [Security groups in Active Directory](screenshots/03-users-groups/15_ADUC_Security_Groups.png)
* [HR security group members](screenshots/03-users-groups/16_ADUC_SG_HR_Users_Members.png)
* [Sales security group naming cleanup](screenshots/03-users-groups/57_SG_Sales_Users_Members_Verified.png)
* [IT admin group members](screenshots/03-users-groups/88_ADUC_SG_IT_Admins_Members.png)

---

## Group Policy Configuration

Group Policy was used to enforce workstation restrictions, user restrictions, drive mappings, service account controls, and workstation local administrator access.

Configured GPOs included:

* Restrict Control Panel
* Disable USB Storage
* Mapped Drives
* Drive Mapping
* Password Policy
* Account Lockout Policy
* Restrict Logon to Service Account
* Allow RDP for IT Admins
* Workstation Local Admins - IT Only

Screenshots:

* [Group Policy Objects list](screenshots/04-group-policy/19_GPMC_Group_Policy_Objects.png)
* [HR linked GPOs](screenshots/04-group-policy/20_GPMC_HR_Linked_GPOs.png)
* [Control Panel restriction setting](screenshots/04-group-policy/25_GPO_ControlPanel_Setting.png)
* [USB storage restriction setting](screenshots/04-group-policy/26_GPO_Disable_USB_Setting.png)
* [Mapped drives setting](screenshots/04-group-policy/27_GPO_Mapped_Drives_Setting.png)
* [Contractors OU linked GPOs](screenshots/04-group-policy/61_GPMC_Contractors_OU_Linked_GPOs.png)
* [Workstation local admin GPO](screenshots/04-group-policy/91_GPO_Workstation_Local_Admins_IT_Admins.png)

---

## File Sharing and NTFS Permissions

A shared folder structure was created on the server:

```text
C:\CompanyShares
```

Subfolders included:

* HR
* Finance
* Sales
* Operations
* Contractors
* Public

The final permission model uses broad share-level access and specific NTFS folder permissions.

Share-level permissions:

```text
Authenticated Users → Change + Read
```

NTFS permissions control access to each department folder using security groups.

Screenshots:

* [CompanyShares folder structure](screenshots/05-file-sharing-ntfs/29_FileExplorer_CompanyShares_Subfolders.png)
* [HR folder NTFS permissions](screenshots/05-file-sharing-ntfs/31_HR_Folder_NTFS_Permissions.png)
* [Finance folder NTFS permissions](screenshots/05-file-sharing-ntfs/32_Finance_Folder_NTFS_Permissions.png)
* [Public folder NTFS permissions](screenshots/05-file-sharing-ntfs/33_Public_Folder_NTFS_Permissions.png)
* [Cleaned share permissions](screenshots/05-file-sharing-ntfs/72_CompanyShares_Share_Permissions_After_Cleanup.png)
* [Finance folder access denied test](screenshots/05-file-sharing-ntfs/36_Client_Denied_Finance_Share.png)

A permissions cleanup issue was also identified and corrected. The Sales folder inherited broad `Users` permissions from the parent folder, which allowed unintended access. Inheritance was cleaned up, and contractor access to Sales was denied afterward.

* [Inherited Users permission issue](screenshots/05-file-sharing-ntfs/73_Sales_Folder_Inherited_Users_Permission_Error.png)
* [Sales folder denied after NTFS cleanup](screenshots/05-file-sharing-ntfs/74_Client_Casey_Denied_Sales_Folder_After_NTFS_Cleanup.png)

---

## FSRM Quota Management

File Server Resource Manager was configured to apply a hard quota to `C:\CompanyShares`.

Final quota:

```text
5 GB hard quota
80% warning threshold
```

A temporary 100 MB quota was used to test quota enforcement. PowerShell-generated test files were used to confirm that the quota blocked additional file creation after the limit was exceeded. The final 5 GB quota was restored after testing.

Screenshots:

* [Final 5 GB FSRM quota](screenshots/06-fsrm-quotas/37_FSRM_CompanyShares_Quota_5GB.png)
* [Temporary 100 MB quota test](screenshots/06-fsrm-quotas/39_FSRM_Quota_Test_100MB.png)
* [PowerShell quota test blocked](screenshots/06-fsrm-quotas/40_PowerShell_Quota_Test_File_Blocked.png)
* [Quota restored after cleanup](screenshots/06-fsrm-quotas/41_FSRM_Quota_After_Test_Cleanup.png)

---

## Contractor Access Control

A contractor test user, Casey Morgan, was created to validate restricted contractor access.

Contractor configuration:

* User: Casey Morgan
* OU: Contractors
* Group: `SG_Contractors`

Contractor testing confirmed:

* Casey could access the Public folder.
* Casey could access the Contractors folder.
* Casey was denied access to HR.
* Casey was denied access to Finance.
* Casey was denied access to Sales.
* Casey was denied access to Operations.
* Casey received contractor-linked restrictions such as Control Panel blocking.

Screenshots:

* [Casey Morgan group membership](screenshots/07-contractor-access/59_Casey_Morgan_SG_Contractors_Membership.png)
* [Casey Morgan in Contractors OU](screenshots/07-contractor-access/60_ADUC_Casey_Morgan_Contractors_OU.png)
* [Contractors OU linked GPOs](screenshots/07-contractor-access/61_GPMC_Contractors_OU_Linked_GPOs.png)
* [Casey Morgan gpresult](screenshots/07-contractor-access/62_Client_gpresult_Casey_Morgan_Contractor.png)
* [Casey Morgan Control Panel blocked](screenshots/07-contractor-access/63_Client_Casey_ControlPanel_Blocked.png)
* [Casey Morgan denied HR folder](screenshots/07-contractor-access/65_Client_Casey_Denied_HR_Folder.png)
* [Casey Morgan allowed Public folder](screenshots/07-contractor-access/66_Client_Casey_Public_Folder_Access.png)
* [Casey Morgan allowed Contractors folder](screenshots/07-contractor-access/67_Client_Casey_Contractors_Folder_Access.png)
* [Casey Morgan denied Operations folder](screenshots/07-contractor-access/75_Client_Casey_Denied_Operations_Folder.png)
* [Casey Morgan denied Finance folder](screenshots/07-contractor-access/76_Client_Casey_Denied_Finance_Folder.png)

---

## Service Account Restrictions

A service account named `SVC_Fileshare` was created to represent a non-human service account for file-share related tasks.

Service account controls included:

* Account separated from normal user accounts
* Denied local interactive logon
* Denied Remote Desktop logon

Screenshots:

* [Managed Service Accounts container](screenshots/08-service-accounts/50_ADUC_Managed_Service_Accounts.png)
* [SVC_Fileshare properties](screenshots/08-service-accounts/51_ADUC_SVC_Fileshare_Properties.png)
* [Service account denied local logon](screenshots/08-service-accounts/69_GPO_Service_Account_Deny_Logon_Locally.png)
* [Service account denied RDP logon](screenshots/08-service-accounts/70_GPO_Service_Account_Deny_RDP_Logon.png)

---

## Help Desk Onboarding and Offboarding Workflow

A help desk-style onboarding/offboarding workflow was simulated using a new HR user named Maya Torres.

### Onboarding Steps

The onboarding workflow included:

1. Creating the Maya Torres user account
2. Placing the account in the HR OU
3. Adding Maya to `SG_HR_Users`
4. Logging into the Windows 11 domain-joined client
5. Verifying GPO application using `gpresult`
6. Confirming HR folder access
7. Confirming denied access to Finance and Sales
8. Confirming Control Panel restriction

Screenshots:

* [Maya Torres created in HR OU](screenshots/09-onboarding-offboarding/78_ADUC_Maya_Torres_HR_User_Created.png)
* [Maya Torres added to SG_HR_Users](screenshots/09-onboarding-offboarding/79_Maya_Torres_SG_HR_Users_Membership.png)
* [Maya Torres gpresult](screenshots/09-onboarding-offboarding/80_Client_gpresult_Maya_Torres_HR_User.png)
* [Maya Torres HR folder access](screenshots/09-onboarding-offboarding/81_Client_Maya_HR_Folder_Access.png)
* [Maya Torres denied Finance folder](screenshots/09-onboarding-offboarding/82_Client_Maya_Denied_Finance_Folder.png)
* [Maya Torres denied Sales folder](screenshots/09-onboarding-offboarding/83_Client_Maya_Denied_Sales_Folder.png)
* [Maya Torres Control Panel blocked](screenshots/09-onboarding-offboarding/84_Client_Maya_ControlPanel_Blocked.png)

### Offboarding Steps

The offboarding workflow included:

1. Disabling the Maya Torres account
2. Testing login after disablement
3. Confirming the disabled account could not sign in
4. Removing Maya from `SG_HR_Users`

Screenshots:

* [Maya Torres account disabled](screenshots/09-onboarding-offboarding/85_ADUC_Maya_Torres_Account_Disabled.png)
* [Maya Torres login blocked after disablement](screenshots/09-onboarding-offboarding/86_Client_Maya_Login_Blocked_After_Disable.png)
* [Maya Torres removed from SG_HR_Users](screenshots/09-onboarding-offboarding/87_ADUC_Maya_Removed_From_SG_HR_Users.png)

---

## Privileged Access Control

Privileged access control was implemented using a dedicated IT admin group.

Configured group:

```text
SG_IT_Admins
```

Members:

* Alex Rivera
* Dennis Lopez

This group was used to manage elevated workstation permissions. Group Policy Preferences were configured to add `SG_IT_Admins` to the local Administrators group on the Windows 11 workstation.

Testing confirmed:

* Standard HR user Hannah Reed was denied Remote Desktop access.
* `SG_IT_Admins` was added to the workstation local Administrators group.
* Standard user groups such as HR users and Domain Users were not included in local Administrators.

Screenshots:

* [SG_IT_Admins members](screenshots/10-privileged-access/88_ADUC_SG_IT_Admins_Members.png)
* [Allow RDP for IT Admins setting](screenshots/10-privileged-access/89_GPO_Allow_RDP_For_IT_Admins_Setting.png)
* [Hannah Reed denied RDP access](screenshots/10-privileged-access/90_Client_Hannah_RDP_Denied_Not_Authorized.png)
* [Workstation local admin GPO](screenshots/10-privileged-access/91_GPO_Workstation_Local_Admins_IT_Admins.png)
* [SG_IT_Admins applied to local Administrators](screenshots/10-privileged-access/92_Client_Local_Administrators_SG_IT_Admins_Applied.png)
* [Hannah Reed not in local Administrators](screenshots/10-privileged-access/93_Client_Hannah_Not_In_Local_Admins_Group.png)

---

## Troubleshooting and Validation

Troubleshooting and validation were performed throughout the project using Windows administrative tools and command-line utilities.

Validation included:

* Confirming client DNS pointed to the domain controller
* Confirming domain name resolution with `nslookup`
* Confirming client-to-domain-controller connectivity with `ping`
* Verifying applied Group Policy Objects using `gpresult`
* Refreshing policy with `gpupdate`
* Resolving FSRM quota testing issues
* Cleaning up inherited NTFS permissions
* Adjusting GPO scope and security filtering during RDP testing
* Pivoting from Domain Controller RDP testing to workstation local admin control for cleaner privileged access validation

Screenshots:

* [Client DNS server configuration](screenshots/11-troubleshooting/43_Client_ipconfig_DNS_Server.png)
* [Domain name resolution with nslookup](screenshots/11-troubleshooting/44_Client_nslookup_Domain.png)
* [Ping test to domain controller](screenshots/11-troubleshooting/49_Client_Ping_Domain_Controller.png)
* [gpresult showing Control Panel GPO applied](screenshots/11-troubleshooting/55_GPO_Result_ControlPanel_Applied.png)
* [FSRM quota issue resolved](screenshots/11-troubleshooting/56_FSRM_Quota_Error_Resolved.png)
* [Sales folder inherited permission issue](screenshots/11-troubleshooting/73_Sales_Folder_Inherited_Users_Permission_Error.png)
* [Sales folder denied after NTFS cleanup](screenshots/11-troubleshooting/74_Client_Casey_Denied_Sales_Folder_After_NTFS_Cleanup.png)

---

## Skills Demonstrated

| Skill Area                    | Evidence                                                                       |
| ----------------------------- | ------------------------------------------------------------------------------ |
| Windows Server Administration | Domain controller, DNS/DHCP, file sharing, FSRM                                |
| Active Directory              | OUs, users, groups, service accounts                                           |
| Group Policy                  | Control Panel restriction, USB restriction, mapped drives, local admin control |
| File Sharing                  | CompanyShares folder structure and share permissions                           |
| NTFS Permissions              | Department-based access control                                                |
| Least Privilege               | Contractor restrictions, user folder denial tests, local admin restriction     |
| Help Desk Support             | User onboarding, offboarding, login testing                                    |
| Troubleshooting               | `gpresult`, `gpupdate`, `ipconfig`, `nslookup`, `ping`                         |
| Workstation Administration    | Windows 11 domain client testing and local admin group control                 |
| Access Control                | Security groups, service account logon restrictions, privileged access group   |

---

## Project Outcome

This lab demonstrates the ability to build, configure, test, troubleshoot, and document a small business Active Directory environment.

The project includes:

* A Windows Server 2022 domain controller
* A domain-joined Windows 11 client
* Organized Active Directory OU structure
* Department users and security groups
* Group Policy enforcement
* File shares and NTFS permissions
* File Server Resource Manager quotas
* Contractor access restrictions
* Service account logon restrictions
* Help desk onboarding/offboarding workflow
* Workstation local administrator control
* Troubleshooting and validation evidence

This project supports Help Desk, IT Support, Desktop Support, and entry-level systems administration roles by demonstrating practical hands-on experience with Windows Server, Active Directory, Group Policy, file-share permissions, and user lifecycle management.

---

## Resume Summary Version

Built a Windows Server 2022 Active Directory home lab simulating a small business IT environment with a domain controller, domain-joined Windows 11 workstation, department OUs, users, security groups, Group Policy Objects, mapped drives, NTFS file-share permissions, FSRM quotas, contractor restrictions, service account logon restrictions, help desk onboarding/offboarding, and workstation local administrator control. Validated access and troubleshooting using `gpresult`, `gpupdate`, `ipconfig`, `nslookup`, `ping`, Remote Desktop testing, and client-side file-share testing.
