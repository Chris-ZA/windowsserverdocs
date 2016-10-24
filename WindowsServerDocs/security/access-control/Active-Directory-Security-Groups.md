---
title: Active Directory Security Groups
description: "Windows Server Security"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-access-control
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 473eff45-2b15-4f68-964f-df49f24fa3f6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
---
# Active Directory Security Groups

>Applies To: Windows Server&reg; 2016, Windows Server&reg; 2012 R2, Windows Server&reg; 2012

This reference topic for the IT professional describes the default Active Directory security groups.

There are two forms of common security principals in Active Directory: user accounts and computer accounts. These accounts represent a physical entity (a person or a computer). User accounts can also be used as dedicated service accounts for some applications. Security groups are used to collect user accounts, computer accounts, and other groups into manageable units.

In the Windows Server operating system, there are several built-in accounts and security groups that are preconfigured with the appropriate rights and permissions to perform specific tasks. For Active Directory, there are two types of administrative responsibilities:

-   **Service administrators** Responsible for maintaining and delivering Active Directory Domain Services (AD DS), including managing domain controllers and configuring the AD DS.

-   **Data administrators** Responsible for maintaining the data that is stored in AD DS and on domain member servers and workstations.

## About Active Directory groups
Groups are used to collect user accounts, computer accounts, and other groups into manageable units. Working with groups instead of with individual users helps simplify network maintenance and administration.

There are two types of groups in Active Directory:

-   **Distribution groups** Used to create email distribution lists.

-   **Security groups** Used to assign permissions to shared resources.

### Distribution groups
Distribution groups can be used only with email applications (such as Exchange Server) to send email to collections of users. Distribution groups are not security enabled, which means that they cannot be listed in discretionary access control lists (DACLs).

### Security groups
Security groups can provide an efficient way to assign access to resources on your network. By using security groups, you can:

-   Assign user rights to security groups in Active Directory.

    User rights are assigned to a security group to determine what members of that group can do within the scope of a domain or forest. User rights are automatically assigned to some security groups when Active Directory is installed to help administrators define a person???s administrative role in the domain.

    For example, a user who is added to the Backup Operators group in Active Directory has the ability to back up and restore files and directories that are located on each domain controller in the domain. This is possible because, by default, the user rights **Backup files and directories** and **Restore files and directories** are automatically assigned to the Backup Operators group. Therefore, members of this group inherit the user rights that are assigned to that group.

    You can use Group Policy to assign user rights to security groups to delegate specific tasks. For more information about using Group Policy, see [User Rights Assignment](User-Rights-Assignment.md).

-   Assign permissions to security groups for resources.

    Permissions are different than user rights. Permissions are assigned to the security group for the shared resource. Permissions determine who can access the resource and the level of access, such as Full Control. Some permissions that are set on domain objects are automatically assigned to allow various levels of access to default security groups, such as the Account Operators group or the Domain Admins group.

    Security groups are listed in DACLs that define permissions on resources and objects. When assigning permissions for resources (file shares, printers, and so on), administrators should assign those permissions to a security group rather than to individual users. The permissions are assigned once to the group, instead of several times to each individual user. Each account that is added to a group receives the rights that are assigned to that group in Active Directory, and the user receives the permissions that are defined for that group.

Like distribution groups, security groups can be used as an email entity. Sending an email message to the group sends the message to all the members of the group.

### Group scope
Groups are characterized by a scope that identifies the extent to which the group is applied in the domain tree or forest. The scope of the group defines where the group can be granted permissions. The following three group scopes are defined by Active Directory:

-   Universal

-   Global

-   Domain Local

> [!NOTE]
> In addition to these three scopes, the default groups in the **Builtin** container have a group scope of Builtin Local. This group scope and group type cannot be changed.

The following table lists the three group scopes and more information about each scope for a security group.

**Group scopes**

|Scope|Possible Members|Scope Conversion|Can Grant Permissions|Possible Member of|
|-----|----------|----------|-------------|-----------|
|Universal|Accounts from any domain in the same forest<br /><br />Global groups from any domain in the same forest<br /><br />Other Universal groups from any domain in the same forest|Can be converted to Domain Local scope<br /><br />Can be converted to Global scope if the group does not contain any other Universal groups|On any domain in the same forest or trusting forests|Other Universal groups in the same forest<br /><br />Domain Local groups in the same forest or trusting forests<br /><br />Local groups on computers in the same forest or trusting forests|
|Global|Accounts from the same domain<br /><br />Other Global groups from the same domain|Can be converted to Universal scope if the group is not a member of any other global group|On any domain in the same forest, or trusting domains or forests|Universal groups from any domain in the same forest<br /><br />Other Global groups from the same domain<br /><br />Domain Local groups from any domain in the same forest, or from any trusting domain|
|Domain Local|Accounts from any domain or any trusted domain<br /><br />Global groups from any domain or any trusted domain<br /><br />Universal groups from any domain in the same forest<br /><br />Other Domain Local groups from the same domain<br /><br />Accounts, Global groups, and Universal groups from other forests and from external domains|Can be converted to Universal scope if the group does not contain any other Domain Local groups|Within the same domain|Other Domain Local groups from the same domain<br /><br />Local groups on computers in the same domain, excluding built-in groups that have well-known SIDs|

### Special identity groups
Special identities are generally referred to as groups. Special identity groups do not have specific memberships that can be modified, but they can represent different users at different times, depending on the circumstances. Some of these groups include Creator Owner, Batch, and Authenticated User.

For information about all the special identity groups, see [Special Identities](Special-Identities.md).

## Default security groups
Default groups, such as the Domain Admins group, are security groups that are created automatically when you create an Active Directory domain. You can use these predefined groups to help control access to shared resources and to delegate specific domain-wide administrative roles.

Many default groups are automatically assigned a set of user rights that authorize members of the group to perform specific actions in a domain, such as logging on to a local system or backing up files and folders. For example, a member of the Backup Operators group has the right to perform backup operations for all domain controllers in the domain.

When you add a user to a group, the user receives all the user rights that are assigned to the group and all the permissions that are assigned to the group for any shared resources.

Default groups are located in the **Builtin** container and in the **Users** container in Active Directory Users and Computers. The **Builtin** container includes groups that are defined with the Domain Local scope. The **Users** includes contains groups that are defined with Global scope and groups that are defined with Domain Local scope. You can move groups that are located in these containers to other groups or organizational units (OU) within the domain, but you cannot move them to other domains.

Some of the administrative groups that are listed in this topic and all members of these groups are protected by a background process that periodically checks for and applies a specific security descriptor. This descriptor is a data structure that contains security information associated with a protected object. This process ensures that any successful unauthorized attempt to modify the security descriptor on one of the administrative accounts or groups will be overwritten with the protected settings.

