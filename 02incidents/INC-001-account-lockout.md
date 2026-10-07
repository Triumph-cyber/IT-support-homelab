# INC-001 — Domain Account Lockout Not Triggering

**Category:** Account / Authentication  
**Priority:** Medium  
**User:** David.Smith  
**Domain:** `triiumph.com`  
**Domain Controller:** `TRIIUMPH-DC-01`  
**Client:** Windows 11 Pro (`Desktop1`)  
**Status:** Resolved

## Issue

Repeated incorrect passwords during interactive domain logon were not initially triggering the expected Active Directory account lockout.

The account remained:

```text
badPwdCount = 0
LockedOut = False
```

## Investigation

### 1. Reviewed the Account Lockout Policy

On the Domain Controller, I reviewed the domain's account lockout settings through **Group Policy Management**:

**Default Domain Policy → Computer Configuration → Windows Settings → Security Settings → Account Policies → Account Lockout Policy**

The configured settings were:

- **Account lockout threshold:** 5 invalid attempts

- **Account lockout duration:** 30 minutes

- **Reset account lockout counter after:** 30 minutes

The policy itself was therefore configured correctly.

### 2. Verified Group Policy application

On the Windows 11 client, I used,

```cmd
gpresult /r
```

This confirmed that the domain's Group Policy was being applied to the client. 

### 3. Verified Domain-Controller Discovery

I used:

```cmd
nltest /dsgetdc:triiumph.com
```

The client successfully located:

```text
TRIIUMPH-DC-01
10.1.10.2
```

### 4. Verify domain logon infrastructure

The client reported:

```text
%LOGONSERVER% = \TRIIUMPH-DC-01
```

Kerberos tickets were also visible with:

```cmd
klist
```


## Root Cause

Although the domain lockout policy was configured correctly and the client was joined to the domain, repeated incorrect passwords during interactive logon were not causing the expected account lockout.

The failed authentication attempts were not consistently reaching the domain controller at the point where Windows processed the interactive logon.

## Remediation

On the Windows client, I enabled:

**Computer Configuration → Administrative Templates → System → Logon → Always wait for the network at computer startup and logon**

I then restarted the client.

The setting was used to ensure that Windows waits for network and domain infrastructure to become available before processing interactive domain logons.

## Validation

Repeated invalid domain-password attempts then produced:

> The referenced account is currently locked out and may not be logged on to.

The domain controller Security log recorded **Event ID 4740**, confirming the account-lockout event.

The account was subsequently unlocked and successful domain authentication was verified.

## Key Help Desk Lesson

When an AD account-lockout policy appears correct but failed interactive logons are not incrementing `badPwdCount`, verify that the client is actually communicating with the domain controller during interactive logon. Do not assume the policy itself is broken.

## Help Desk Skills Demonstrated

* Active Directory account troubleshooting
* Group Policy troubleshooting
* Account lockout policy verification
* Domain controller discovery
* Kerberos authentication verification
* Windows interactive logon troubleshooting
* Event Viewer investigation
* Root-cause analysis
* Validation after remediation
