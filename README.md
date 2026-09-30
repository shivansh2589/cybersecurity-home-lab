# Cybersecurity Home Lab

This repository documents my hands-on cybersecurity home lab, including firewall deployment, network segmentation, troubleshooting, and practical security configuration.

The lab is designed to strengthen my skills in:

- Network security
- Firewall administration
- TCP/IP networking
- Network segmentation
- Access control
- Troubleshooting
- Security monitoring
- Windows and Linux administration
- Virtualization
- Cloud-security integration

## Current Focus

The repository currently documents my **pfSense firewall project**, including:

- pfSense installation
- WAN and LAN configuration
- Interface assignment
- Firewall rules
- NAT and routing concepts
- Network troubleshooting
- Access-control concepts
- Physical and logical network verification

Additional sections will be added as the home lab continues to develop.

## Current Lab Architecture

The lab uses pfSense as the main firewall between the upstream home network and the cybersecurity lab environment.

```text
Internet / Home Router
        |
        v
   pfSense Firewall
        |
        v
    Lab Network
        |
        +-- Network Switch
        +-- Wireless Access Point
        +-- Windows Systems
        +-- Linux Systems
        +-- Virtualization Hosts
```

## Technologies

Current and planned technologies used in the home lab include:

- pfSense
- TCP/IP
- DHCP
- NAT
- Firewall Rules
- Network Segmentation
- Windows Server
- Windows 11
- Ubuntu Server
- Kali Linux
- PowerShell
- SSH
- UFW
- Splunk
- Wazuh
- Microsoft Sentinel
- Azure Arc
- VMware Fusion
- Proxmox VE

## pfSense Project

The pfSense section documents the firewall deployment and configuration process.

### Documentation

- [`pfSense/README.md`](pfSense/README.md) — pfSense project overview
- [`pfSense/installation.md`](pfSense/installation.md) — installation process
- [`pfSense/wan-lan-setup.md`](pfSense/wan-lan-setup.md) — WAN and LAN configuration
- [`pfSense/Firewall-rules.md`](pfSense/Firewall-rules.md) — firewall rule concepts and testing
- [`pfSense/troubleshooting.md`](pfSense/troubleshooting.md) — troubleshooting issues and lessons learned

## Skills Practiced

Through this lab, I have practiced:

- Installing and configuring pfSense
- Assigning WAN and LAN interfaces
- Working with USB Ethernet adapters
- Configuring firewall rules
- Understanding rule order
- Testing allowed and blocked traffic
- Verifying gateway connectivity
- Working with NAT and routing concepts
- Troubleshooting LAN connectivity
- Troubleshooting interface-assignment issues
- Testing connectivity with ping
- Reviewing firewall behavior
- Using a structured troubleshooting process

## Troubleshooting Approach

A major goal of this lab is learning how to troubleshoot methodically.

My general troubleshooting process is:

1. Check power and physical connections
2. Verify Ethernet cables and adapters
3. Confirm WAN and LAN interface assignments
4. Check IP addressing
5. Verify gateway connectivity
6. Check DHCP
7. Review firewall rules
8. Review NAT configuration
9. Check logs
10. Test connectivity

This approach helps isolate basic connectivity problems before moving into more advanced firewall or network configuration.

## Security Concepts Practiced

The lab is also used to reinforce security principles such as:

- Least privilege
- Default deny
- Network segmentation
- Access control
- Traffic filtering
- Rule documentation
- Secure administration
- Layered troubleshooting

## Current Status

The pfSense firewall is installed and operational, and the repository currently contains documentation for the initial firewall build, WAN/LAN setup, firewall rules, and troubleshooting.

The home lab is continuing to expand.

## Planned Repository Sections

Future documentation may include:

```text
cybersecurity-home-lab/
├── README.md
├── pfSense/
│   ├── README.md
│   ├── installation.md
│   ├── wan-lan-setup.md
│   ├── Firewall-rules.md
│   └── troubleshooting.md
├── Proxmox/
├── Wazuh/
├── Windows-Server/
├── Linux/
└── Cloud-Security/
```

## Upcoming Work

Planned improvements include:

- Expanding the Proxmox virtualization environment
- Deploying additional Windows and Linux virtual machines
- Rebuilding Wazuh in the virtualized lab
- Adding more firewall testing
- Adding project screenshots and evidence
- Expanding network segmentation
- Adding security-monitoring documentation
- Adding Windows Server documentation
- Adding Linux administration documentation
- Adding Azure Arc and Microsoft Sentinel notes

## Purpose

This repository is a technical record of my ongoing cybersecurity learning and home-lab development.

It is intended to demonstrate practical experience with:

- Network defense
- Firewall administration
- Troubleshooting
- Windows and Linux systems
- Security monitoring
- Virtualization
- Cloud-security technologies

## Related Portfolio

My main Cyber & Cloud Security portfolio is available here:

**GitHub:**  
https://github.com/shivansh2589

The portfolio repository contains a broader overview of my cybersecurity projects, certifications, skills, resume, and project evidence.
