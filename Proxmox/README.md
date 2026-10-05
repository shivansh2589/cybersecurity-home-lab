# Proxmox Cybersecurity Home Lab

## Overview

This project documents the build, troubleshooting, and security-monitoring validation of a Proxmox VE home lab integrated with pfSense and Wazuh.

The goal is to create a realistic multi-system environment for practicing virtualization, network defense, Windows and Linux administration, Active Directory monitoring, endpoint security, and SOC-style investigations.

## Architecture

The lab includes Proxmox VE, pfSense, Wazuh, Windows Server 2022, Ubuntu Server, Kali Linux, and Windows endpoints on the segmented cybersecurity network.

## Active Directory Security Monitoring

Windows Server 2022 was validated as an Active Directory Domain Controller with AD DS and DNS running. Wazuh was used to validate key Windows Security events, including successful and failed logons, account creation and deletion, password and lockout activity, and group-membership changes.

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

### Privileged Domain Admins Monitoring

A controlled test account named `PrivTest` was temporarily added to the **Domain Admins** group and then removed.

Wazuh captured the corresponding privileged-group membership events:

- Event ID `4728` for the addition to Domain Admins
- Event ID `4729` for the removal from Domain Admins

The event details confirmed the changed member, the `Domain Admins` target group, the administrative actor, and the domain controller that recorded the change.

This validated centralized monitoring of high-impact Active Directory privilege changes through Wazuh Threat Hunting.

## File Integrity Monitoring

Real-time Windows File Integrity Monitoring was configured and validated. Wazuh detected file creation and file modification events and captured integrity hash data.

## SOC-Style Investigation

A controlled account-lockout scenario was used to practice event correlation. Repeated failed authentication events were correlated with the resulting account-lockout event in Wazuh.

## Reusable AD Security View

A Wazuh Threat Hunting view was saved to group important Windows Server and Active Directory event IDs into a reusable monitoring workflow.

## Skills Demonstrated

- Proxmox VE administration
- pfSense integration
- Windows Server administration
- Active Directory Domain Services
- Privileged Active Directory group monitoring
- Wazuh SIEM/XDR administration
- Windows Security Event analysis
- File Integrity Monitoring
- Authentication and account-management monitoring
- Event correlation
- SOC-style incident triage
- Structured troubleshooting
- Technical documentation

## Current Status

The lab is operational with centralized Wazuh monitoring, Active Directory event collection, real-time FIM, privileged Domain Admins membership monitoring, and a reusable AD security-event workflow.

## Next Phase

- Additional SOC-style investigations
- Custom Wazuh detection rules
- Expanded Linux monitoring
- Additional attack/defense scenarios
