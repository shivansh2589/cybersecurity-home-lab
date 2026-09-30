# pfSense WAN and LAN Setup

## Objective

Document the WAN and LAN configuration used to connect the upstream home network to the isolated cybersecurity lab through pfSense.

## Network Design

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

## WAN Interface

The WAN interface connects pfSense to the existing home network and upstream internet connection.

### WAN Tasks Completed

- Connected the WAN USB Ethernet adapter
- Identified the WAN interface in pfSense
- Assigned the WAN interface
- Verified that the WAN interface received connectivity
- Confirmed pfSense could communicate with the upstream router

## LAN Interface

The LAN interface connects pfSense to the cybersecurity home-lab network.

### LAN Tasks Completed

- Connected the LAN USB Ethernet adapter
- Assigned the LAN interface in pfSense
- Connected the LAN interface to the network switch
- Verified that the LAN interface was active
- Prepared the LAN side for lab devices

## Interface Assignment

The two Ethernet adapters were used for separate purposes:

- **WAN adapter** → upstream home network / internet router
- **LAN adapter** → cybersecurity lab switch

Keeping WAN and LAN on separate interfaces allows pfSense to control traffic between the upstream network and the lab environment.

## Verification

After configuring the interfaces, I verified that:

- WAN was assigned correctly
- LAN was assigned correctly
- Both interfaces were active
- pfSense remained operational after reboot
- The lab side was ready for connected systems and services

## Troubleshooting Performed

During setup, I verified physical connections and interface assignments when the LAN side did not initially show connectivity.

Troubleshooting included:

- Checking USB Ethernet adapters
- Confirming Ethernet cables were connected
- Reviewing pfSense interface assignments
- Rebooting pfSense
- Verifying interface status from the console

## Skills Practiced

- WAN and LAN configuration
- Network interface assignment
- TCP/IP networking
- Network troubleshooting
- Firewall deployment
- Physical network verification
- pfSense administration

## Key Takeaway

Separating WAN and LAN onto dedicated interfaces created the foundation for a segmented lab network where pfSense can control and inspect traffic between the upstream home network and cybersecurity lab devices.

## Next Steps

Future work includes:

- Expanding firewall rules
- Adding additional network segmentation
- Integrating virtualized lab systems
- Adding monitoring and logging
- Documenting additional pfSense testing
