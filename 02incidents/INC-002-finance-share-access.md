# INC-002 — Finance Share Access Denied

**Category:** File & Access  
**Priority:** Medium  
**User:** David Smith  
**Resource:** `\\TRIIUMPH-DC-01\Finance`  
**Security Group:** `TRIIUMPH\GG-Finance-Users`  
**Status:** Resolved

## Issue

David Smith reported that he could not access the Finance network share: 

```text
\\TRIIUMPH-DC-01\Finance
```
The objective was to determine whether the issue was related to the user's account, group membership, or the permissions assigned to the 
finance resource.

## Investigation

### 1. Confirmed the user's identity

I first confirmed that the affected Windows session belonged to David Smith using:

```cmd
whoami
```

This confirmed the user account being investigated. 


### 2. Checked the Finance Folder Permissions

Rather than assuming which security group controlled access, I first inspected the permissions assigned to the Finance folder on the Domain Controller:

```cmd
icacls "C:\Shares\Finance"
```
The output showed:

```text
TRIIUMPH\GG-Finance-Users:(OI)(CI)(F)
```

This established that the GG-Finance-Users security group had permissions on the Finance folder. The group was therefore identified as the security group that should provide access to the resource.

### 3. Checked the User's Security Group Membership
After identifying the group controlling access to the Finance folder, I checked David's current security token:

```cmd
whoami /groups
```

The `GG-Finance-Users` was not present.

This indicated that David did not currently have the group membership required for the Finance resource.


## Root Cause

David Smith was not a member of the GG-Finance-Users security group.

The Finance folder was configured to grant access through that security group, so David’s account did not have the required authorization.

## Remediation

On the Domain Controller, I opened **Server Manager** and navigated to the Active Directory group management settings.

I located the **Finance security group** and added **David.Smith** back as a group member.

After restoring the group membership, David signed out of the Windows 11 client and signed back in to refresh his domain security token.

## Validation

After signing back in, David accessed:

```text
\\TRIIUMPH-DC-01\Finance
```

The Finance folder opened successfully and access was restored.

This confirmed that the issue was caused by the user’s missing Finance security group membership rather than a problem with the network share itself.

## Technical Concept Demonstrated

This incident demonstrates the difference between:

**Authentication**
> Can the user prove who they are?

and

**Authorization**
> Does the user's security token contain the identity/group required by the resource ACL?

The user could authenticate successfully but was not authorized to access the Finance resource until group membership was restored.

## Help Desk Skills Demonstrated

* User identity verification
* NTFS permission investigation
* Active Directory security group troubleshooting
* Windows security-token troubleshooting
* Access-control troubleshooting
* Root-cause analysis
* User/group administration
* Validation after remembering
