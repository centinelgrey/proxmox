# Installation Guide

Proxmox is an open source virtualization platform categorized as a type 1 hypervisor. Though there is a subscription model, the free version offers more than enough features for a homelab setup. I have previously used VMware Workstation for virtualization but decided to try out Proxmox as my laptop was showing stress signs from the number of VMs I needed to periodically run.

## Table of Contents
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Network Configuration](#network-configuration)
* [Post-Installation Steps](#post-installation-steps)
* [Troubleshooting](#troubleshooting)

## Prerequisites

### System Requirements
* CPU: 64-bit processor with virtualization support (Intel VT-x/AMD-V)
* RAM: Minimum 8GB, recommended 16GB or more
* Storage: At least 32GB for system installation
* Network: Gigabit Ethernet recommended
* USB drive (for installation media)

### Required Software
* [Proxmox VE ISO](https://www.proxmox.com/en/downloads) (current version 8.3)
* [Ventoy](https://www.ventoy.net/) for creating bootable USB

## Installation

The current version of Proxmox is 8.3

### Preparing Installation Media
1. Download the Proxmox VE ISO installer from the official website
![image](https://github.com/user-attachments/assets/460665a7-01b3-46ca-861e-c5f0093a99da)
   
2. Install Ventoy on your USB drive
   * Ventoy allows booting multiple images from the same flash drive, allowing you the ease of not having to maintain multiple drives for installing different systems
   * Simply copy the Proxmox ISO to the Ventoy drive after installation

### BIOS Configuration
1. Insert the USB drive into the target system
2. Configure the BIOS of the system being used to setup Proxmox to boot from the USB Drive
3. Upon booting, select the first option *Install Proxmox VE (Graphical)* and proceed
![image](https://github.com/user-attachments/assets/8f5b6fb9-a009-41fd-a9b3-70b0d7904de1)


4. Accept the End User License Agreement by clicking on "I agree"
![image](https://github.com/user-attachments/assets/9fc61830-7dc4-4567-9edb-f1e0df62252c)

### Storage Configuration
1. Select the hard drive on which Proxmox is to be installed and click on options
2. Choose storage configuration options:
   * For this installation we will be using zfs(RAID0)
![image](https://github.com/user-attachments/assets/6a6d8045-2ab0-4b7f-956b-0ef4162948ef)

   * This is because the system on which Proxmox is being installed is equipped with only a single drive
   * Later another drive will be added and a mirrored pool configured using zfs(RAID1) to provide redundancy should any of the drives fail
   * This only provides tolerance for a single disk failure, for my purposes this is adequate, however if you need additional fault tolerance, then you may need to consider the other RAID options

#### ZFS Configuration Options
```
RAID0 (Single disk) - No redundancy, maximum storage
RAID1 (Mirror) - 50% storage, single disk failure tolerance
RAID10 (Stripe of mirrors) - 50% storage, better performance
RAIDZ-1 (Similar to RAID5) - Single disk failure tolerance
RAIDZ-2 (Similar to RAID6) - Two disk failure tolerance
```

### System Configuration
1. Provide your time zone information and click Next
![image](https://github.com/user-attachments/assets/f485a7bc-e336-4047-b249-6d47d73a9218)

2. Provide a password and email address
   * Follow password hygiene rules
   * Use a unique password, not easy to guess
   * At least 12 characters
   * Store securely in a password manager
![image](https://github.com/user-attachments/assets/8317e20e-966b-470a-9965-1ece0d619132)

    
3. Network Configuration:
   * If you are plugged into a network which is using DHCP, an address will be assigned automatically
   * Since this is going to be a server hosting other workloads, you should change this to use a static address
![image](https://github.com/user-attachments/assets/bb5af8a2-430f-4fdd-b000-00e349c73936)

4. Review the summary options and click on Install, check the option at the bottom to ensure server is rebooted when installation is completed.
![image](https://github.com/user-attachments/assets/0ce436e4-4eb1-4fb9-8cfa-f5c2c20a3a04)



## Post-Installation Steps

1. Access Web Interface
   * Navigate to https://your-ip:8006
   * Login with root and your configured password
![image](https://github.com/user-attachments/assets/f9212836-6bdb-4ebb-8427-f35801cf1da4)

2. Subscription Notice
   * You will see a notice after logging in about not having a valid subscription. You can ignore this for now. Click on Ok to close the notice.
![image](https://github.com/user-attachments/assets/730d8436-ce85-452e-bd05-da705e0aad02)


