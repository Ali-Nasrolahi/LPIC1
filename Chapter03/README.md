# Configuring Hardware

- [Configuring Hardware](#configuring-hardware)
  - [Configuring the Firmware and Core Hardware](#configuring-the-firmware-and-core-hardware)
    - [Understanding the Role of Firmware](#understanding-the-role-of-firmware)
      - [The BIOS Startup](#the-bios-startup)
      - [The UEFI Startup](#the-uefi-startup)
    - [Device Interfaces](#device-interfaces)
      - [PCI Boards](#pci-boards)
      - [The USB Interface](#the-usb-interface)
      - [The GPIO Interface](#the-gpio-interface)
    - [The `/dev` Directory](#the-dev-directory)

## Configuring the Firmware and Core Hardware

### Understanding the Role of Firmware

All IBM-compatible workstations and servers utilize some type of built-in **firmware** to control how the installed operating system starts.

On older workstations and servers, this firmware was called the **Basic Input/Output System** (`BIOS`).

On newer workstations and servers, a new method, called the **Unified Extensible Firmware Interface** (`UEFI`), is responsible for maintaining the system hardware status and launching an installed operating system.

#### The BIOS Startup

One limitation of the original BIOS firmware was that it could read **only one** sector’s worth of data from a hard drive into memory to run.

To get around that limitation, most operating systems (including Linux and Microsoft Windows) split the boot process into two parts.

1. First, the BIOS runs a **boot loader program**, a small program that initializes the necessary hardware to find and run the full operating system program.

2. When booting from a hard drive, you must designate which hard drive, and partition on the hard drive, the BIOS should load the boot loader program from.
      - This is done by defining a **master boot record** (`MBR`).

#### The UEFI Startup

Intel created the **Extensible Firmware Interface** (`EFI`) in 1998 to address some of the limitations of `BIOS`. It was somewhat of a slow process, but by 2005, the idea caught on with other vendors, and the **Unified EFI** (`UEFI`) specification was adopted as a standard.

Instead of relying on a single boot sector on a hard drive to hold the boot loader program, UEFI specifies a special **disk partition**, called the **EFI System Partition** (`ESP`), to store boot loader programs.
> This allows for any size of boot loader program, plus the ability to store multiple boot loader programs for multiple operating systems.

### Device Interfaces

There are currently three popular standards used to connect devices.

#### PCI Boards

The **Peripheral Component Interconnect** (`PCI`) standard was developed in 1993 as a method for connecting hardware boards to PC motherboards.

The **PCI Express** (PCIe) standard is currently used on most server and desktop workstations to provide a common interface for external hardware devices.

Lots of different client devices use PCI boards to connect to a server or desktop workstation:

- *Internal hard drives*: Hard drives using the **Serial Advanced Technology Attachment** (`SATA`) and the **Small Computer System Interface** (`SCSI`) interface often use PCI boards to connect with workstations or servers.

- *External hard drives*: Network hard drives using the Fibre Channel standard provide a high-speed shared drive environment for server environments.

- *Network interface cards*: Hard-wired network cards allow you to connect the workstation or server to a **local area network** (`LAN`) using the common *RJ-45* cable standard.

- *Wireless cards*: PCI boards are available that support the **IEEE 802.11** standard for wireless connections to LANs.

- *Bluetooth devices*: The Bluetooth technology allows for short-distance wireless communication with other Bluetooth devices in a peer-to-peer network setup.

#### The USB Interface

The **Universal Serial Bus** (`USB`) interface has become increasingly popular due to its ease of use and its increasing support for high-speed data communication.

#### The GPIO Interface

The **General Purpose Input/Output** (`GPIO`) interface has become popular with small utility Linux systems, designed for controlling external devices for automation projects.
> The `GPIO` interface is ideal for supporting communications to external devices such as relays, lights, sensors, and motors.  
> Applications can read individual GPIO lines to determine the status of switches, turn relays on or off, or read digital values returned from any type of analog-to-digital sensors such as temperature or pressure sensors.

### The `/dev` Directory

**Device files** are files that the Linux kernel creates in the special `/dev` directory to *interface* with hardware devices.

As you add hardware devices such as USB drives, network cards, or hard drives to your system, Linux creates a file in the `/dev` directory representing that hardware device.

There are two types of device fi les in Linux, based on how Linux transfers data to the device:

- **Character device** files: Transfer data **one character** at a time. This method is often used for **serial devices** such as terminals and USB devices.

- **Block device** files: Transfer data in **large blocks of data**. This method is often used for *high-speed data* transfer devices such as hard drives and network cards.

Besides *device files*, Linux also provides a system called the **device mapper**.

The **device mapper** function is performed by the Linux kernel. It **maps** *physical block* devices to *virtual block* devices. These virtual block devices allow the system to intercept the data written
to or read from the physical device and perform some type of operation on them.
