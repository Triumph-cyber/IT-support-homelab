# INC-003 — New Employee Account Provisioning

**Category:** User Administration / Access  
**Priority:** Medium  
**User:** Andy Johnson  
**Department:** Finance  
**Domain:** `triiumph.com`  
**Client:** Windows 11 Pro (`Desktop1`)  
**Status:** Resolved

## Request

A manager requested a new domain account for Andy Johnson, a new Finance employee.

The account needed to be created in Active Directory and provided with the appropriate Finance access.

## Investigation & Provisioning

### 1. Created the Domain Account

Created the `andy.johnson` user account in Active Directory under the appropriate user OU.

The account was configured with an initial password and confirmed as enabled.

### 2. Assigned Department Access

Added Andy Johnson to the Finance security group responsible for access to the Finance network resource.

This provided the user with the same department-level access required for the Finance role.

### 3. Verified Domain Authentication

Andy successfully signed in to the Windows 11 domain client.

The authenticated session was verified using:

```cmd
whoami
```
This confirmed that the Windows session was authenticated as the newly provisioned domain user.

### 4. Validated Resource Access

Andy successfully accessed:

```text
\\TRIIUMPH-DC-01\Finance
```
This confirmed that the new account, group membership, domain authentication, and resource permissions were functioning together as expected.

## Resolution
Resolved.

The new Finance employee account was successfully provisioned, assigned the required security group membership, authenticated against the domain, and verified against the Finance network resource.

## Help Desk Skills Demonstrated

* Active Directory user provisioning
* User account administration
* Security group assignment
* Department-based access control
* Windows domain authentication
* Network share access validation
* User onboarding
* Post-provisioning verification

