## Access Control List (ACL) Overview
1. `Discretionary Access Control List` (`DACL`) - defines which security principals are granted or denied access to an object. DACLs are made up of ACEs that either allow or deny access. When someone attempts to access an object, the system will check the DACL for the level of access that is permitted. If a DACL does not exist for an object, all who attempt to access the object are granted full rights. If a DACL exists, but does not have any ACE entries specifying specific security settings, the system will deny access to all users, groups, or processes attempting to access it.
2. `System Access Control Lists` (`SACL`) - allow administrators to log access attempts made to secured objects.
#### Viewing forend's ACL
![](Pasted%20image%2020260528154505.png)
#### Viewing the SACLs through the Auditing Tab
![](Pasted%20image%2020260528154541.png)
## Access Control Entries (ACEs)
|**ACE**|**Description**|
|---|---|
|`Access denied ACE`|Used within a DACL to show that a user or group is explicitly denied access to an object|
|`Access allowed ACE`|Used within a DACL to show that a user or group is explicitly granted access to an object|
|`System audit ACE`|Used within a SACL to generate audit logs when a user or group attempts to access an object. It records whether access was granted or not and what type of access occurred|
#### Viewing Permissions through Active Directory Users & Computers
![](Pasted%20image%2020260528154826.png)
## Why are ACEs Important?
![](Pasted%20image%2020260528155217.png)
