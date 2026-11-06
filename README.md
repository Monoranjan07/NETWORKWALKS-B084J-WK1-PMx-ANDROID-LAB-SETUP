<div align="center">

# NETWORKWALKS-B084J-WK1-PMx-ANDROID-LAB-SETUP

### Android-x86 9.0-r2 on VMware Workstation 17 Player: Installation, Static IPv4 Configuration and Connectivity Verification on a Custom Lab Network

**NetworkWalks Cybersecurity Internship · Batch B084J · Week 01 · PMx Extra Project**

![VMware](https://img.shields.io/badge/Hypervisor-VMware%20Workstation%2017%20Player-607078?style=for-the-badge&logo=vmware&logoColor=white)
![Android-x86](https://img.shields.io/badge/Guest%20OS-Android--x86%209.0--r2-3DDC84?style=for-the-badge&logo=android&logoColor=white)
![Kali Linux](https://img.shields.io/badge/Lab%20Peer-Kali%20Linux-557C94?style=for-the-badge&logo=kalilinux&logoColor=white)
![Network](https://img.shields.io/badge/Network-10.0.0.0%2F24-blue?style=for-the-badge)
![Addressing](https://img.shields.io/badge/Android%20IPv4-Static%20(10.0.0.9)-informational?style=for-the-badge)

[![GitHub](https://img.shields.io/badge/GitHub-RaxingR-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RaxingR)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rajatava%20Ghosh-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rajatava-ghosh-265aa6316/)

</div>

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Objectives](#objectives)
3. [Lab Environment](#lab-environment)
4. [Installation Procedure](#installation-procedure)
5. [Network Configuration](#network-configuration)
6. [Connectivity Testing](#connectivity-testing)
7. [Troubleshooting](#troubleshooting)
8. [Screenshot Index](#screenshot-index)
9. [Repository Structure](#repository-structure)
10. [Learning Outcomes](#learning-outcomes)
11. [Security and Ethical Use](#security-and-ethical-use)
12. [Acknowledgements](#acknowledgements)
13. [Copyright and Credits](#copyright-and-credits)
14. [Author](#author)

---

## Project Overview

This repository documents the practical work I completed for the Week 01 PMx extra project of the NetworkWalks Cybersecurity Internship (Batch B084J).

I built an Android-x86 9.0-r2 virtual machine named **Android9** in VMware Workstation 17 Player, connected it to the same custom lab network (*Network Walks*) that my Kali Linux machine uses, and moved its network interface from DHCP to a **static IPv4 address (10.0.0.9/24)**. I then tested connectivity from the Android terminal toward the Kali Linux address, the gateway, a public IP address and two domain names.

Android-x86 is a useful addition to a security lab because it gives a mobile operating system environment that runs on ordinary virtualisation software and can share a subnet with other lab machines.

---

## Objectives

- Create and configure an Android-x86 virtual machine in VMware Workstation 17 Player.
- Boot the installer, handle the GRUB boot menu, and install Android-x86 to a virtual disk.
- Complete the first-run Android setup and confirm the installed version.
- Replace the default DHCP configuration with a static IPv4 configuration on `10.0.0.0/24`.
- Verify local, gateway, public-IP, DNS and internet connectivity in separate steps.
- Document each stage with screenshot evidence.

---

## Lab Environment

### Virtual machine configuration

| Configuration   | Value                                         |
| --------------- | --------------------------------------------- |
| VM name         | Android9                                      |
| Hypervisor      | VMware Workstation 17 Player                  |
| Guest OS        | Android-x86 9.0-r2                            |
| RAM             | 5096 MB (approximately 5 GB)                  |
| CPU             | 3 cores                                       |
| Virtual disk    | 9 GB (as shown in the VMware creation wizard) |
| Network adapter | Custom (Network Walks)                        |
| Network type    | Custom lab network                            |

> **Disk size note:** the VMware creation wizard shows a 9 GB virtual disk, while the Android-x86 installer later reports a capacity of approximately 10.7 GB. I have recorded each value exactly as the respective tool displayed it and have not altered either figure.

### Lab addressing

| Device          | IP address    | Subnet |
| --------------- | ------------- | ------ |
| Kali Linux      | 10.0.0.2      | /24    |
| Android-x86 9.0 | 10.0.0.9      | /24    |
| Gateway         | 10.0.0.1      | n/a    |
| DNS 1           | 8.8.8.8       | n/a    |
| DNS 2           | 8.8.4.4       | n/a    |
| Subnet mask     | 255.255.255.0 | n/a    |

**Network:** `10.0.0.0/24`

---

## Installation Procedure

### Step 1: Create the Android9 virtual machine

I created a new virtual machine in VMware Workstation 17 Player and assigned 3 CPU cores, about 5 GB of RAM, a virtual disk, and the **Custom (Network Walks)** network adapter. Selecting the custom adapter at creation time places the VM on the same lab network as Kali Linux from its first boot.

![Android9 virtual machine in VMware Workstation 17 Player](01-vmware-android-vm.png)

*Figure 1: The Android9 virtual machine and its hardware summary in VMware Workstation 17 Player.*

### Step 2: Boot from the Android-x86 9.0-r2 installation media

Starting the VM from the Android-x86 9.0-r2 installation image opens the boot menu. This menu determines whether the system starts in live mode or launches the installer, and it is also where boot parameters can be adjusted.

![Android-x86 boot menu](images/02-android-x86-boot-menu.png)

*Figure 2: The Android-x86 boot menu shown when the VM starts from the installation media.*

### Step 3: Open the GRUB entry for editing

GRUB allows the kernel command line of a boot entry to be edited for a single boot without changing the installation media. I opened the selected entry for editing so that I could review, and if needed extend, the kernel parameters.

![GRUB boot entry opened for editing](images/03-android-grub-configuration.png)

*Figure 3: The GRUB boot entry opened for editing.*

### Step 4: Graphics troubleshooting parameters

Android-x86 running under a hypervisor can sometimes fail to bring up its graphical interface, dropping to a text console or a black screen. Two kernel parameters are commonly used as workarounds:

| Parameter    | Purpose                                                                                      |
| ------------ | -------------------------------------------------------------------------------------------- |
| `nomodeset`  | Prevents the kernel from loading video mode-setting drivers, falling back to basic graphics. |
| `xforcevesa` | Makes Android-x86 use generic VESA graphics instead of hardware-specific drivers.            |

Both are **optional troubleshooting measures, not mandatory installation steps**. My screenshot evidence (Figure 4) shows `nomodeset` on the GRUB command line. I do not claim that either parameter was required for the installation to succeed.

![GRUB command line containing nomodeset](images/04-android-grub-nomodeset.png)

*Figure 4: The GRUB command line with the `nomodeset` parameter present.*

### Step 5: Boot the installer environment

With the boot entry accepted, the Android-x86 environment began loading.

![Android-x86 boot process](images/05-android-boot-process.png)

*Figure 5: The Android-x86 boot process after the GRUB entry was accepted.*

### Step 6: Prepare the virtual disk and partition

The installer needs a formatted partition to write to, and a new virtual disk is empty. I used the installer's partitioning step to create a partition on the Android9 virtual disk. Because the disk is virtual, this step affects only the VM and not the Windows host.

![Partition creation on the virtual disk](images/06-android-partition-creation.png)

*Figure 6: Creating a partition on the virtual disk in the Android-x86 installer.*

### Step 7: Select the installation partition

I then chose the partition that the installer would use as the installation target. Selecting the right partition is the key decision in this stage, since the installer writes the system to whichever partition is chosen.

![Installation partition selection](images/07-android-installation-partition-selection.png)

*Figure 7: Selecting the partition for the Android-x86 installation.*

### Step 8: Boot the installed system

After installation, the VM started from the virtual disk and displayed the Android boot logo, confirming that the system had been written to disk and could boot independently.

![Android boot logo](images/08-android-boot-logo.png)

*Figure 8: The Android boot logo shown when starting the installed system.*

### Step 9: Complete the initial Android setup

The welcome screen begins Android's first-run setup, which must be completed before the Settings application and the rest of the interface are available.

![Android welcome screen](images/09-android-welcome-screen.png)

*Figure 9: The Android welcome screen at first boot.*

### Step 10: Verify the installed Android version

I confirmed the installed release in the **About tablet** screen of Settings, so the result is checked on the running system and not assumed from the name of the installation image.

![About tablet screen](images/10-android-about-tablet.png)

*Figure 10: The About tablet screen used to verify the installed Android version.*

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
    |--- Kali Linux ........... 10.0.0.2
    |
    |--- Android-x86 9.0 ...... 10.0.0.9   (static)
    |
    |--- Gateway .............. 10.0.0.1
    |
    '--- Internet connectivity (reached via the gateway)
```

> This diagram is a logical view of the addressing I configured and tested. It is not a claim about the internal implementation of VMware's networking or about the physical topology of the host.

### VMware network adapter

The Android9 VM uses the **Custom (Network Walks)** adapter. This is the same lab network used by my Kali Linux machine, which is why both machines can be given addresses in `10.0.0.0/24`.

![VMware network configuration](images/13-vmware-network-configuration.png)

*Figure 13: The VMware network configuration for the lab network used by the Android9 VM.*

### Static IPv4 configuration in Android

Android-x86 presents its virtual network interface as a Wi-Fi network named **VirtWifi**. It obtains its address by DHCP by default. A fixed address is more convenient in a lab because other machines and test commands can always refer to the same IP.

I changed the interface from DHCP to Static as follows:

1. Open **Settings → Network & Internet → Wi-Fi**.
2. Select **VirtWifi** and choose **Edit**.
3. Expand **Advanced options**.
4. Change **IP settings** from **DHCP** to **Static**.
5. Enter the values in the table below and save.

| Field                 | Value      |
| --------------------- | ---------- |
| IP address            | `10.0.0.9` |
| Gateway               | `10.0.0.1` |
| Network prefix length | `24`       |
| DNS 1                 | `8.8.8.8`  |
| DNS 2                 | `8.8.4.4`  |

A prefix length of 24 is the same as the subnet mask `255.255.255.0`. The gateway address is the router for traffic leaving the subnet, and the two DNS entries are the resolvers Android uses to turn domain names into addresses.

![Android network details for VirtWifi](images/11-android-network-details.png)

*Figure 11: Android network details for the VirtWifi connection.*

![Android static IP configuration](images/12-android-static-ip-configuration.png)

*Figure 12: Static IP configuration with address 10.0.0.9, gateway 10.0.0.1, prefix length 24, and DNS 8.8.8.8 and 8.8.4.4.*

---

## Connectivity Testing

I ran the following commands from the Android terminal:

```bash
ping -c 4 10.0.0.2
ping -c 4 10.0.0.1
ping -c 4 8.8.8.8
ping -c 4 google.com
ping -c 4 networkwalks.com
```

The `-c 4` option sends exactly four echo requests and then stops.

![Connectivity tests from the Android terminal](images/14-android-connectivity-tests.png)

*Figure 14: Connectivity tests run from the Android terminal.*

### Results

| Destination             | Result                     | What it verifies                                                       |
| ----------------------- | -------------------------- | ---------------------------------------------------------------------- |
| `10.0.0.2` (Kali Linux) | 4 received, 0% packet loss | Local VM-to-VM communication on the lab subnet                         |
| `10.0.0.1` (gateway)    | 4 received, 0% packet loss | Gateway connectivity, the path out of the local subnet                 |
| `8.8.8.8`               | 4 received, 0% packet loss | Public IP connectivity without involving DNS                           |
| `google.com`            | 4 received, 0% packet loss | DNS resolution, followed by internet reachability                      |
| `networkwalks.com`      | 4 received, 0% packet loss | DNS resolution and reachability of the training provider's domain      |

### Why the order matters

The tests move outward one layer at a time, so a failure shows where the problem starts:

| Test type                        | If it fails, suspect                                                           |
| -------------------------------- | ------------------------------------------------------------------------------ |
| Local VM-to-VM (`10.0.0.2`)      | Adapter selection, IP or prefix settings, or a firewall on the peer            |
| Gateway (`10.0.0.1`)             | Wrong gateway address or the VM not attached to the intended network           |
| Public IP (`8.8.8.8`)            | Routing or NAT beyond the gateway                                              |
| DNS resolution (`google.com`)    | DNS 1 or DNS 2 entries, when pings to a public IP still work                   |
| Internet reachability            | Any of the above, or filtering of the specific destination                     |

> **Scope of evidence:** these tests were run **from Android**. I have not included a Kali-to-Android test, and no result for one is claimed in this documentation.

---

## Troubleshooting

### Issues evidenced in my screenshots

| Area                            | What my screenshots show                                                          | Reference     |
| ------------------------------- | --------------------------------------------------------------------------------- | ------------- |
| GRUB boot configuration         | The boot entry was opened for editing in GRUB                                     | Figures 3, 4  |
| `nomodeset`                     | The parameter appears on the GRUB command line                                    | Figure 4      |
| Disk partition selection        | A partition was created and then selected as the installation target              | Figures 6, 7  |
| Static IP configuration         | VirtWifi was configured with a static address instead of DHCP                     | Figure 12     |
| VMware custom network selection | The VM is attached to the custom *Network Walks* network                          | Figures 1, 13 |

### General recommendations (not claimed as problems I encountered)

| Problem                                               | Suggested approach                                                                                                  |
| ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Boots to a console instead of the graphical interface | Edit the GRUB entry and retry with graphics parameters such as `nomodeset` or `xforcevesa`.                         |
| Black screen or other boot issues                     | Re-edit the boot entry, confirm the installation image is still attached, and review the VM's display settings.     |
| Wrong installation partition selected                 | Read the partition list carefully before confirming. On a virtual disk, the VM can be recreated if necessary.       |
| No connectivity after setting a static IP             | Check the address, prefix length (`24`) and gateway (`10.0.0.1`), and confirm the adapter is the custom network.    |
| Wrong network adapter in VMware                       | Open the VM's network adapter settings and select the custom *Network Walks* network.                               |
| Pings to IPs work but domain names fail               | Recheck the DNS 1 and DNS 2 values (`8.8.8.8`, `8.8.4.4`).                                                          |
| IP address conflict                                   | Give each device a unique address (Kali `10.0.0.2`, Android `10.0.0.9`) and avoid overlap with any DHCP range.      |

---

## Screenshot Index

| Figure | File                                                  | Content                                           |
| ------ | ----------------------------------------------------- | ------------------------------------------------- |
| 1      | `images/01-vmware-android-vm.png`                     | Android9 VM in VMware Workstation 17 Player       |
| 2      | `images/02-android-x86-boot-menu.png`                 | Android-x86 boot menu                             |
| 3      | `images/03-android-grub-configuration.png`            | GRUB entry opened for editing                     |
| 4      | `images/04-android-grub-nomodeset.png`                | GRUB command line with `nomodeset`                |
| 5      | `images/05-android-boot-process.png`                  | Boot process                                      |
| 6      | `images/06-android-partition-creation.png`            | Partition creation                                |
| 7      | `images/07-android-installation-partition-selection.png` | Installation partition selection               |
| 8      | `images/08-android-boot-logo.png`                     | Android boot logo                                 |
| 9      | `images/09-android-welcome-screen.png`                | Welcome screen                                    |
| 10     | `images/10-android-about-tablet.png`                  | About tablet / version check                      |
| 11     | `images/11-android-network-details.png`               | VirtWifi network details                          |
| 12     | `images/12-android-static-ip-configuration.png`       | Static IP configuration                           |
| 13     | `images/13-vmware-network-configuration.png`          | VMware network configuration                      |
| 14     | `images/14-android-connectivity-tests.png`            | Terminal connectivity tests                       |

---

## Repository Structure

```text
NETWORKWALKS-B084J-WK1-PMx-ANDROID-LAB-SETUP/
├── README.md
└── images/
    ├── 01-vmware-android-vm.png
    ├── 02-android-x86-boot-menu.png
    ├── 03-android-grub-configuration.png
    ├── 04-android-grub-nomodeset.png
    ├── 05-android-boot-process.png
    ├── 06-android-partition-creation.png
    ├── 07-android-installation-partition-selection.png
    ├── 08-android-boot-logo.png
    ├── 09-android-welcome-screen.png
    ├── 10-android-about-tablet.png
    ├── 11-android-network-details.png
    ├── 12-android-static-ip-configuration.png
    ├── 13-vmware-network-configuration.png
    └── 14-android-connectivity-tests.png
```

---

## Learning Outcomes

- Creating and sizing a virtual machine in VMware Workstation 17 Player.
- Attaching a VM to a custom virtual network shared with another lab machine.
- Booting and installing Android-x86 on a hypervisor.
- Understanding how GRUB boot entries can be edited and what `nomodeset` and `xforcevesa` do.
- Partitioning a virtual disk and selecting an installation target.
- Converting an Android network interface from DHCP to static IPv4.
- Relating prefix length to subnet mask, and understanding the roles of gateway and DNS.
- Testing a network in layers: VM-to-VM, gateway, public IP, DNS and internet reachability.

---

## Security and Ethical Use

This project is for **educational purposes and authorised practice only**. Security tools and techniques should be used only on systems that I own or have explicit permission to test. All activity in this project took place inside my own virtual lab, and no third-party systems were tested or attacked.

---

## Acknowledgements

I thank **NetworkWalks** and the instructor for their guidance and for the opportunity to complete this hands-on cybersecurity project.

| Contribution                    | Source                                                  |
| ------------------------------- | ------------------------------------------------------- |
| Training and guidance           | NetworkWalks Cybersecurity Internship and its instructor |
| Practical implementation        | My own VM creation, installation and network configuration |
| Documentation and screenshots   | Written and captured by me                               |

---

## Copyright and Credits

VMware, Android-x86, Kali Linux and NetworkWalks, along with their names and logos, belong to their respective owners. I claim ownership only of my own documentation and screenshots in this repository. All trademarks are the property of their respective holders.

---

## Author

### Rajatava Ghosh

**Cybersecurity Student / Intern, NetworkWalks Cybersecurity Program, Batch B084J**

[![GitHub](https://img.shields.io/badge/GitHub-RaxingR-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RaxingR)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Rajatava%20Ghosh-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rajatava-ghosh-265aa6316/)

<!--
Add these only after confirming they are yours:
- Repository URL:      https://github.com/RaxingR/NETWORKWALKS-B084J-WK1-PMx-ANDROID-LAB-SETUP
- LinkedIn project post: <your own post URL>
-->
