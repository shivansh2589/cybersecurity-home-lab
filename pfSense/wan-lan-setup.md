# pfSense WAN and LAN Setup

## Objective

Configure the WAN and LAN interfaces on pfSense so the firewall can connect the home internet connection to the cybersecurity lab network.

## Network Design

Internet / Home Router  
↓  
pfSense WAN  
↓  
pfSense LAN  
↓  
Network Switch  
↓  
Lab Computers / Servers / Wireless Access Point

## WAN Interface

The WAN interface connects pfSense to the existing home network / internet router.

### WAN Tasks Completed

- Connected the WAN USB Ethernet adapter
- Identified the WAN interface in pfSense
- Assigned the WAN interface
- Verified that the WAN interface received network connectivity
- Confirmed pfSense could communicate with the upstream router

## LAN Interface

The LAN interface connects pfSense to the cybersecurity home lab network.

### LAN Tasks Completed

- Connected the LAN USB Ethernet adapter
- Assigned the LAN interface in pfSense
- Connected the LAN interface to the network switch
- Verified that the LAN interface was active
- Prepared the LAN side for lab devices

## Interface Assignment

The two Ethernet adapters were used for separate purposes:

- WAN adapter → Internet / upstream router
- LAN adapter → Home lab switch

Keeping WAN and LAN on separate interfaces allows pfSense to control and inspect traffic passing between the internet and the lab network.

## Verification

After configuring the interfaces, I checked the pfSense console to confirm that:

- WAN was assigned correctly
- LAN was assigned correctly
- Both interfaces were active
- The firewall remained operational after reboot

## Troubleshooting

During setup, I verified the physical connections and interface assignments when the LAN side did not initially show connectivity.

This included:

- Checking USB Ethernet adapters
- Confirming cables were connected
- Reviewing pfSense interface assignments
- Rebooting pfSense
- Verifying interface status from the console

## Skills Practiced

- WAN and LAN configuration
- Interface assignment
- Network troubleshooting
- Firewall deployment
- TCP/IP networking
- Physical network connectivity
- pfSense administration

## Next Steps

Future work will include:

- DHCP configuration
- Firewall rules
- VLAN configuration
- Network segmentation
- Wireless access point integration
- Traffic monitoring
