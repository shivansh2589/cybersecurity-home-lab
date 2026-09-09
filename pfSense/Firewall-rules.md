# pfSense Firewall Rules

## Objective

Configure and document firewall rules in pfSense to control traffic between the home lab network and other networks.

## Purpose

Firewall rules determine what traffic is allowed or blocked.

In my home lab, pfSense is used to control traffic between:

- WAN
- LAN
- Lab devices
- Future VLANs
- Wireless devices

## Default Behavior

pfSense uses firewall rules to decide whether network traffic is permitted.

Rules are evaluated from top to bottom.

The first matching rule is applied.

## LAN Rules

The LAN interface is used by devices inside the cybersecurity home lab.

Typical LAN rules may include:

- Allow LAN devices to access the internet
- Allow DNS traffic
- Allow DHCP traffic
- Allow management access to pfSense
- Block unnecessary traffic

## WAN Rules

The WAN interface faces the upstream network.

For security, unsolicited inbound traffic should normally remain blocked unless there is a specific reason to allow it.

## Example Rule Structure

A firewall rule can include:

- Action: Pass or Block
- Interface
- Protocol
- Source
- Destination
- Source Port
- Destination Port
- Description

## Example Lab Rule

Example:

Action: Pass

Interface: LAN

Protocol: TCP/UDP

Source: LAN net

Destination: Any

Description: Allow LAN devices outbound access

## Security Principles Practiced

- Least privilege
- Default deny
- Network segmentation
- Access control
- Traffic filtering
- Rule documentation

## Testing Firewall Rules

After creating or modifying rules, I verify that the expected traffic is allowed or blocked.

Testing can include:

- Ping
- Web browsing
- DNS queries
- Port testing
- pfSense firewall logs
- Packet capture

## Troubleshooting

If traffic does not work as expected, I check:

- Rule order
- Interface selection
- Source and destination
- Protocol
- Port numbers
- Firewall logs
- NAT configuration

## Skills Practiced

- Firewall rule creation
- Access control
- Network security
- Rule troubleshooting
- Traffic analysis
- pfSense administration

## Next Steps

Future firewall work may include:

- VLAN-specific rules
- Guest network isolation
- Blocking unnecessary ports
- Logging suspicious traffic
- Restricting management access
- Testing inter-VLAN communication
