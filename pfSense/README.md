# pfSense Firewall Project

This folder documents the pfSense firewall deployment used in my cybersecurity home lab.

The project focuses on hands-on experience with:

- Firewall installation
- WAN and LAN configuration
- Network interface assignment
- Firewall rules
- Network segmentation
- Access control
- Troubleshooting
- TCP/IP networking
- Secure lab design

## Project Objective

Use pfSense as the main firewall and security boundary between the upstream home network and the isolated cybersecurity lab environment.

## Current Architecture

```text
Internet / Home Router
        |
        v
    pfSense WAN
        |
        v
    pfSense LAN
        |
        v
    Network Switch
        |
        +-- Lab Computers
        +-- Servers
        +-- Wireless Access Point
```

## Documentation

### Installation

[`installation.md`](installation.md)

Documents the pfSense installation on dedicated hardware, including bootable USB creation, interface assignment, and verification after reboot.

### WAN and LAN Setup

[`wan-lan-setup.md`](wan-lan-setup.md)

Documents the configuration of separate WAN and LAN interfaces and the basic network design used for the lab.

### Firewall Rules

[`Firewall-rules.md`](Firewall-rules.md)

Documents the firewall rule design used to isolate the cybersecurity lab from the upstream home network while preserving internet access.

### Troubleshooting

[`troubleshooting.md`](troubleshooting.md)

Documents common connectivity and interface problems encountered during the build, along with the troubleshooting process and lessons learned.

## Key Firewall Goal

One of the main security objectives is to keep cybersecurity lab traffic isolated from the primary home network.

The lab is designed so that:

- Lab devices can access the internet
- Approved lab systems can communicate with each other
- Lab devices are prevented from freely accessing the upstream home network
- Specific exceptions can be added when required for testing

## Hardware

The pfSense project uses:

- Dedicated Dell laptop
- USB Ethernet adapter(s)
- Network switch
- Ethernet cables
- Wireless access point
- Windows and Linux lab systems

## Skills Practiced

- pfSense administration
- Firewall deployment
- WAN and LAN configuration
- TCP/IP networking
- Network segmentation
- Firewall rule creation
- Rule-order troubleshooting
- Access-control testing
- NAT and routing concepts
- Network diagnostics
- Physical connectivity troubleshooting

## Security Concepts Practiced

- Least privilege
- Network segmentation
- Access control
- Traffic filtering
- Rule ordering
- Default-deny thinking
- Firewall logging
- Change verification

## Current Status

pfSense is installed and operational as the firewall for the cybersecurity home lab.

The project currently includes documented installation, WAN/LAN configuration, firewall rules, and troubleshooting.

## Next Steps

Future pfSense work may include:

- VLAN-specific rules
- Guest-network isolation
- Additional traffic logging
- More granular service-based rules
- Inter-VLAN communication testing
- Restricting management access
- Expanded monitoring and evidence

## Key Takeaway

This pfSense project provides hands-on experience with building and protecting a segmented lab network, testing firewall behavior, and troubleshooting real connectivity issues.