The security descriptor is present on the **AdminSDHolder** object. This means that if you want to modify the permissions on one of the service administrator groups or on any of its member accounts, you must modify the security descriptor on the **AdminSDHolder** object so that it will be applied consistently. Be careful when you make these modifications because you are also changing the default settings that will be applied to all of your protected administrative accounts.

### <a name="BKMK_GroupsTable"></a>Active Directory default security groups by operating system version
The following tables provide descriptions of the default groups that are located in the **Builtin** and **Users** containers in each operating system.

|Default Security Group| Windows Server 2012 R2 | Windows Server 2012 | Windows Server 2008 R2 | Windows Server 2008 |
|-------------|---------------------------------|------------------------------|---------------------------------|---------------------------------|
|[Access Control Assistance Operators](#BKMK_ACAsstOps)|Yes|Yes|||
|[Account Operators](#BKMK_AccountOperators)|Yes|Yes|Yes|Yes|
|[Administrators](#BKMK_Admins)|Yes|Yes|Yes|Yes|
|[Allowed RODC Password Replication Group](#BKMK_AllowedRODCPwdRepl)|Yes|Yes|Yes|Yes|
|[Backup Operators](#BKMK_BackupOperators)|Yes|Yes|Yes|Yes|
|[Certificate Service DCOM Access](#BKMK_CertificateServiceDCOMAccess)|Yes|Yes|Yes|Yes|
|[Cert Publishers](#BKMK_CertPublishers)|Yes|Yes|Yes|Yes|
|[Cloneable Domain Controllers](#BKMK_CloneableDomainControllers)|Yes|Yes|||
|[Cryptographic Operators](#BKMK_CryptographicOperators)|Yes|Yes|Yes|Yes|
|[Denied RODC Password Replication Group](#BKMK_DeniedRODCPwdRepl)|Yes|Yes|Yes|Yes|
|[Distributed COM Users](#BKMK_DistributedCOMUsers)|Yes|Yes|Yes|Yes|
|[DnsUpdateProxy](#BKMK_DnsUpdateProxy)|Yes|Yes|Yes|Yes|
|[DnsAdmins](#BKMK_DnsAdmins)|Yes|Yes|Yes|Yes|
|[Domain Admins](#BKMK_DomainAdmins)|Yes|Yes|Yes|Yes|
|[Domain Computers](#BKMK_DomainComputers)|Yes|Yes|Yes|Yes|
|[Domain Controllers](#BKMK_DomainControllers)|Yes|Yes|Yes|Yes|
|[Domain Guests](#BKMK_DomainGuests)|Yes|Yes|Yes|Yes|
|[Domain Users](#BKMK_DomainUsers)|Yes|Yes|Yes|Yes|
|[Enterprise Admins](#BKMK_EntAdmins)|Yes|Yes|Yes|Yes|
|[Enterprise Read-only Domain Controllers](#BKMK_EntRODC)|Yes|Yes|Yes|Yes|
|[Event Log Readers](#BKMK_EventLogReaders)|Yes|Yes|Yes|Yes|
|[Group Policy Creators Owners](#BKMK_GPCreatorsOwners)|Yes|Yes|Yes|Yes|
|[Guests](#BKMK_Guests)|Yes|Yes|Yes|Yes|
|[Hyper-V Administrators](#BKMK_HyperVAdministrators)|Yes|Yes|||
|[IIS_IUSRS](#BKMK_IIS_IUSRS)|Yes|Yes|Yes|Yes|
|[Incoming Forest Trust Builders](#BKMK_InForestTrustBldrs)|Yes|Yes|Yes|Yes|
|[Network Configuration Operators](#BKMK_NetworkCfgOperators)|Yes|Yes|Yes|Yes|
|[Performance Log Users](#BKMK_PerfLogUsers)|Yes|Yes|Yes|Yes|
|[Performance Monitor Users](#BKMK_PerfMonitorUsers)|Yes|Yes|Yes|Yes|
|[Pre???Windows 2000 Compatible Access](#BKMK_Pre-WS2KcompatAccess)|Yes|Yes|Yes|Yes|
|[Print Operators](#BKMK_PrintOperators)|Yes|Yes|Yes|Yes|
|[Protected Users](#BKMK_ProtectedUsers)|Yes||||
|[RAS and IAS Servers](#BKMK_RASandIAS)|Yes|Yes|Yes|Yes|
|[RDS Endpoint Servers](#BKMK_RDSEndpointServers)|Yes|Yes|||
|[RDS Management Servers](#BKMK_RDSManagementServers)|Yes|Yes|||
|[RDS Remote Access Servers](#BKMK_RDSRemoteAccessServers)|Yes|Yes|||
|[Read-only Domain Controllers](#BKMK_RODC)|Yes|Yes|Yes|Yes|
|[Remote Desktop Users](#BKMK_REmoteDesktopUsers)|Yes|Yes|Yes|Yes|
|[Remote Management Users](#BKMK_RemoteManagementUsers)|Yes|Yes|||
|[Replicator](#BKMK_Replicator)|Yes|Yes|Yes|Yes|
|[Schema Admins](#BKMK_SchemaAdmins)|Yes|Yes|Yes|Yes|
|[Server Operators](#BKMK_ServerOperators)|Yes|Yes|Yes|Yes|
|[Terminal Server License Servers](#BKMK_TerminalServerLic)|Yes|Yes|Yes|Yes|
|[Users](#BKMK_Users)|Yes|Yes|Yes|Yes|
|[Windows Authorization Access Group](#BKMK_WinAuthAccess)|Yes|Yes|Yes|Yes|
|[WinRMRemoteWMIUsers_](#BKMK_WinRMRemoteWMIUsers_)|Yes|Yes|||

### <a name="BKMK_ACAsstOps"></a>Access Control Assistance Operators
Members of this group can remotely query authorization attributes and permissions for resources on the computer.

The Access Control Assistance Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-579|
|Type|BuiltIn Local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_AccountOperators"></a>Account Operators
The Account Operators group grants limited account creation privileges to a user. Members of this group can create and modify most types of accounts, including those of users, local groups, and global groups, and members can log in locally to domain controllers.

Members of the Account Operators group cannot manage the Administrator user account, the user accounts of administrators, or the [Administrators](Active-Directory-Security-Groups.md#BKMK_Admins), [Server Operators](Active-Directory-Security-Groups.md#BKMK_ServerOperators), [Account Operators](Active-Directory-Security-Groups.md#BKMK_AccountOperators), [Backup Operators](Active-Directory-Security-Groups.md#BKMK_BackupOperators), or [Print Operators](Active-Directory-Security-Groups.md#BKMK_PrintOperators) groups. Members of this group cannot modify user rights.

The Account Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> By default, this built-in group has no members, and it can create and manage users and groups in the domain, including its own membership and that of the Server Operators group. This group is considered a service administrator group because it can modify Server Operators, which in turn can modify domain controller settings. As a best practice, leave the membership of this group empty, and do not use it for any delegated administration. This group cannot be renamed, deleted, or moved.

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-548|
|Type|BuiltIn Local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-locally.md):  SeInteractiveLogonRight|

### <a name="BKMK_Admins"></a>Administrators
Members of the Administrators group have complete and unrestricted access to the computer, or if the computer is promoted to a domain controller, members have unrestricted access to the domain.

The Administrators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> The Administrators group has built-in capabilities that give its members full control over the system. This group cannot be renamed, deleted, or moved. This built-in group controls access to all the domain controllers in its domain, and it can change the membership of all administrative groups.
> 
> Membership can be modified by members of the following groups: the default service Administrators, Domain Admins in the domain, or Enterprise Admins. This group has the special privilege to take ownership of any object in the directory or any resource on a domain controller. This account is considered a service administrator group because its members have full access to the domain controllers in the domain.

This security group includes the following changes since Windows Server 2008:

-   Default user rights changes: **Allow log on through Terminal Services** existed in Windows Server 2008, and it was replaced by [Allow log on through Remote Desktop Services](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-through-Remote-Desktop-Services.md).

-   [Remove computer from docking station](../group-managed-service-accounts/user-rights-assignment/Remove-computer-from-docking-station.md) was removed in  Windows Server 2012 R2 .

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-544|
|Type|BuiltIn Local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|Administrator, Domain Admins, Enterprise Admins|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Adjust memory quotas for a process](../group-managed-service-accounts/user-rights-assignment/Adjust-memory-quotas-for-a-process.md): SeIncreaseQuotaPrivilege<br /><br />[Access this computer from the network](Access-this-computer-from-the-network.md): SeNetworkLogonRight<br /><br />[Allow log on locally](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-locally.md): SeInteractiveLogonRight<br /><br />[Allow log on through Remote Desktop Services](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-through-Remote-Desktop-Services.md): SeRemoteInteractiveLogonRight<br /><br />[Back up files and directories](../group-managed-service-accounts/user-rights-assignment/Back-up-files-and-directories.md): SeBackupPrivilege<br /><br />[Bypass traverse checking](Bypass-traverse-checking.md): SeChangeNotifyPrivilege<br /><br />[Change the system time](../group-managed-service-accounts/user-rights-assignment/Change-the-system-time.md): SeSystemTimePrivilege<br /><br />[Change the time zone](../group-managed-service-accounts/user-rights-assignment/Change-the-time-zone.md): SeTimeZonePrivilege<br /><br />[Create a pagefile](../group-managed-service-accounts/user-rights-assignment/Create-a-pagefile.md): SeCreatePagefilePrivilege<br /><br />[Create global objects](../group-managed-service-accounts/user-rights-assignment/Create-global-objects.md): SeCreateGlobalPrivilege<br /><br />[Create symbolic links](../group-managed-service-accounts/user-rights-assignment/Create-symbolic-links.md): SeCreateSymbolicLinkPrivilege<br /><br />[Debug programs](../group-managed-service-accounts/user-rights-assignment/Debug-programs.md): SeDebugPrivilege<br /><br />[Enable computer and user accounts to be trusted for delegation](../group-managed-service-accounts/user-rights-assignment/Enable-computer-and-user-accounts-to-be-trusted-for-delegation.md): SeEnableDelegationPrivilege<br /><br />[Force shutdown from a remote system](../group-managed-service-accounts/user-rights-assignment/Force-shutdown-from-a-remote-system.md): SeRemoteShutdownPrivilege<br /><br />[Impersonate a client after authentication](../group-managed-service-accounts/user-rights-assignment/Impersonate-a-client-after-authentication.md): SeImpersonatePrivilege<br /><br />[Increase scheduling priority](../group-managed-service-accounts/user-rights-assignment/Increase-scheduling-priority.md): SeIncreaseBasePriorityPrivilege<br /><br />[Load and unload device drivers](../group-managed-service-accounts/user-rights-assignment/Load-and-unload-device-drivers.md): SeLoadDriverPrivilege<br /><br />[Log on as a batch job](../group-managed-service-accounts/user-rights-assignment/Log-on-as-a-batch-job.md): SeBatchLogonRight<br /><br />[Manage auditing and security log](../group-managed-service-accounts/user-rights-assignment/Manage-auditing-and-security-log.md): SeSecurityPrivilege<br /><br />[Modify firmware environment values](../group-managed-service-accounts/user-rights-assignment/Modify-firmware-environment-values.md): SeSystemEnvironmentPrivilege<br /><br />[Perform volume maintenance tasks](../group-managed-service-accounts/user-rights-assignment/Perform-volume-maintenance-tasks.md): SeManageVolumePrivilege<br /><br />[Profile system performance](../group-managed-service-accounts/user-rights-assignment/Profile-system-performance.md): SeSystemProfilePrivilege<br /><br />[Profile single process](../group-managed-service-accounts/user-rights-assignment/Profile-single-process.md): SeProfileSingleProcessPrivilege<br /><br />[Remove computer from docking station](../group-managed-service-accounts/user-rights-assignment/Remove-computer-from-docking-station.md): SeUndockPrivilege<br /><br />[Restore files and directories](../group-managed-service-accounts/user-rights-assignment/Restore-files-and-directories.md): SeRestorePrivilege<br /><br />[Shut down the system](../group-managed-service-accounts/user-rights-assignment/Shut-down-the-system.md): SeShutdownPrivilege<br /><br />[Take ownership of files or other objects](../group-managed-service-accounts/user-rights-assignment/Take-ownership-of-files-or-other-objects.md): SeTakeOwnershipPrivilege|

### <a name="BKMK_AllowedRODCPwdRepl"></a>Allowed RODC Password Replication Group
The purpose of this security group is to manage a RODC password replication policy. This group has no members by default, and it results in the condition that new Read-only domain controllers do not cache user credentials. The [Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl) group contains a variety of high-privilege accounts and security groups. The Denied RODC Password Replication group supersedes the Allowed RODC Password Replication group.

The Allowed RODC Password Replication group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-571|
|Type|Domain local|
|Default container|CN=Users DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_BackupOperators"></a>Backup Operators
Members of the Backup Operators group can back up and restore all files on a computer, regardless of the permissions that protect those files. Backup Operators also can log on to and shut down the computer. This group cannot be renamed, deleted, or moved. By default, this built-in group has no members, and it can perform backup and restore operations on domain controllers. Its membership can be modified by the following groups: default service Administrators, Domain Admins in the domain, or Enterprise Admins. It cannot modify the membership of any administrative groups. While members of this group cannot change server settings or modify the configuration of the directory, they do have the permissions needed to replace files (including operating system files) on domain controllers. Because of this, members of this group are considered service administrators.

The Backup Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-551|
|Type|Builtin local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-locally.md): SeInteractiveLogonRight<br /><br />[Back up files and directories](../group-managed-service-accounts/user-rights-assignment/Back-up-files-and-directories.md): SeBackupPrivilege<br /><br />[Log on as a batch job](../group-managed-service-accounts/user-rights-assignment/Log-on-as-a-batch-job.md): SeBatchLogonRight<br /><br />[Restore files and directories](../group-managed-service-accounts/user-rights-assignment/Restore-files-and-directories.md): SeRestorePrivilege<br /><br />[Shut down the system](../group-managed-service-accounts/user-rights-assignment/Shut-down-the-system.md): SeShutdownPrivilege|

### <a name="BKMK_CertificateServiceDCOMAccess"></a>Certificate Service DCOM Access
Members of this group are allowed to connect to certification authorities in the enterprise.

The Certificate Service DCOM Access group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-<domain>-574|
|Type|Domain Local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_CertPublishers"></a>Cert Publishers
Members of the Cert Publishers group are authorized to publish certificates for User objects in Active Directory.

The Cert Publishers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-517|
|Type|Domain Local|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|[Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### <a name="BKMK_CloneableDomainControllers"></a>Cloneable Domain Controllers
Members of the Cloneable Domain Controllers group that are domain controllers may be cloned. In  Windows Server 2012 R2  and  Windows Server 2012 , you can deploy domain controllers by copying an existing virtual domain controller. In a virtual environment, you no longer have to repeatedly deploy a server image that is prepared by using sysprep.exe, promote the server to a domain controller, and then complete additional configuration requirements for deploying each domain controller (including adding the virtual domain controller to this security group).

For more information, see [Introduction to Active Directory Domain Services &#40;AD DS&#41; Virtualization &#40;Level 100&#41;](../../identity/ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-522|
|Type|Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_CryptographicOperators"></a>Cryptographic Operators
Members of this group are authorized to perform cryptographic operations. This security group was added in Windows Vista Service Pack 1 (SP1) to configure Windows Firewall for IPsec in Common Criteria mode.

The Cryptographic Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group was introduced in Windows Vista Service Pack 1, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-569|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_DeniedRODCPwdRepl"></a>Denied RODC Password Replication Group
Members of the Denied RODC Password Replication group cannot have their passwords replicated to any Read-only domain controller.

The purpose of this security group is to manage a RODC password replication policy. This group contains a variety of high-privilege accounts and security groups. The Denied RODC Password Replication Group supersedes the [Allowed RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_AllowedRODCPwdRepl).

This security group includes the following changes since Windows Server 2008:

-   Windows Server 2012 changed the default members to include [Cert Publishers](Active-Directory-Security-Groups.md#BKMK_CertPublishers).

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-572|
|Type|Domain local|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|[Cert Publishers](Active-Directory-Security-Groups.md#BKMK_CertPublishers)<br /><br />[Domain Admins](Active-Directory-Security-Groups.md#BKMK_DomainAdmins)<br /><br />[Domain Controllers](Active-Directory-Security-Groups.md#BKMK_DomainControllers)<br /><br />[Enterprise Admins](Active-Directory-Security-Groups.md#BKMK_EntAdmins)<br /><br />Group Policy Creator Owners<br /><br />krbtgt<br /><br />[Read-only Domain Controllers](Active-Directory-Security-Groups.md#BKMK_RODC)<br /><br />[Schema Admins](Active-Directory-Security-Groups.md#BKMK_SchemaAdmins)|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?||
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_DistributedCOMUsers"></a>Distributed COM Users
Members of the Distributed COM Users group are allowed to launch, activate, and use Distributed COM objects on the computer. Microsoft Component Object Model (COM) is a platform-independent, distributed, object-oriented system for creating binary software components that can interact. Distributed Component Object Model (DCOM) allows applications to be distributed across locations that make the most sense to you and to the application. This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Distributed COM Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-562|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_DnsUpdateProxy"></a>DnsUpdateProxy
Members of the DnsUpdateProxy group are DNS clients. They are permitted to perform dynamic updates on behalf of other clients (such as DHCP servers). A DNS server can develop stale resource records when a DHCP server is configured to dynamically register host (A) and pointer (PTR) resource records on behalf of DHCP clients by using dynamic update. Adding clients to this security group mitigates this scenario.

However, to protect against unsecured records or to permit members of the DnsUpdateProxy group to register records in zones that allow only secured dynamic updates, you must create a dedicated user account and configure DHCP servers to perform DNS dynamic updates by using the credentials of this account (user name, password, and domain). Multiple DHCP servers can use the credentials of one dedicated user account.

For information, see [DNS Record Ownership and the DnsUpdateProxy Group](http://technet.microsoft.com/library/dd334715(v=WS.10.aspx.

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-1103|
|Type|Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_DnsAdmins"></a>DnsAdmins
Members of DNSAdmins group have access to network DNS information. The default permissions are as follows: Allow: Read, Write, Create All Child objects, Delete Child objects, Special Permissions.

For information about other means to secure the DNS server service, see [Securing the DNS Server Service](http://technet.microsoft.com/library/cc731367.aspx).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-1102|
|Type|Domain local|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_DomainAdmins"></a>Domain Admins
Members of the Domain Admins security group are authorized to administer the domain. By default, the Domain Admins group is a member of the Administrators group on all computers that have joined a domain, including the domain controllers. The Domain Admins group is the default owner of any object that is created in Active Directory for the domain by any member of the group. If members of the group create other objects, such as files, the default owner is the Administrators group.

The Domain Admins group controls access to all domain controllers in a domain, and it can modify the membership of all administrative accounts in the domain. Membership can be modified by members of the service administrator groups in its domain (Administrators and Domain Admins), and by members of the Enterprise Admins group. This is considered a service administrator account because its members have full access to the domain controllers in a domain.

The Domain Admins group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-512|
|Type|Domain Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Administrator|
|Default member of|[Administrators](Active-Directory-Security-Groups.md#BKMK_Admins)<br /><br />[Denied RODC Password ReplicationGroup](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Administrators](Active-Directory-Security-Groups.md#BKMK_Admins)<br /><br />See [Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|

### <a name="BKMK_DomainComputers"></a>Domain Computers
This group can include all computers and servers that have joined the domain, excluding domain controllers. By default, any computer account that is created automatically becomes a member of this group.

The Domain Computers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-515|
|Type|Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|All computers joined to the domain, excluding domain controllers|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes (but not required)|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### <a name="BKMK_DomainControllers"></a>Domain Controllers
The Domain Controllers group can include all domain controllers in the domain. New domain controllers are automatically added to this group.

The Domain Controllers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-516|
|Type|Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Computer accounts for all domain controllers of the domain|
|Default member of|[Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|No|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### <a name="BKMK_DomainGuests"></a>Domain Guests
The Domain Guests group includes the domain???s built-in Guest account. When members of this group sign in as local guests on a domain-joined computer, a domain profile is created on the local computer.

The Domain Guests group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-514|
|Type|Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Guest|
|Default member of|[Guests](Active-Directory-Security-Groups.md#BKMK_Guests)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Can be moved out but it is not recommended|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Guests](Active-Directory-Security-Groups.md#BKMK_Guests)|

### <a name="BKMK_DomainUsers"></a>Domain Users
The Domain Users group includes all user accounts in a domain. When you create a user account in a domain, it is automatically added to this group.

By default, any user account that is created in the domain automatically becomes a member of this group. This group can be used to represent all users in the domain. For example, if you want all domain users to have access to a printer, you can assign permissions for the printer to this group (or add the Domain Users group to a local group on the print server that has permissions for the printer).

The Domain Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-513|
|Type|Domain Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Administrator<br /><br />krbtgt|
|Default member of|[Users](Active-Directory-Security-Groups.md#BKMK_Users)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Users](Active-Directory-Security-Groups.md#BKMK_Users)|

### <a name="BKMK_EntAdmins"></a>Enterprise Admins
The Enterprise Admins group exists only in the root domain of an Active Directory forest of domains. It is a Universal group if the domain is in native mode; it is a Global group if the domain is in mixed mode. Members of this group are authorized to make forest-wide changes in Active Directory, such as adding child domains.

By default, the only member of the group is the Administrator account for the forest root domain. This group is automatically added to the Administrators group in every domain in the forest, and it provides complete access for configuring all domain controllers. Members in this group can modify the membership of all administrative groups. Membership can be modified only by the default service administrator groups in the root domain. This is considered a service administrator account.

The Enterprise Admins group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<root domain>-519|
|Type|Universal (if Domain is in Native-Mode) else Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Administrator|
|Default member of|[Administrators](Active-Directory-Security-Groups.md#BKMK_Admins)<br /><br />[Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Administrators](Active-Directory-Security-Groups.md#BKMK_Admins)<br /><br />See [Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|

### <a name="BKMK_EntRODC"></a>Enterprise Read-Only Domain Controllers
Members of this group are Read-Only Domain Controllers in the enterprise. Except for account passwords, a Read-only domain controller holds all the Active Directory objects and attributes that a writable domain controller holds. However, changes cannot be made to the database that is stored on the Read-only domain controller. Changes must be made on a writable domain controller and then replicated to the Read-only domain controller.

Read-only domain controllers address some of the issues that are commonly found in branch offices. These locations might not have a domain controller. Or, they might have a writable domain controller, but not the physical security, network bandwidth, or local expertise to support it.

For more information, see [AD DS: Read-Only Domain Controllers](http://technet.microsoft.com/library/cc732801(v=WS.10.aspx.

The Enterprise Read-Only Domain Controllers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-498|
|Type|Universal|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?||
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_EventLogReaders"></a>Event Log Readers
Members of this group can read event logs from local computers. The group is created when the server is promoted to a domain controller.

The Event Log Readers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-573|
|Type|Builtin local|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_GPCreatorsOwners"></a>Group Policy Creators Owners
This group is authorized to create, edit, or delete Group Policy Objects in the domain. By default, the only member of the group is Administrator.

For information about other features you can use with this security group, see [Group Policy Planning and Deployment Guide](http://technet.microsoft.com/library/cc754948(v=WS.10.aspx.

The Group Policy Creators Owners group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<domain>-520|
|Type|Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Administrator|
|Default member of|[Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|No|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|

### <a name="BKMK_Guests"></a>Guests
Members of the Guests group have the same access as members of the Users group by default, except that the Guest account has further restrictions. By default, the only member is the Guest account. The Guests group allows occasional or one-time users to sign in with limited privileges to a computer???s built-in Guest account.

When a member of the Guests group signs out, the entire profile is deleted. This includes everything that is stored in the **%userprofile%** directory, including the user's registry hive information, custom desktop icons, and other user-specific settings. This implies that a guest must use a temporary profile to sign in to the system. This security group interacts with the Group Policy setting **Do not logon users with temporary profiles** when it is enabled. This setting is located under the following path:

Computer Configuration\Administrative Templates\System\User Profiles

> [!NOTE]
> A Guest account is a default member of the Guests security group. People who do not have an actual account in the domain can use the Guest account. A user whose account is disabled (but not deleted) can also use the Guest account.
> 
> The Guest account does not require a password. You can set rights and permissions for the Guest account as in any user account. By default, the Guest account is a member of the built-in Guests group and the Domain Guests global group, which allows a user to sign in to a domain. The Guest account is disabled by default, and we recommend that it stay disabled.

The Guests group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-546|
|Type|Builtin Local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|Guest|
|Default member of|[Domain Guests](Active-Directory-Security-Groups.md#BKMK_DomainGuests)<br /><br />Guest|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### <a name="BKMK_HyperVAdministrators"></a>Hyper-V Administrators
Members of the Hyper-V Administrators group have complete and unrestricted access to all the features in Hyper-V. Adding members to this group helps reduce the number of members required in the Administrators group, and further separates access.

> [!NOTE]
> Prior to Windows Server 2012, access to features in Hyper-V was controlled in part by membership in the Administrators group.

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-578|
|Type|Builtin local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|None|
|Default member of|No|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_IIS_IUSRS"></a>IIS_IUSRS
IIS_IUSRS is a built-in group that is used by Internet Information Services beginning with IIS 7.0. A built-in account and group are guaranteed by the operating system to always have a unique SID. IIS 7.0 replaces the IUSR_MachineName account and the IIS_WPG group with the IIS_IUSRS group to ensure that the actual names that are used by the new account and group will never be localized. For example, regardless of the language of the Windows operating system that you install, the IIS account name will always be IUSR, and the group name will be IIS_IUSRS.

For more information, see [Understanding Built-In User and Group Accounts in IIS 7](http://www.iis.net/leaplanning-for-security/understanding-built-in-user-and-group-accounts-in-iis).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-568|
|Type|BuiltIn Local|
|Default container|CN=BuiltIn, DC=<domain>, DC=|
|Default members|IUSR|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?||
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_InForestTrustBldrs"></a>Incoming Forest Trust Builders
Members of the Incoming Forest Trust Builders group can create incoming, one-way trusts to this forest. Active Directory provides security across multiple domains or forests through domain and forest trust relationships. Before authentication can occur across trusts, Windows must determine whether the domain being requested by a user, computer, or service has a trust relationship with the logon domain of the requesting account.

To make this determination, the Windows security system computes a trust path between the domain controller for the server that receives the request and a domain controller in the domain of the requesting account. A secured channel extends to other Active Directory domains through interdomain trust relationships. This secured channel is used to obtain and verify security information, including security identifiers (SIDs) for users and groups.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller  and it holds the operations master role (also known as flexible single master operations or FSMO).

For more information, see [How Domain and Forest Trusts Work: Domain and Forest Trusts](http://technet.microsoft.com/library/f5c70774-25cd-4481-8b7a-3d65c86e69b1).

The Incoming Forest Trust Builders group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-557|
|Type|BuiltIn local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### <a name="BKMK_NetworkCfgOperators"></a>Network Configuration Operators
Members of the Network Configuration Operators group can have the following administrative privileges to manage configuration of networking features:

-   Modify the Transmission Control Protocol/Internet Protocol (TCP/IP) properties for a local area network (LAN) connection, which includes the IP address, the subnet mask, the default gateway, and the name servers.

-   Rename the LAN connections or remote access connections that are available to all the users.

-   Enable or disable a LAN connection.

-   Modify the properties of all of remote access connections of users.

-   Delete all the remote access connections of users.

-   Rename all the remote access connections of users.

-   Issue ipconfig, ipconfig /release, or ipconfig /renew commands.

-   Enter the PIN unblock key (PUK) for mobile broadband devices that support a SIM card.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Network Configuration Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-556|
|Type|BuiltIn local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### <a name="BKMK_PerfLogUsers"></a>Performance Log Users
Members of the Performance Log Users group can manage performance counters, logs, and alerts locally on the server and from remote clients without being a member of the Administrators group. Specifically, members of this security group:

-   Can use all the features that are available to the Performance Monitor Users group.

-   Can create and modify Data Collector Sets after the group is assigned the [Log on as a batch job](../group-managed-service-accounts/user-rights-assignment/Log-on-as-a-batch-job.md) user right.

    > [!WARNING]
    > If you are a member of the Performance Log Users group, you must configure Data Collector Sets that you create to run under your credentials.

-   Cannot use the Windows Kernel Trace event provider in Data Collector Sets.

For members of the Performance Log Users group to initiate data logging or modify Data Collector Sets, the group must first be assigned the [Log on as a batch job](../group-managed-service-accounts/user-rights-assignment/Log-on-as-a-batch-job.md) user right. To assign this user right, use the Local Security Policy snap-in in Microsoft Management Console.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Performance Log Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> This account cannot be renamed, deleted, or moved.

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-559|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|[Log on as a batch job](../group-managed-service-accounts/user-rights-assignment/Log-on-as-a-batch-job.md): SeBatchLogonRight|

### <a name="BKMK_PerfMonitorUsers"></a>Performance Monitor Users
Members of this group can monitor performance counters on domain controllers in the domain, locally and from remote clients, without being a member of the Administrators or Performance Log Users groups. The Windows Performance Monitor is a Microsoft Management Console (MMC) snap-in that provides tools for analyzing system performance. From a single console, you can monitor application and hardware performance, customize what data you want to collect in logs, define thresholds for alerts and automatic actions, generate reports, and view past performance data in a variety of ways.

Specifically, members of this security group:

-   Can use all the features that are available to the Users group.

-   Can view real-time performance data in Performance Monitor.

    Can change the Performance Monitor display properties while viewing data.

-   Cannot create or modify Data Collector Sets.

    > [!WARNING]
    > You cannot configure a Data Collector Set to run as a member of the Performance Monitor Users group.

> [!NOTE]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO). This group cannot be renamed, deleted, or moved.

The Performance Monitor Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-558|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### <a name="BKMK_Pre-WS2KcompatAccess"></a>Pre???Windows 2000 Compatible Access
Members of the Pre???Windows 2000 Compatible Access group have Read access for all users and groups in the domain. This group is provided for backward compatibility for computers running Windows NT 4.0 and earlier. By default, the special identity group, Everyone, is a member of this group. Add users to this group only if they are running Windows NT 4.0 or earlier.

> [!WARNING]
> This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Pre???Windows 2000 Compatible Access group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-554|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|If you choose the Pre???Windows 2000 Compatible Permissions mode, Everyone and Anonymous are members, and if you choose the Windows 2000-only permissions mode, Authenticated Users are members.|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Access this computer from the network](Access-this-computer-from-the-network.md): SeNetworkLogonRight<br /><br />[Bypass traverse checking](Bypass-traverse-checking.md): SeChangeNotifyPrivilege|

### <a name="BKMK_PrintOperators"></a>Print Operators
Members of this group can manage, create, share, and delete printers that are connected to domain controllers in the domain. They can also manage Active Directory printer objects in the domain. Members of this group can locally sign in to and shut down domain controllers in the domain.

This group has no default members. Because members of this group can load and unload device drivers on all domain controllers in the domain, add users with caution. This group cannot be renamed, deleted, or moved.

The Print Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008. However, in Windows Server 2008 R2, functionality was added to manage print administration. For more information, see [Assigning Delegated Print Administrator and Printer Permission Settings in Windows Server 2008 R2](http://technet.microsoft.com/library/ee524015(WS.10).aspx).

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-550|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-locally.md): SeInteractiveLogonRight<br /><br />[Load and unload device drivers](../group-managed-service-accounts/user-rights-assignment/Load-and-unload-device-drivers.md):  SeLoadDriverPrivilege<br /><br />[Shut down the system](../group-managed-service-accounts/user-rights-assignment/Shut-down-the-system.md):  SeShutdownPrivilege|

### <a name="BKMK_ProtectedUsers"></a>Protected Users
Members of the Protected Users group are afforded additional protection against the compromise of credentials during authentication processes.

This security group is designed as part of a strategy to effectively protect and manage credentials within the enterprise. Members of this group automatically have non-configurable protection applied to their accounts. Membership in the Protected Users group is meant to be restrictive and proactively secure by default. The only method to modify the protection for an account is to remove the account from the security group.

This domain-related, global group triggers non-configurable protection on devices and host computers running  Windows Server 2012 R2  and Windows 8.1, and on domain controllers in domains with a primary domain controller running  Windows Server 2012 R2 . This greatly reduces the memory footprint of credentials when users sign in to computers on the network from a non-compromised computer.

Depending on the account???s domain functional level, members of the Protected Users group are further protected due to behavior changes in the authentication methods that are supported in Windows.

-   Members of the Protected Users group cannot authenticate by using the following Security Support Providers (SSPs): NTLM, Digest Authentication, or CredSSP. Passwords are not cached on a device running Windows 8.1,  so the device fails to authenticate to a domain when the account is a member of the Protected User group.

-   The Kerberos protocol will not use the weaker DES or RC4 encryption types in the preauthentication process. This means that the domain must be configured to support at least the AES cipher suite.

-   The user???s account cannot be delegated with Kerberos constrained or unconstrained delegation. This means that former connections to other systems may fail if the user is a member of the Protected Users group.

-   The default Kerberos ticket-granting tickets (TGTs) lifetime setting of four hours is configurable by using Authentication Policies and Silos, which can be accessed through the Active Directory Administrative Center. This means that when four hours has passed, the user must authenticate again.

The Protected Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This group was introduced in  Windows Server 2012 R2 . For more information about how this group works, see [Protected Users Security Group](../credentials-protection-and-management/Protected-Users-Security-Group.md).

The following table specifies the properties of the Protected Users group.

|Attribute|Value|
|-------|-----|
|Well-known SID/RID|S-1-5-21-<domain>-525|
|Type|Domain Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-service admins?|No|
|Default user rights|None|

### <a name="BKMK_RASandIAS"></a>RAS and IAS Servers
Computers that are members of the RAS and IAS Servers group, when properly configured, are allowed to use remote access services. By default, this group has no members. Computers that are running the Routing and Remote Access service are added to the group automatically, such as IAS servers and Network Policy Servers. Members of this group have access to certain properties of User objects, such as Read Account Restrictions, Read Logon Information, and Read Remote Access Information.

The RAS and IAS Servers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-553|
|Type|Domain local|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### <a name="BKMK_RDSEndpointServers"></a>RDS Endpoint Servers
Servers that are members in the RDS Endpoint Servers group can run virtual machines and host sessions where user RemoteApp programs and personal virtual desktops run. This group needs to be populated on servers running RD Connection Broker. Session Host servers and RD Virtualization Host servers used in the deployment need to be in this group.

For information about Remote Desktop Services, see [Remote Desktop Services Design Guide](http://technet.microsoft.com/library/gg750997(v=WS.10.aspx.

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-553|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_RDSManagementServers"></a>RDS Management Servers
Servers that are members in the RDS Management Servers group can be used to perform routine administrative actions on servers running Remote Desktop Services. This group needs to be populated on all servers in a Remote Desktop Services deployment. The servers running the RDS Central Management service must be included in this group.

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-577|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_RDSRemoteAccessServers"></a>RDS Remote Access Servers
Servers in the RDS Remote Access Servers group provide users with access to RemoteApp programs and personal virtual desktops. In Internet facing deployments, these servers are typically deployed in an edge network. This group needs to be populated on servers running RD Connection Broker. RD Gateway servers and RD Web Access servers that are used in the deployment need to be in this group.

For information about RemoteApp programs, see [Overview of RemoteApp](http://technet.microsoft.com/library/cc755055.aspx)

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-575|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_REmoteDesktopUsers"></a>Remote Desktop Users
The Remote Desktop Users group on an RD Session Host server is used to grant users and groups permissions to remotely connect to an RD Session Host server. This group cannot be renamed, deleted, or moved. It appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Remote Desktop Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-555|
|Type|Builtin Local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### <a name="BKMK_RODC"></a>Read-Only Domain Controllers
This group is comprised of the Read-only domain controllers in the domain. A Read-only domain controller makes it possible for organizations to easily deploy a domain controller in scenarios where physical security cannot be guaranteed, such as branch office locations, or in scenarios where local storage of all domain passwords is considered a primary threat, such as in an extranet or in an application-facing role.

Because administration of a Read-only domain controller can be delegated to a domain user or security group, an Read-only domain controller is well suited for a site that should not have a user who is a member of the Domain Admins group. A Read-only domain controller encompasses the following functionality:

-   Read-only AD DS database

-   Unidirectional replication

-   Credential caching

-   Administrator role separation

-   Read-only Domain Name System (DNS)

For information about deploying a Read-only domain controller, see [Read-Only Domain Controllers Step-by-Step Guide](http://technet.microsoft.com/library/cc772234(v=WS.10.aspx.

This security group was introduced in Windows Server 2008, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-521|
|Type||
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|[Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|See [Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|

### <a name="BKMK_RemoteManagementUsers"></a>Remote Management Users
Members of the Remote Management Users group can access WMI resources over management protocols (such as WS-Management via the Windows Remote Management service). This applies only to WMI namespaces that grant access to the user.

The Remote Management Users group is generally used to allow users to manage servers through the Server Manager console, whereas the [WinRMRemoteWMIUsers_](Active-Directory-Security-Groups.md#BKMK_WinRMRemoteWMIUsers_) group is allows remotely running Windows PowerShell commands.

For more information, see [WS-Management Protocol (Windows)](http://msdn.microsoft.com/library/aa384470(v=vs.85).aspx) and [About WMI (Windows)](http://msdn.microsoft.com/library/aa384642(v=vs.85).aspx).

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-580|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_Replicator"></a>Replicator
Computers that are members of the Replicator group support file replication in a domain. Windows Server operating systems use the File Replication service (FRS) to replicate system policies and logon scripts stored in the System Volume (SYSVOL). Each domain controller keeps a copy of SYSVOL for network clients to access. FRS can also replicate data for the Distributed File System (DFS), synchronizing the content of each member in a replica set as defined by DFS. FRS can copy and maintain shared files and folders on multiple servers simultaneously. When changes occur, content is synchronized immediately within sites and by a schedule between sites.

> [!IMPORTANT]
> In Windows Server 2008 R2, FRS cannot be used for replicating DFS folders or custom (non-SYSVOL) data. A Windows Server 2008 R2 domain controller can still use FRS to replicate the contents of a SYSVOL shared resource in a domain that uses FRS for replicating the SYSVOL shared resource between domain controllers.
> 
> However, Windows Server 2008 R2 servers cannot use FRS to replicate the contents of any replica set apart from the SYSVOL shared resource. The DFS Replication service is a replacement for FRS, and it can be used to replicate the contents of a SYSVOL shared resource, DFS folders, and other custom (non-SYSVOL) data. You should migrate all non-SYSVOL FRS replica sets to DFS Replication. For more information, see [File Replication Service (FRS) Is Deprecated in Windows Server 2008 R2 (Windows).](http://msdn.microsoft.com/library/windows/desktop/ff384840(v=vs.85).aspx)

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-552|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

### <a name="BKMK_SchemaAdmins"></a>Schema Admins
Members of the Schema Admins group can modify the Active Directory schema. This group exists only in the root domain of an Active Directory forest of domains. It is a Universal group if the domain is in native mode; it is a Global group if the domain is in mixed mode.

The group is authorized to make schema changes in Active Directory. By default, the only member of the group is the Administrator account for the forest root domain. This group has full administrative access to the schema.

The membership of this group can be modified by any of the service administrator groups in the root domain. This is considered a service administrator account because its members can modify the schema, which governs the structure and content of the entire directory.

For more information, see [What Is the Active Directory Schema?: Active Directory](http://technet.microsoft.com/library/cc784826(v=WS.10.aspx.

The Schema Admins group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-<root domain>-518|
|Type|Universal (if Domain is in Native-Mode) else Global|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|Administrator|
|Default member of|[Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|See [Denied RODC Password Replication Group](Active-Directory-Security-Groups.md#BKMK_DeniedRODCPwdRepl)|

### <a name="BKMK_ServerOperators"></a>Server Operators
Members in the Server Operators group can administer domain servers. This group exists only on domain controllers. By default, the group has no members. Memebers of the Server Operators group can sign in to a server interactively, create and delete network shared resources, start and stop services, back up and restore files, format the hard disk drive of the computer, and shut down the computer. This group cannot be renamed, deleted, or moved.

By default, this built-in group has no members, and it has access to server configuration options on domain controllers. Its membership is controlled by the service administrator groups, Administrators and Domain Admins, in the domain, and the Enterprise Admins group. Members in this group cannot change any administrative group memberships. This is considered a service administrator account because its members have physical access to domain controllers, they can perform maintenance tasks (such as backup and restore), and they have the ability to change binaries that are installed on the domain controllers. Note the default user rights in the following table.

The Server Operators group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-549|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|Yes|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|[Allow log on locally](../group-managed-service-accounts/user-rights-assignment/Allow-log-on-locally.md): SeInteractiveLogonRight<br /><br />[Back up files and directories](../group-managed-service-accounts/user-rights-assignment/Back-up-files-and-directories.md): SeBackupPrivilege<br /><br />[Change the system time](../group-managed-service-accounts/user-rights-assignment/Change-the-system-time.md): SeSystemTimePrivilege<br /><br />[Change the time zone](../group-managed-service-accounts/user-rights-assignment/Change-the-time-zone.md): SeTimeZonePrivilege<br /><br />[Force shutdown from a remote system](../group-managed-service-accounts/user-rights-assignment/Force-shutdown-from-a-remote-system.md): SeRemoteShutdownPrivilege<br /><br />[Restore files and directories](../group-managed-service-accounts/user-rights-assignment/Restore-files-and-directories.md): Restore files and directories SeRestorePrivilege<br /><br />[Shut down the system](../group-managed-service-accounts/user-rights-assignment/Shut-down-the-system.md): SeShutdownPrivilege|

### <a name="BKMK_TerminalServerLic"></a>Terminal Server License Servers
Members of the Terminal Server License Servers group can update user accounts in Active Directory with information about license issuance. This is used to track and report TS Per User CAL usage. A TS Per User CAL gives one user the right to access a Terminal Server from an unlimited number of client computers or devices. This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

For more information about this security group, see [Terminal Services License Server Security Group Configuration](http://technet.microsoft.com/library/cc775331(v=WS.10.aspx.

The Terminal Server License Servers group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

This security group only applies to Windows Server 2003 and Windows Server 2008 because Terminal Services was replaced by Remote Desktop Services in Windows Server 2008 R2.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-561|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Safe to move out of default container?|Cannot be moved|
|Protected by ADMINSDHOLDER?|No|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default User Rights|None|

### <a name="BKMK_Users"></a>Users
Members of the Users group are prevented from making accidental or intentional system-wide changes, and they can run most applications. After the initial installation of the operating system, the only member is the Authenticated Users group. When a computer joins a domain, the Domain Users group is added to the Users group on the computer.

Users can perform tasks such as running applications, using local and network printers, shutting down the computer, and locking the computer. Users can install applications that only they are allowed to use if the installation program of the application supports per-user installation. This group cannot be renamed, deleted, or moved.

The Users group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

This security group includes the following changes since Windows Server 2008:

-   In Windows Server 2008 R2, INTERACTIVE was added to the default members list.

-   In Windows Server 2012, the default **Member Of** list changed from Domain Users to none.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-545|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|Authenticated Users<br /><br />[Domain Users](Active-Directory-Security-Groups.md#BKMK_DomainUsers)<br /><br />INTERACTIVE|
|Default member of|Domain Users (this membership is due to the fact that the Primary Group ID of all user accounts is Domain Users.)|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|No|
|Default User Rights|None|

### <a name="BKMK_WinAuthAccess"></a>Windows Authorization Access Group
Members of this group have access to the computed token GroupsGlobalAndUniversal attribute on User objects. Some applications have features that read the token-groups-global-and-universal (TGGAU) attribute on user account objects or on computer account objects in Active Directory Domain Services. Some Win32 functions make it easier to read the TGGAU attribute. Applications that read this attribute or that call an API (referred to as a function) that reads this attribute do not succeed if the calling security context does not have access to the attribute. This group appears as a SID until the domain controller is made the primary domain controller and it holds the operations master role (also known as flexible single master operations or FSMO).

The Windows Authorization Access group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

> [!NOTE]
> This group cannot be renamed, deleted, or moved.

This security group has not changed since Windows Server 2008.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-32-560|
|Type|Builtin local|
|Default container|CN=Builtin, DC=<domain>, DC=|
|Default members|Enterprise Domain Controllers|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Cannot be moved|
|Safe to delegate management of this group to non-Service admins?|Yes|
|Default user rights|None|

### <a name="BKMK_WinRMRemoteWMIUsers_"></a>WinRMRemoteWMIUsers_
In Windows 8 and in Windows Server 2012, a **Share** tab was added to the Advanced Security Settings user interface. This tab displays the security properties of a remote file share. To view this information, you must have the following permissions and memberships, as appropriate for the version of Windows Server that the file server is running.

The WinRMRemoteWMIUsers_ group applies to versions of the Windows Server operating system listed in the [Active Directory Default Security Groups table](#BKMK_GroupsTable).

-   If the file share is hosted on a server that is running a supported version of the operating system:

    -   You must be a member of the WinRMRemoteWMIUsers__ group or the BUILTIN\Administrators group.

    -   You must have Read permissions to the file share.

-   If the file share is hosted on a server that is running a version of Windows Server that is earlier than Windows Server 2012:

    -   You must be a member of the BUILTIN\Administrators group.

    -   You must have Read permissions to the file share.

In Windows Server 2012, the Access Denied Assistance functionality adds the Authenticated Users group to the local WinRMRemoteWMIUsers__ group. Therefore, when the Access Denied Assistance functionality is enabled, all authenticated users who have Read permissions to the file share can view the file share permissions.

> [!NOTE]
> The WinRMRemoteWMIUsers_ group allows running Windows PowerShell commands remotely whereas the [Remote Management Users](Active-Directory-Security-Groups.md#BKMK_RemoteManagementUsers) group is generally used to allow users to manage servers by using the Server Manager console.

This security group was introduced in Windows Server 2012, and it has not changed in subsequent versions.

|Attribute|Value|
|-------|-----|
|Well-Known SID/RID|S-1-5-21-<domain>-1000|
|Type|Domain local|
|Default container|CN=Users, DC=<domain>, DC=|
|Default members|None|
|Default member of|None|
|Protected by ADMINSDHOLDER?|No|
|Safe to move out of default container?|Yes|
|Safe to delegate management of this group to non-Service admins?||
|Default User Rights|None|

## See also
[Security Principals Technical Overview](Security-Principals-Technical-Overview.md)

[Special Identities](Special-Identities.md)

