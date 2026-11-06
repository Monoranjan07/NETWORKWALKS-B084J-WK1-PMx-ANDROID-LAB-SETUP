<div align="center">

# NETWORKWALKS-B084J-WK1-PMx-ANDROID-LAB-SETUP

### Installing Android-x86 9.0-r2 in VMware Workstation and Configuring a Static IPv4 Address on a Custom Lab Network

NetworkWalks Cybersecurity Internship · Batch B084J · Week 01 · PMx Extra Project

![VMware](https://img.shields.io/badge/Hypervisor-VMware%20Workstation%2017%20Player-607078?style=for-the-badge&logo=vmware&logoColor=white)
![Android-x86](https://img.shields.io/badge/Guest%20OS-Android--x86%209.0--r2-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Lab%20Peer-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-blue?style=for-the-badge)
![Android IP](https://img.shields.io/badge/Android%20IP-10.0.0.9%2F24-informational?style=for-the-badge)

[![GitHub](https://img.shields.io/badge/GitHub-RaxingR-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RaxingR)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rajatava%20Ghosh-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rajatava-ghosh-265aa6316/)

</div>

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Objectives](#objectives)
3. [Virtual Machine Configuration](#virtual-machine-configuration)
4. [Installation Procedure](#installation-procedure)
5. [Network Configuration](#network-configuration)
6. [Connectivity Testing](#connectivity-testing)
7. [Troubleshooting](#troubleshooting)
8. [Learning Outcomes](#learning-outcomes)
9. [Security and Ethical Use](#security-and-ethical-use)
10. [Acknowledgements](#acknowledgements)
11. [Copyright and Credits](#copyright-and-credits)
12. [Author](#author)

---

## Project Overview

This repository documents an extra Week 1 project from the NetworkWalks Cybersecurity Internship (Batch B084J). I installed **Android-x86 9.0-r2** in a VMware Workstation 17 Player virtual machine named **Android9**, attached it to the custom lab network (*Network Walks*) that my Kali Linux machine also uses, and replaced its DHCP configuration with a **static IPv4 address** (`10.0.0.9/24`).

I then verified connectivity from the Android terminal to the Kali Linux machine, the gateway, a public IP address, and two domain names.

---

## Objectives

- Create and configure an Android-x86 virtual machine in VMware Workstation 17 Player.
- Boot the installer, work around graphics boot problems, and install Android-x86 to a virtual disk.
- Complete the initial Android setup and confirm the installed version.
- Move the Android VM from DHCP to a static IPv4 configuration on `10.0.0.0/24`.
- Verify local, gateway, public-IP, DNS and internet connectivity.
- Document the work with screenshot evidence.

---

## Virtual Machine Configuration

| Configuration | Value |
|---|---|
| VM name | Android9 |
| Hypervisor | VMware Workstation 17 Player |
| Guest OS | Android-x86 9.0-r2 |
| RAM | 5096 MB (approximately 5 GB) |
| CPU | 3 cores |
| Virtual disk | 9 GB (as shown in the VMware creation wizard) |
| Network adapter | Custom (Network Walks) |
| Network type | Custom lab network |

> **Disk size note:** the VMware creation wizard shows a 9 GB virtual disk, while the Android installer later displays a capacity of approximately 10.7 GB. Both values are reported here exactly as displayed by each tool.

### Lab Addressing

| Device | IP address | Subnet |
|---|---|---|
| Kali Linux | 10.0.0.2 | /24 |
| Android-x86 9.0 | 10.0.0.9 | /24 |
| Gateway | 10.0.0.1 | n/a |
| DNS 1 | 8.8.8.8 | n/a |
| DNS 2 | 8.8.4.4 | n/a |
| Subnet mask | 255.255.255.0 | n/a |

Network: `10.0.0.0/24`

---

## Installation Procedure

### 1. Create the Android9 virtual machine

I created the VM in VMware Workstation 17 Player and configured 3 CPU cores, about 5 GB of RAM, a virtual disk, and the *Network Walks* custom network adapter. Choosing the custom adapter at this stage places the VM on the same lab network as Kali Linux.

<p align="center">
  <img src="images/01-vmware-android-vm.png" alt="Android9 virtual machine in VMware Workstation 17 Player" width="800"><br>
  <em>Figure 1 – The Android9 virtual machine in VMware Workstation 17 Player.</em>
</p>

### 2. Boot from the Android-x86 9.0-r2 installation media

Booting from the installation media opens the Android-x86 boot menu, where I choose how to start the installer.

<p align="center">
  <img src="images/02-android-x86-boot-menu.png" alt="Android-x86 boot menu" width="800"><br>
  <em>Figure 2 – Android-x86 boot menu.</em>
</p>

### 3. Edit the GRUB boot entry

GRUB lets me edit the kernel command line for a single boot without changing the installation media. I opened the boot entry for editing so that I could add kernel parameters for graphics compatibility.

<p align="center">
  <img src="images/03-android-grub-configuration.png" alt="Android-x86 GRUB configuration" width="800"><br>
  <em>Figure 3 – GRUB boot entry opened for editing.</em>
</p>

### 4. Graphics troubleshooting options: `nomodeset` and `xforcevesa`

Android-x86 under a hypervisor can fail to start its graphical interface or show a black screen. Two optional kernel parameters are commonly used to work around this:

| Parameter | Purpose |
|---|---|
| `nomodeset` | Stops the kernel from loading video mode-setting drivers, falling back to basic graphics. |
| `xforcevesa` | Asks Android-x86 to use generic VESA graphics instead of hardware-specific drivers. |

These are **troubleshooting options, not mandatory installation steps**. Figure 4 shows the GRUB screen with `nomodeset` added.

<p align="center">
  <img src="images/04-android-grub-nomodeset.png" alt="GRUB command line with nomodeset" width="800"><br>
  <em>Figure 4 – GRUB command line with the nomodeset parameter.</em>
</p>

### 5. Boot the installer

After the boot entry was accepted, the Android-x86 environment started loading.

<p align="center">
  <img src="images/05-android-boot-process.png" alt="Android-x86 boot process" width="800"><br>
  <em>Figure 5 – Android-x86 boot process.</em>
</p>

### 6. Prepare the virtual disk and partition

The installer needs a partition to install to. I created a partition on the empty virtual disk using the installer's partitioning tool.

<p align="center">
  <img src="images/06-android-partition-creation.png" alt="Android-x86 installer partition creation" width="800"><br>
  <em>Figure 6 – Creating a partition on the virtual disk.</em>
</p>

### 7. Select the installation partition

I selected the newly created partition as the installation target. Choosing the correct partition matters because the installer writes to the selected partition. In this lab the disk is virtual, so no physical disk is at risk.

<p align="center">
  <img src="images/07-android-installation-partition-selection.png" alt="Android-x86 installation partition selection" width="800"><br>
  <em>Figure 7 – Selecting the partition for installation.</em>
</p>

### 8. Boot the installed Android-x86 system

After installation, the VM booted into the installed system and showed the Android boot logo.

<p align="center">
  <img src="images/08-android-boot-logo.png" alt="Android boot logo" width="800"><br>
  <em>Figure 8 – Android boot logo after installation.</em>
</p>

### 9. Complete the initial Android setup

The Android welcome screen starts the first-run setup.

<p align="center">
  <img src="images/09-android-welcome-screen.png" alt="Android welcome screen" width="800"><br>
  <em>Figure 9 – Android welcome screen at first boot.</em>
</p>

### 10. Verify the Android version

I confirmed the installed version in **Settings → About tablet**.

<p align="center">
  <img src="images/10-android-about-tablet.png" alt="Android About tablet screen" width="800"><br>
  <em>Figure 10 – About tablet screen confirming the installed Android version.</em>
</p>

---

## Network Configuration

### Architecture

```text
Windows Host
    |
VMware Workstation 17 Player
    |
Custom Network (Network Walks)
    |
10.0.0.0/24
    |
    |--- Kali Linux ......... 10.0.0.2
    |
    |--- Android-x86 ........ 10.0.0.9
    |
    |--- Gateway ............ 10.0.0.1
    |
    '--- Internet connectivity (reached through the gateway)
```

> This is a logical representation of the addressing used in the lab. It is based on my configuration and test results, and is not a claim about VMware's internal implementation.

### VMware adapter

The Android9 VM uses the **Custom (Network Walks)** network adapter, which is the same lab network as Kali Linux.

<p align="center">
  <img src="images/13-vmware-network-configuration.png" alt="VMware network configuration" width="800"><br>
  <em>Figure 13 – VMware network configuration for the lab network.</em>
</p>

### Static IPv4 configuration in Android

Android-x86 exposes its virtual network interface as **VirtWifi**. By default it uses DHCP. I changed it to a static configuration:

1. Open **Settings → Network & Internet → Wi-Fi**.
2. Select **VirtWifi** and choose **Edit**.
3. Open **Advanced options**.
4. Change **IP settings** from **DHCP** to **Static**.
5. Enter the values below and save.

| Field | Value |
|---|---|
| IP address | 10.0.0.9 |
| Gateway | 10.0.0.1 |
| Network prefix length | 24 |
| DNS 1 | 8.8.8.8 |
| DNS 2 | 8.8.4.4 |

<p align="center">
  <img src="images/11-android-network-details.png" alt="Android network details" width="800"><br>
  <em>Figure 11 – Android network details for VirtWifi.</em>
</p>

<p align="center">
  <img src="images/12-android-static-ip-configuration.png" alt="Android static IP configuration" width="800"><br>
  <em>Figure 12 – Static IP configuration (10.0.0.9/24, gateway 10.0.0.1, DNS 8.8.8.8 and 8.8.4.4).</em>
</p>

---

## Connectivity Testing

I ran these commands from the Android terminal:

```bash
ping -c 4 10.0.0.2
ping -c 4 10.0.0.1
ping -c 4 8.8.8.8
ping -c 4 google.com
ping -c 4 networkwalks.com
```

<p align="center">
  <img src="images/14-android-connectivity-tests.png" alt="Android terminal connectivity tests" width="800"><br>
  <em>Figure 14 – Connectivity tests from the Android terminal.</em>
</p>

| Destination | Result | What it verifies |
|---|---|---|
| `10.0.0.2` (Kali Linux) | 4 received, 0% packet loss | **Local VM-to-VM communication** on the lab subnet |
| `10.0.0.1` (gateway) | 4 received, 0% packet loss | **Gateway connectivity**, the path out of the subnet |
| `8.8.8.8` | 4 received, 0% packet loss | **Public IP connectivity** without relying on DNS |
| `google.com` | 4 received, 0% packet loss | **DNS resolution** plus internet reachability |
| `networkwalks.com` | 4 received, 0% packet loss | **DNS resolution** and reachability of the program's domain |

Testing in this order isolates faults: a failure at the gateway points to local network settings, a failure at `8.8.8.8` points to routing or NAT, and a failure only at domain names points to DNS.

The tests above were run from Android. A test from Kali Linux to Android is not part of this evidence and is not claimed.

---

## Troubleshooting

### Issues demonstrated in my screenshots

| Area | What it shows |
|---|---|
| GRUB boot configuration | The boot entry was edited in GRUB (Figures 3 and 4). |
| `nomodeset` | The parameter was added on the GRUB command line (Figure 4). |
| Disk partition selection | A partition was created and selected for installation (Figures 6 and 7). |
| Static IP configuration | VirtWifi was changed from DHCP to a static address (Figure 12). |
| VMware custom network selection | The VM uses the custom *Network Walks* adapter (Figures 1 and 13). |

### General troubleshooting recommendations (not claimed as encountered)

| Problem | Suggested approach |
|---|---|
| Boots to a console instead of the graphical interface | Retry the boot with graphics parameters such as `nomodeset` or `xforcevesa`. |
| Black screen or boot failure | Edit the GRUB entry again; confirm the installation media is connected. |
| Wrong partition selected | Review the partition list before confirming; on a virtual disk, recreate the VM if necessary. |
| No connectivity after setting a static IP | Check the prefix length (24), gateway (`10.0.0.1`), and that the adapter is the custom network. |
| Domain names fail but IP pings work | Check the DNS 1 and DNS 2 entries. |
| IP address conflict | Make sure each device has a unique address (Kali `10.0.0.2`, Android `10.0.0.9`) and that no DHCP lease overlaps. |

---

## Learning Outcomes

- Creating and sizing a virtual machine in VMware Workstation 17 Player.
- Booting and installing Android-x86 on a hypervisor.
- Editing GRUB kernel parameters for graphics troubleshooting.
- Partitioning a virtual disk and selecting an installation target.
- Switching an Android interface from DHCP to static IPv4.
- Understanding subnet masks and prefix length, gateways, and DNS servers.
- Verifying a network in layers: VM-to-VM, gateway, public IP, DNS.

---

## Security and Ethical Use

This lab is for **educational and authorized practice only**. Security tools and techniques must only be used on systems I own or have explicit written permission to test. No third-party systems were tested in this project.

---

## Acknowledgements

I thank **NetworkWalks** and the instructor for their guidance and for the opportunity to complete this hands-on cybersecurity project.

- **Training and guidance:** NetworkWalks Cybersecurity Internship and its instructor.
- **Practical implementation:** my own VM creation, installation, and network configuration.
- **Documentation and screenshots:** written and captured by me.

---

## Copyright and Credits

VMware, Android-x86, Kali Linux, and NetworkWalks belong to their respective owners. This repository claims ownership only of my own documentation and screenshots. All trademarks are the property of their owners.

---

## Author

<div align="center">

### Rajatava Ghosh

**Cybersecurity Student / Intern – NetworkWalks Cybersecurity Program, Batch B084J**

[![GitHub](https://img.shields.io/badge/GitHub-RaxingR-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RaxingR)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rajatava%20Ghosh-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rajatava-ghosh-265aa6316/)

</div>
