# Proxmox Cybersecurity Home Lab

## Overview

This project documents the build, troubleshooting, and security-monitoring validation of a Proxmox VE home lab integrated with pfSense and Wazuh.

The goal is to create a realistic multi-system environment for practicing virtualization, network defense, Windows and Linux administration, Active Directory monitoring, endpoint security, and SOC-style investigations.

## Architecture

```text
Internet / Home Network
        |
        v
   pfSense Firewall
 WAN: 192.168.0.15
 LAN: 192.168.10.1
        |
        v
Cybersecurity Lab - 192.168.10.0/24
        |
        +-- Proxmox VE .............. 192.168.10.80
             +-- Wazuh Server ....... 192.168.10.30
             +-- Kali Linux ......... 192.168.10.60
             +-- Ubuntu Server ...... 192.168.10.90
             +-- Windows Server 2022
             +-- Client VM
```

## VM Roles

| VM | Role |
|---|---|
| Ubuntu Server | Linux administration and monitored endpoint |
| Wazuh Server | Manager, Indexer, and Dashboard |
| Windows Server 2022 | AD DS, DNS, Windows security monitoring |
| Kali Linux | Attack simulation and testing |
| Client VM | Windows client / future endpoint testing |

## Proxmox Network Recovery

### Problem

The Proxmox host retained the correct static address and route but became unreachable from the rest of the lab. Pings returned `Destination Host Unreachable` and the web interface was unavailable.

### Validation

Routing was checked with:

```bash
ip route
```

The host still had:

```text
default via 192.168.10.1 dev vmbr0
192.168.10.0/24 dev vmbr0
```

The persistent network configuration also correctly referenced the physical adapter:

```text
iface enx8cae4cb99fee inet manual

auto vmbr0
iface vmbr0 inet static
    address 192.168.10.80/24
    gateway 192.168.10.1
    bridge-ports enx8cae4cb99fee
    bridge-stp off
    bridge-fd 0
```

However, `bridge link show` revealed that the physical interface was missing from the live `vmbr0` bridge.

### Recovery

The interface was brought online:

```bash
ip link set enx8cae4cb99fee up
```

It was then attached to `vmbr0`:

```bash
ip link set enx8cae4cb99fee master vmbr0
```

The bridge configuration was reloaded:

```bash
ifreload -a
```

The repair was validated with gateway pings and a controlled Proxmox reboot. The host returned online at `192.168.10.80` and the web interface remained accessible.

## VM Connectivity Validation

After host networking was restored, VMs were started individually and tested.

Ubuntu Server:

```text
192.168.10.90
```

Kali Linux:

```text
192.168.10.60
```

Wazuh Server:

```text
192.168.10.30
```

Validation included:

```bash
ping -c 4 192.168.10.1
ping -c 4 8.8.8.8
```

Ubuntu and Kali also successfully communicated with each other across `vmbr0`.

## Wazuh Platform Validation

The following services were verified as active:

```bash
sudo systemctl status wazuh-manager
sudo systemctl status wazuh-indexer
sudo systemctl status wazuh-dashboard
```

The Wazuh Dashboard was reachable at:

```text
https://192.168.10.30
```

Active monitored endpoints included Windows, Windows Server 2022, Ubuntu Server, and Kali Linux.

## Windows Server 2022 Wazuh Agent

Windows Server 2022 was enrolled with Wazuh Manager `192.168.10.30`.

The Wazuh service was verified with:

```powershell
Get-Service WazuhSvc
```

Agent logs confirmed successful communication:

```text
Connected to the server ([192.168.10.30]:1514/tcp)
Agent is now online
FIM sync module started
```

## File Integrity Monitoring

A dedicated test directory was added to the Windows Wazuh agent configuration:

```xml
<directories realtime="yes">C:\Wazuh-FIM-Test</directories>
```

A test file was created and modified:

```powershell
New-Item -Path "C:\Wazuh-FIM-Test\test1.txt" -ItemType File
Add-Content -Path "C:\Wazuh-FIM-Test\test1.txt" -Value "Wazuh FIM test"
```

Wazuh detected both actions:

- Rule ID `554` - file added
- Rule ID `550` - integrity checksum changed

The event included MD5, SHA1, and SHA256 integrity data.

## Active Directory Security Monitoring

Windows Server 2022 was validated as an Active Directory Domain Controller with AD DS and DNS running.

Wazuh was then used to validate key Windows Security events.

| Event ID | Activity |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4720 | User account created |
| 4722 | User account enabled |
| 4724 | Password reset attempt |
| 4725 | User account disabled |
| 4726 | User account deleted |
| 4728 | Member added to global security group |
| 4729 | Member removed from global security group |
| 4740 | User account locked out |
| 4767 | User account unlocked |

Events `4728` and `4729` have been validated for ordinary global security-group membership changes. The next controlled phase is to repeat this workflow with the privileged **Domain Admins** group and investigate the resulting events in Wazuh.

### Example: User Creation

A temporary test account was created in Active Directory. Windows generated Event ID `4720`, and Wazuh indexed it with the description:

```text
User account enabled or created
```

### Example: Account Lockout

A fine-grained lockout policy was temporarily applied to a test account so three failed logons would trigger a lockout.

Wazuh detected Event ID `4740`:

```text
User account locked out (multiple login errors)
```

The temporary policy and test objects were removed after validation.

## SOC-Style Investigation

A small incident investigation was performed by correlating the account-lockout event with the failed logons immediately before it.

```text
4625 Failed Logon
      |
4625 Failed Logon
      |
4625 Failed Logon
      |
4740 Account Lockout
```

Evidence reviewed included:

- target username
- source/caller computer
- event ID
- Wazuh rule ID and severity
- timestamp
- Windows Security event channel

### Assessment

The activity was confirmed as an authorized lab test. The account was unlocked after validation and temporary test objects were removed.

## Reusable AD Security View

A Wazuh Threat Hunting view was saved to group the main Windows Server / Active Directory event IDs into a reusable monitoring workflow.

This reduces repetitive searching and provides a faster SOC-style view of account-management and authentication activity.

## Resource Management

The Proxmox host has approximately 16 GB RAM.

Running Wazuh, Ubuntu, Kali, and Windows Server simultaneously pushed memory usage above 90%. Kali was stopped when not needed, bringing the host back to approximately 79-81% usage with no swap consumption.

This established a practical operating strategy for the current hardware.

## Skills Demonstrated

- Proxmox VE administration
- Linux bridge troubleshooting
- Virtual machine deployment
- VM resource management
- pfSense integration
- Windows Server administration
- Active Directory Domain Services
- DNS
- PowerShell
- Linux administration
- Wazuh SIEM/XDR administration
- Endpoint enrollment
- Windows Security Event analysis
- File Integrity Monitoring
- Active Directory account-management monitoring
- Authentication monitoring
- Event correlation
- SOC-style incident triage
- Structured troubleshooting
- Technical documentation

## Current Status

The Proxmox lab is operational and integrated with pfSense and Wazuh. Host networking is stable after recovery, Windows and Linux endpoints are monitored, Active Directory security events are being collected, FIM is validated, and a repeatable SOC-style investigation workflow has been practiced.

## Next Phase

- Controlled Domain Admins membership monitoring and investigation
- Additional SOC-style investigations
- Custom Wazuh rules
- Expanded Linux monitoring
- Additional attack/defense scenarios
