# Configuring Hardware

- [Configuring Hardware](#configuring-hardware)
  - [Configuring the Firmware and Core Hardware](#configuring-the-firmware-and-core-hardware)
    - [Understanding the Role of Firmware](#understanding-the-role-of-firmware)
      - [The BIOS Startup](#the-bios-startup)
      - [The UEFI Startup](#the-uefi-startup)

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
