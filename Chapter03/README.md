# Configuring Hardware

- [Configuring Hardware](#configuring-hardware)
  - [Configuring the Firmware and Core Hardware](#configuring-the-firmware-and-core-hardware)
    - [Understanding the Role of Firmware](#understanding-the-role-of-firmware)
      - [The BIOS Startup](#the-bios-startup)
      - [The UEFI Startup](#the-uefi-startup)
    - [Device Interfaces](#device-interfaces)

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
