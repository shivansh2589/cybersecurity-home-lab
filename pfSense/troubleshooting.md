# pfSense Troubleshooting Notes

## Objective

Document common issues encountered while installing and configuring pfSense in my cybersecurity home lab.

## Issue 1: WAN or LAN Interface Not Detected

### Symptoms

- WAN or LAN interface does not appear in pfSense
- Ethernet adapter is connected but no interface is shown
- No IP address is assigned

### Troubleshooting Steps

- Checked that the USB Ethernet adapter was securely connected
- Verified the Ethernet cable connection
- Confirmed that the correct adapter was assigned to WAN or LAN
- Reviewed interface assignments from the pfSense console
- Rebooted pfSense after changing interface settings

### Lesson Learned

Correct interface identification is important when multiple USB Ethernet adapters are connected.

---

## Issue 2: LAN Connection Not Working

### Symptoms

- LAN interface appears configured
- Connected device does not receive network access
- Switch does not appear to have connectivity

### Troubleshooting Steps

- Confirmed the LAN adapter was connected to the network switch
- Checked the Ethernet cable
- Verified the LAN interface was enabled
- Confirmed the correct LAN IP configuration
- Rechecked WAN and LAN assignments
- Restarted the interface when necessary

### Lesson Learned

Physical connectivity and interface assignment should be checked before changing more advanced firewall settings.

---

## Issue 3: No Internet Access Through pfSense

### Symptoms

- pfSense is running
- LAN devices are connected
- Internet access is unavailable

### Troubleshooting Steps

- Verified the WAN interface received an IP address
- Confirmed the upstream home router was working
- Checked WAN gateway status
- Reviewed firewall rules
- Checked NAT configuration
- Tested connectivity using ping

### Useful Tests

**Issue 4: USB Ethernet Adapter Problems**

**Symptoms**

Adapter disappears after reboot
Interface names change
WAN and LAN become reversed

**Troubleshooting Steps**

Identified each USB Ethernet adapter before assignment
Kept WAN and LAN adapters in consistent USB ports
Verified interface names after reboot
Reassigned interfaces when necessary

**Lesson Learned**

Keeping the same physical USB ports for WAN and LAN helps avoid confusion.

**Issue 5: pfSense Appears Frozen or Unresponsive**
Symptoms
Console does not appear to respond
Display remains on the same screen
Network connectivity stops responding
Troubleshooting Steps
Waited to confirm whether pfSense was still processing
Checked network activity
Used the pfSense console menu when available
Restarted the system when necessary
Verified interfaces again after reboot
Troubleshooting Approach

When troubleshooting pfSense, I follow this order:

Check power and physical connections
Check Ethernet cables
Check WAN and LAN adapter assignments
Check IP addresses
Check gateway status
Check DHCP
Check firewall rules
Check NAT
Review logs
Test connectivity
**Useful pfSense Tools**
Interface Status
Gateway Status
System Logs
Firewall Logs
Diagnostics > Ping
Diagnostics > Traceroute
Diagnostics > Packet Capture
**Skills Practiced**
Network troubleshooting
Interface troubleshooting
TCP/IP diagnostics
Firewall troubleshooting
Gateway troubleshooting
DNS troubleshooting
Physical network diagnostics
pfSense administration
Key Takeaway

Troubleshooting should begin with basic physical connectivity and interface configuration before moving to firewall rules, NAT, DNS, and other advanced settings.
