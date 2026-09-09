# pfSense Installation

## Objective

Install pfSense on a dedicated Dell laptop to use as the firewall for my cybersecurity home lab.

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

The pfSense installer then started.

### 4. Installed pfSense

Followed the pfSense installation prompts and installed pfSense onto the Dell laptop's internal storage.

After installation was complete, the USB installer was removed and the laptop was rebooted.

### 5. Assigned Network Interfaces

Configured the network interfaces for:

- WAN
- LAN

The Ethernet adapters were identified and assigned to the appropriate interfaces.

### 6. Configured the LAN

Configured the LAN interface so that devices on the home lab network could communicate with the pfSense firewall.

### 7. Rebooted and Verified pfSense

After configuration, the Dell laptop was rebooted.

pfSense started successfully and displayed the WAN and LAN interface information on the console.

## Current Status

pfSense is installed and running successfully on the Dell laptop.

The firewall is ready to be connected to the home lab switch and other network devices.

## Skills Practiced

- Firewall installation
- Bootable USB creation
- Network interface assignment
- WAN/LAN configuration
- Network troubleshooting
- pfSense administration
