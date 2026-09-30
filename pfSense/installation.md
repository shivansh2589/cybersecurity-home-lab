# pfSense Installation

## Objective

Document the installation of pfSense on a dedicated Dell laptop used as the firewall for my cybersecurity home lab.

## Hardware Used

- Dell laptop
- USB flash drive
- USB Ethernet adapter(s)
- Another computer used to create the pfSense installer
- Network switch
- Ethernet cables

## Installation Process

### 1. Downloaded pfSense

Downloaded the pfSense installer from the official pfSense website.

The installer image was prepared on another computer before being transferred to the Dell laptop.

### 2. Created a Bootable USB

Used Rufus to write the pfSense installer image to a USB flash drive.

The USB drive was then connected to the Dell laptop.

### 3. Booted the Dell Laptop from USB

Opened the Dell boot menu and selected the USB flash drive as the boot device.

The pfSense installer then started successfully.

### 4. Installed pfSense

Followed the pfSense installation prompts and installed pfSense onto the Dell laptop's internal storage.

After installation was complete, the USB installer was removed and the laptop was rebooted.

### 5. Assigned Network Interfaces

Configured separate network interfaces for:

- WAN
- LAN

The Ethernet adapters were identified and assigned to the appropriate interfaces.

### 6. Configured the LAN Interface

Configured the LAN interface so lab devices could communicate with the pfSense firewall.

This prepared the firewall for connection to the home-lab switch and other internal devices.

### 7. Rebooted and Verified pfSense

After configuration, the Dell laptop was rebooted.

I verified that:

- pfSense started successfully
- WAN and LAN interfaces were assigned
- Interface information appeared correctly on the console
- The firewall remained operational after reboot

## Current Status

pfSense is installed and operational on the dedicated Dell laptop.

The firewall is being used as the main security boundary for the cybersecurity home lab.

## Skills Practiced

- Firewall installation
- Bootable USB creation
- Network interface assignment
- WAN/LAN configuration
- Network troubleshooting
- pfSense administration
- Hardware and network verification

## Key Takeaway

Installing pfSense on dedicated hardware provided hands-on experience with firewall deployment, interface assignment, and the basic network configuration required to build a segmented cybersecurity lab.
