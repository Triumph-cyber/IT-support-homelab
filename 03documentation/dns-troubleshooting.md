# DNS Troubleshooting — Internal Domain Resolution

## Issue

The Windows 11 domain client initially could not reliably resolve the internal Active Directory domain:

```text
triiumph.com
```

## Investigation

### Checked the Client's Network Configuration

I checked the Windows 11 client's network configuration using:

```cmd
ipconfig /all
```

The client's IPV4 DNS server was configured as:

```text
10.1.10.2
```

This was the IP address of the domain controller and internal DNS server. 
However, an IPv6 DNS path was also being presented on the client.

### Tested Normal DNS Resolution

I tested the client's ability to resolve the internal domain using:

```cmd
nslookup triiumph.com
```

The query timed out instead of returning the expected domain information.
This indicated that the client was having a problem reaching or using the configured DNS path.

### Tested the Domain Controller DNS server directly

To determine whether the DNS service itself was working, I queried the domain controller directly:

```cmd
nslookup triiumph.com 10.1.10.2
```

The query successfully returned:

```text
triiumph.com -> 10.1.10.2
```

This confirmed that the DNS service on the domain controller was functioning correctly.

The problem was therefore isolated to the client's DNS path rather than the DNS service on the domain controller.


## Root Cause

The Windows 11 client was attempting to use an IPv6 DNS path that was not responding correctly in the lab environment.

The IPv4 DNS configuration pointing to the domain controller was correct, but the additional IPv6 DNS path was interfering with normal DNS resolution.

## Remediation

I disabled IPv6 on the Windows 11 lab Ethernet adapter so that the client would use the working IPv4 DNS configuration provided by the domain controller.

## Validation

After making the change, I ran:

```cmd
nslookup triiumph.com
```

The query successfully resolved the internal domain. This confirmed that internal DNS name resolution was working correctly.

## Troubleshooting Approach

The investigation followed a simple troubleshooting process:

1. Check the client’s network and DNS configuration.
2. Test normal DNS resolution.
3. Query the intended DNS server directly.
4. Determine whether the problem is on the client or DNS server.
5. Apply the smallest targeted change.
6. Retest DNS resolution.

## Help Desk Skills Demonstrated

* Windows network troubleshooting
* DNS troubleshooting
* ipconfig analysis
* nslookup testing
* Client/server problem isolation
* IPv4/IPv6 troubleshooting
* Domain infrastructure troubleshooting
* Validation after remediation
