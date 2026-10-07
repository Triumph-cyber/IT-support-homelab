# IT Support & Active Directory Home Lab

A hands-on IT Support home lab built to demonstrate practical Windows administration, Active Directory, Group Policy, authentication, access control, DNS troubleshooting, and user support skills.

The lab simulates a small business Windows domain environment and focuses on investigating and resolving realistic Help Desk and IT Support incidents.

---

## Environment

| Component | Configuration |
|---|---|
| Domain Controller | Windows Server 2022 |
| Client | Windows 11 Pro |
| Active Directory Domain | `triiumph.com` |
| Domain Controller | `TRIIUMPH-DC-01` |
| Domain Controller IP | `10.1.10.2` |
| Client Hostname | `Desktop1` |
| Client IP | `10.1.10.6` |

---

## Technical Skills Demonstrated

- Active Directory user and group administration
- Windows domain authentication
- Group Policy configuration and troubleshooting
- Account lockout troubleshooting
- Domain controller discovery
- Kerberos authentication troubleshooting
- NTFS and network share permissions
- Security group-based access control
- DNS troubleshooting
- IPv4/IPv6 network troubleshooting
- Windows Event Viewer investigation
- Help Desk incident investigation
- Root-cause analysis
- Post-remediation validation

---

## Incidents & Troubleshooting

### INC-001 — Domain Account Lockout Not Triggering

Investigated a domain account that was not being locked out after repeated incorrect interactive logon attempts.

**Investigation included:**

- Reviewing the Active Directory account lockout policy
- Verifying Group Policy application
- Verifying domain controller discovery
- Checking the logon server
- Verifying Kerberos authentication
- Investigating Windows interactive logon behavior
- Reviewing Security Event ID 4740

**Resolution:**

Enabled:

`Computer Configuration → Administrative Templates → System → Logon → Always wait for the network at computer startup and logon`

After remediation, repeated invalid logons successfully triggered the Active Directory account lockout mechanism.

[View Incident Report](02incidents/INC-001-account-lockout.md)

---

### INC-002 — Finance Share Access Denied

Investigated a user who could not access the Finance network share.

**Investigation included:**

- Confirming the affected user's identity
- Inspecting NTFS permissions on the Finance folder
- Identifying the security group controlling access
- Checking the user's group membership
- Restoring the required security group membership
- Re-authenticating the user
- Validating access to the network share

**Resolution:**

The user was added back to the Finance security group that controlled access to the resource.

[View Incident Report](02incidents/INC-002-finance-share-access.md)

---

### DNS Troubleshooting — Internal Domain Resolution

Investigated a Windows 11 client that could not reliably resolve the internal Active Directory domain.

**Investigation included:**

- Reviewing client DNS configuration
- Testing DNS resolution with `nslookup`
- Querying the domain controller DNS server directly
- Isolating the issue to the client DNS path
- Investigating IPv6 DNS behavior

**Resolution:**

Disabled IPv6 on the lab Ethernet adapter so the client would use the working IPv4 DNS configuration provided by the domain controller.

Internal DNS resolution was then successfully restored.

[View DNS Troubleshooting Documentation](03documentation/dns-troubleshooting.md)

---

## Repository Structure

```text
IT-support-homelab/
│
├── README.md
│
├── 01environment/
│   ├── README.md
│   └── Active Directory environment documentation
│
├── 02incidents/
│   ├── README.md
│   ├── INC-001-account-lockout.md
│   └── INC-002-finance-share-access.md
│
├── 03documentation/
│   ├── README.md
│   └── dns-troubleshooting.md
│
└── 04evidence/
    └── Troubleshooting screenshots and supporting evidence

```


## Troubleshooting Methodology

Each incident follows a structured IT Support troubleshooting process:

1. Identify the reported issue.
2. Confirm the affected user, device, and resource.
3. Gather technical evidence.
4. Isolate the source of the problem.
5. Identify the root cause.
6. Apply the appropriate remediation.
7. Validate that the issue is resolved.
8. Document the resolution.

The goal is to demonstrate not only the final fix, but the investigation and reasoning used to reach it.


## Lab Objective

This project was built to develop and demonstrate practical IT Support experience in a Windows enterprise-style environment.

The lab focuses on the types of issues commonly encountered by Help Desk and IT Support teams, including authentication failures, account problems, permissions issues, Group Policy behavior, network connectivity, and DNS resolution.
