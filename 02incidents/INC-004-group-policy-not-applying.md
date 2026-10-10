## INC-004 — Group Policy Not Applying as Expected

Category: Windows Administration / Group Policy

Priority: Medium

Domain: triiumph.com

Domain Controller: TRIIUMPH-DC-01

Client: Windows 11 Pro (Desktop1)

Status: Resolved

Scenario Type: Controlled Lab Simulation


## Scenario

A Windows workstation was expected to receive a specific computer configuration through Group Policy, but the test policy was not initially applying to the workstation.

This controlled lab simulation was designed to investigate Group Policy Object (GPO) linking, Organizational Unit (OU) scope, and policy application.


## Investigation

### 1. Verified the Computer’s OU

Using Active Directory Users and Computers, I confirmed that the Desktop1 computer account was located in:

```text
Computers\workstations
```

This established the OU containing the target workstation.

### 2. Verified Existing Group Policy Processing

On the Windows 11 client, I ran:

```cmd
gpresult /r /scope computer
```

The initial results showed that the following policies were applied:

* GPO-Workstation-Security
* Default Domain Policy

The test policy, GPO-Test-Desktop-Policyy, was absent from the initial list of applied computer policies.

### 3. Reviewed the Test GPO Configuration

I created a separate GPO named GPO-Test-Desktop-Policyy and configured the following computer security options.

Interactive logon: Message title for users attempting to log on

```text
Company Workstation Policy
```

Interactive logon: Message text for users attempting to log on

```text
This workstation is managed by the IT Support team.
```

The test GPO was initially linked to the Finance OU, which did not contain the Desktop1 computer account.


## Root Cause

The test GPO was linked to an OU outside the target workstation’s location. As a result, Desktop1 was outside the scope of that GPO link.

The investigation distinguished an OU-linking issue from a general Group Policy processing failure, since the existing workstation security policies were already applying.


## Remediation

I corrected the GPO link by linking the existing GPO-Test-Desktop-Policy to the Computers\workstations OU, where the Desktop1 computer account resides.

I then refreshed Group Policy on the Windows 11 client using:

```cmd
gpupdate /force
```

## Validation

### 1. Confirmed Policy Application

I ran the following command again:

```cmd
gpresult /r /scope computer
```

The output showed GPO-Test-Desktop-Policy under Applied Group Policy Objects.

### 2. Verified the Actual Setting

After restarting the Windows 11 client, I checked the sign-in screen and confirmed that the configured logon notice appeared.

Logon notice title:

```text
Company Workstation Policy
```

Logon notice message:

```text
This workstation is managed by the IT Support team.
```

This verified that the policy was not only listed as applied but that its configured setting was taking effect.


## Resolution

Status: Resolved

The controlled policy application failure was caused by an incorrect GPO link. Linking the test policy to the OU containing the target workstation restored the intended policy application.


## Skills Demonstrated

* Group Policy troubleshooting — investigating GPO application and scope
* Active Directory administration — identifying computer OU placement
* GPO linking — correcting the link to the target OU
* Command-line diagnostics — using gpresult and gpupdate
* Windows security configuration — configuring an interactive logon notice
* Root-cause analysis — identifying the incorrect GPO link
* Post-remediation validation — confirming policy application and the resulting setting
