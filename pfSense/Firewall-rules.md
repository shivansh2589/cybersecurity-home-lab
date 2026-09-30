# pfSense Firewall Rules

## Objective

Document the firewall rules used in my cybersecurity home lab to control traffic between the isolated lab network, the upstream home network, and the internet.

## Purpose

pfSense is used as the main firewall for the lab.

The firewall rules are designed to:

- Protect the home network from lab traffic
- Allow lab devices to reach the internet
- Control which services are permitted
- Support troubleshooting and testing
- Provide a foundation for future segmentation and VLAN work

## Rule Processing

pfSense evaluates firewall rules from top to bottom.

The first matching rule is applied.

Because of this, rule order is important.

A more specific block or allow rule must be placed above broader rules when required.

## Main Lab Isolation Rule

One of the key rules in the lab blocks devices on the cybersecurity lab network from accessing the primary home network.

### Purpose

The goal is to keep lab activity isolated from normal household devices while still allowing the lab to access the internet.

### Rule Logic

- **Interface:** LAN
- **Source:** Lab network
- **Destination:** Home network
- **Action:** Block
- **Description:** `BLOCK LAB TO HOME NETWORK`

This rule is placed above the general LAN allow rule.

## Internet Access Rule

Lab devices are allowed to access the internet through pfSense.

This supports:

- Software updates
- Security-tool downloads
- Cloud services
- DNS resolution
- Web access
- Lab testing

The general LAN allow rule remains below the isolation rule so that the home-network block is evaluated first.

## Traffic Tested

After configuring the firewall rules, I tested expected behavior from multiple lab devices.

Testing included:

- Internet connectivity
- DNS resolution
- ICMP connectivity
- SSH access
- Access to lab systems
- Attempts to reach the upstream home network

## Expected Results

The intended behavior is:

- Lab devices can access the internet
- Lab devices can communicate with approved lab systems
- Lab devices cannot freely access the upstream home network
- Specific exceptions can be added when needed for testing

## Firewall Rule Testing

When creating or modifying rules, I verify both allowed and blocked traffic.

Testing methods include:

- Ping
- DNS queries
- Web browsing
- SSH
- Firewall logs
- pfSense diagnostics
- Packet capture when needed

## Troubleshooting Firewall Rules

If traffic does not behave as expected, I check:

1. Rule order
2. Interface selection
3. Source
4. Destination
5. Protocol
6. Port numbers
7. Gateway
8. NAT configuration
9. Firewall logs
10. Device-side firewall settings

## Security Concepts Practiced

This work reinforces:

- Network segmentation
- Least privilege
- Access control
- Traffic filtering
- Rule ordering
- Default-deny thinking
- Firewall logging
- Change verification

## Skills Practiced

- pfSense firewall administration
- Firewall rule creation
- Rule-order troubleshooting
- Network segmentation
- Access-control testing
- TCP/IP troubleshooting
- ICMP and SSH testing
- Security validation

## Evidence

A screenshot of the pfSense LAN rules is also included in my main cybersecurity portfolio.

It shows the lab-to-home block rule placed above the general LAN allow rule.

## Next Steps

Future firewall work may include:

- VLAN-specific rules
- Guest-network isolation
- Additional logging
- Restricting management access
- Inter-VLAN communication testing
- More granular service-based rules

## Key Takeaway

The most important part of this firewall configuration is that the cybersecurity lab is isolated from the primary home network while retaining the connectivity needed for hands-on security practice.
