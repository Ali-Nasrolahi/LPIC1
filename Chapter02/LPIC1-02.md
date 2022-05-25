# Managing Software and Processes

- [Managing Software and Processes](#managing-software-and-processes)
  - [Looking at Package Concepts](#looking-at-package-concepts)
  - [Using RPM](#using-rpm)
    - [RPM Distributions and Conventions](#rpm-distributions-and-conventions)
    - [The rpm Command Set](#the-rpm-command-set)
      - [Installing and Updating RPM Packages](#installing-and-updating-rpm-packages)
      - [Querying RPM Packages](#querying-rpm-packages)
      - [Verifying RPM Packages](#verifying-rpm-packages)
      - [Removing RPM Packages](#removing-rpm-packages)
    - [Extracting Data from RPMs](#extracting-data-from-rpms)
    - [Using YUM](#using-yum)
    - [Using ZYpp](#using-zypp)
  - [Using Debian Packages](#using-debian-packages)
    - [Debian Package File Conventions](#debian-package-file-conventions)
    - [The `dpkg` Command Set](#the-dpkg-command-set)
    - [Looking at the APT Suite](#looking-at-the-apt-suite)
    - [Using `apt-cache`](#using-apt-cache)
    - [Using `apt-get`](#using-apt-get)
      - [Switches](#switches)
    - [Reconfiguring Packages](#reconfiguring-packages)
  - [Managing Shared Libraries](#managing-shared-libraries)
    - [Library Principles](#library-principles)
    - [Locating Library Files](#locating-library-files)
    - [Loading Dynamically](#loading-dynamically)
    - [Library Management Commands](#library-management-commands)
      - [Managing the Library Cache](#managing-the-library-cache)
    - [Troubleshooting Shared Library Dependencies](#troubleshooting-shared-library-dependencies)
  - [Managing Processes](#managing-processes)
    - [Examining Process Lists](#examining-process-lists)
    - [Viewing Processes with `ps`](#viewing-processes-with-ps)
      - [Interpreting ps Output](#interpreting-ps-output)
    - [top](#top)
    - [Jobs, Foreground and Background Processes](#jobs-foreground-and-background-processes)
    - [Managing Process Priorities](#managing-process-priorities)
    - [Killing Processes](#killing-processes)
  - [END](#end)

## Looking at Package Concepts

Linux distributions have created a system for bundling already compiled applications for distribution.

This bundle is called a **package**, and it consists of most of the files required to run a single application.

You can then *install*, *remove*, and *manage* the entire application as a **single package** rather than as a group of disjointed files.

Tracking software packages on a Linux system is called **package management**.

Two major package management system are:

- Red Hat package management (RPM)
- Debian package management (Apt)

Each package management system uses a different method of tracking application packages and files, but they both track similar information:

- **Application files**: The package database tracks each individual file as well as the folder where it’s located.

- **Library dependencies**: The package database tracks what library files are **required** for each application and can warn you if a dependent library file is not present when you install a package.

- **Application version**: The package database tracks version numbers of applications so that you know when an updated version of the application is available.

---

## Using RPM

### RPM Distributions and Conventions

The convention for naming RPM packages is as follows:

```bash
PACKAGE-NAME - VERSION - RELEASE . ARCHITECTURE .rpm
```

- **Package name**: is the name of the package.
  > such as samba.

- **Version number**: In format of `a.b.c` is the package version number.
  > such as 2.2.7a.

- **Release or Build number**: In format of `x` is the build number (also known as the *release number*).
  > This number represents minor changes made by the *package maintainer*, **not** by the *program author*.

- **Architecture**: `arch` is a code for the package’s architecture.
  > i386 architecture represents a file compiled for any x86 CPU.  
  > x86_64 for the AMD64 platform.  
  > Scripts, documentation, and other CPU-independent packages generally use the **noarch** architecture code.  
  > Source RPMs, which use the **src** architecture code.

### The rpm Command Set

```bash
$ rpm [operation][options] [package-files|package-names]

$ rpm -i # Installs a package; system must not contain a package of the same name.
$ rpm -U # Installs a new package or upgrades an existing one.
$ rpm -V # or -y or --verify: Verifies a package
$ rpm -e # Uninstalls a package
$ rpm -q # Queries a package—finds if a package is installed, what files it con- tains, and so on.
-a or --all # Queries or verifies all packages.
--force # Forces installation of a package even when it means overwriting existing files or packages.
--nodeps # Specifies that no dependency checks be performed.
```

#### Installing and Updating RPM Packages

```bash
 # Use this for test your own rpm packages
$ rpm -ivh --force
$ rpm -Uvh samba-3.0.10-1.fc3.i386.rpm #  Upgrade a package
```

#### Querying RPM Packages

```bash
 # query all packages
$ rpm -qa
$ rpm -qi # displays information such as when and on what computer the binary package was built
```

#### Verifying RPM Packages

```bash
 # or -y or --verify: Verifies a package
$ rpm -V
```

#### Removing RPM Packages

```bash
 # Uninstalls a package
$ rpm -e
```

### Extracting Data from RPMs

Occasionally you may need to extract files from an RPM package file without installing it.

The `rpm2cpio` utility is helpful in these situations. It allows you to build a `cpio` archive.

```bash
# outputs the cpio archive
$ rpm2cpio X.rpm > X.cpio

# -i extract an archive and --make-directories to create directories:
$ cpio -i --make-directories < X.cpio

# OR

$ rpm2cpio X.rpm | cpio -i --make-directories
```

---

### Using YUM

Each Linux distribution has its own central clearinghouse of packages, called a **repository**.

The **repository** contains software packages that have been *tested* and known to *install* and *work* correctly in the distribution environment.

The core tool used for working with Red Hat repositories is the `YUM` utility (short for *YellowDog Update Manager*.

> originally developed for the YellowDog Linux distribution).

The `yum` command uses the `/etc/yum.repos.d/` directory to hold files that list the different repositories it checks for packages.

```bash
# The basic `yum` command syntax is:
$ yum [OPTIONS] [COMMAND] [PACKAGE…]
```

```bash
# Some of command associated with `yum`:
$ yum install      [package]  # Installs the specified package
$ yum reinstall    [package]  # Reinstalls the specified package
$ yum remove       [package]  # Removes a package from the system
$ yum search       [keywords] # Searches repository package names and descriptions for specified keyword
$ yum info         [package]  # Displays information about the specified package
$ yum groupinstall [package]  # Installs the specified package group
```

### Using ZYpp

The openSUSE has created its own package management tool called `ZYpp` (also called `libzypp`).

Its `zypper` command allows you to *query*, *install*, and *remove* software packages on your system directly from an openSUSE repository.

```bash
# basic commands of zypper (same functionality as yum's)
$ zyppper install [package]
$ zyppper info    [package]
$ zyppper remove  [package]
$ zyppper search  [keyword]
$ zyppper update
```

---

## Using Debian Packages

### Debian Package File Conventions

Debian bundles application files into a single `.deb` package file for distribution that uses the following filename format:

```bash
PACKAGE-NAME - VERSION - RELEASE _ ARCHITECTURE .deb
```

This filenaming convention for `.deb` packages is very similar to the `.rpm` file format.

> However, in the `ARCHITECTURE` , you typically find `amd64` , denoting it was optimized for the **AMD64/Intel64 CPU** architecture.

### The `dpkg` Command Set

The core tool to use for handling `.deb` files is the `dpkg` program, which is a command-line utility that has options for *installing*, *updating*, and *removing* `.deb` package files on your Linux system.

```bash
$ dpkg [options][action] [package-files|package-name]

dpkg -i           # or --install Installs a package
dpkg --configure  # Reconfigures an installed package: runs the post-installation script to set site-specific options
dpkg -r           # or --remove Removes a package, but leaves configuration files intact
dpkg -P           # or --purge Removes a package, including configuration files 
dpkg -p           # or --print-avail Displays information about an installed package
dpkg -I           # or --info Displays information about an uninstalled package file
```

Examples:

```bash
dpkg -i samba-common_3.0.10-1_powerpc.deb
dpkg -r samba
dpkg -p samba-common
```

### Looking at the APT Suite

The **Advanced Package Tool** (`APT`) suite is used for working with Debian repositories.

This includes the `apt-cache` program that provides information about the package database, and the `apt-get` program that does the work of installing, updating, and removing packages.

The APT suite of tools relies on the `/etc/apt/sources.list` file to identify the locations of where to look for repositories.

### Using `apt-cache`

```bash
# apt-cache - query the APT cache
$ apt-cache depends [package] # Displays the dependencies required for the package
$ apt-cache pkgnames          # Shows all the packages installed on the system
$ apt-cache search  [keyword] # Displays the name of packages matching the specified item
$ apt-cache showpkg [package] # Lists information about the specified package
$ apt-cache stats   [package] # Displays package statistics for the system
$ apt-cache unmet   [package] # Shows any unmet dependencies for all installed packages or the specified installed package
```

### Using `apt-get`

```bash
$ apt-get [options][command] [package-names]

apt update            # Obtains updated information on packages.
apt upgrade           # Upgrades all installed packages to the newest versions available, based on locally stored information on available packages.
apt dist-upgrade      # Similar to upgrade, but performs “smart” conflict resolution to avoid upgrading a package if that would break a dependency.

apt install           # Installs a package by package name
apt source            # Retrieves the newest available source package file by package file name
apt clean             # Performs housekeeping to help clear out information on retrieved files from the Debian package database.
apt autoclean         # Similar to clean, but removes information only on packages that can no longer be downloaded.

apt remove            # Removes a specified package by package name
apt purge             # Removes a package as well as its configurations file. 

```

#### Switches

```bash
-d or --download-only   # Downloads package files but does not install them.
-f or --fix-broken      # Attempts to fix a system on which dependencies are unsatisfied.

# Ignores all package files that can’t be retrieved (because of network errors, missing files, or the like).
-m, --ignore-missing, or --fix-missing
--no-upgrade            # Causes apt-get to not upgrade a package if an older version is already installed.
```

### Reconfiguring Packages

If the package required configuration when it was installed.

Instead of purging the package and reinstalling it, you can employ the handy `dpkg-reconfigure` tool.

```bash
# E.G
$ dpkg-reconfigure [package]
```

> It would be worthwhile to run the `debconf-show` command and record the settings before and after running the `dpkg-reconfigure` utility.

---

## Managing Shared Libraries

### Library Principles

A **system library** is a collection of items, such as program *functions*.

> Functions are self-contained code modules that perform a specific task within an application, such as opening and reading a data file.

The benefit of splitting functions into separate library files is that multiple applications that use the same functions can **share the same library** files.

These files full of functions make it easier to **distribute** applications, but also make it more complicated to keep track of what library files are installed with which applications.

Linux supports two different fl avors of libraries:

- One is **static** libraries
  > that are copied into an application when it is compiled.
- The other flavor is **shared** libraries
  > where the library functions are copied into memory and bound to the application when the program is launched.

**Shared library** file employs the following filename format:

```bash
libLIBRARYNAME.so.VERSION
```

### Locating Library Files

the system will search for the function’s library file in a specific order, as shown:

1. `LD_LIBRARY_PATH` environment variable
2. Program’s `PATH` environment variable
3. `/etc/ld.so.conf.d/` folder
4. `/etc/ld.so.conf` file
5. `/lib*/` and `/usr/lib*/` folders

### Loading Dynamically

- When a program is started, the **dynamic linker** (also called the *dynamic linker/loader*) is responsible for **finding** the program’s needed library functions.

- After they are located, the *dynamic linker* will **copy** them into memory and bind them to the program.

### Library Management Commands

#### Managing the Library Cache

The **library cache** is a catalog of library directories and all the various libraries contained within them.

When new libraries or library directories are added to the system, this library cache file must be updated.

However, it is not a simple text file you can just edit. Instead, you have to employ the `ldconfig` command.

If you are troubleshooting the library cache, you can easily see what library files are **cataloged** by using the `ldconfig -v` command.

### Troubleshooting Shared Library Dependencies

The `ldd` utility can come in handy if you need to track down missing library files for an application. It displays a **list of the library** files required for the specified application.

```bash
$ ldd /bin/ls
    librt.so.1 => /lib/librt.so.1 (0x0000002a9566c000)
    libncurses.so.5 => /lib/libncurses.so.5 (0x0000002a95784000)
    libacl.so.1 => /lib/libacl.so.1 (0x0000002a958ea000)
    libc.so.6 => /lib/libc.so.6 (0x0000002a959f1000)
    libpthread.so.0 => /lib/libpthread.so.0 (0x0000002a95c17000)
    /lib64/ld-linux-x86-64.so.2 (0x0000002a95556000)
    libattr.so.1 => /lib/libattr.so.1 (0x0000002a95dad000)
```

---

## Managing Processes

### Examining Process Lists

Linux calls each running program a **process**. The Linux system assigns each process a **process ID** (PID) and manages how the process uses *memory* and *CPU time* based on that PID.

When a Linux system first boots, it starts a special process called the `init` process.

The `init` process is the core of the Linux system; it runs scripts that start all of the other processes running on the system.

### Viewing Processes with `ps`

> CAUTION: watchout switches with dash (-).In other words, *-x* and *x* are **not same**.

- `-A and -e` ==> display all the processes on the system.
- `x` ==> displays all processes owned by the user. Also increases the amount of information that’s displayed.
- `(-u | -U | --User)` User ==> Display one user’s processes. The user variable may be a *username* or a *user ID*.
- *Display extra information*: The `-f, -l, j, l, u, and v` options all expand the information provided in the ps output.
- *Display process hierarchy*: The `-H, -f, and --forest` options group processes and use indentation to show the hierarchy of relationships between processes.
- `-w and w` options tell ps that output *must not* exceeded 80 chars per line. (useful for saving ps output to a file)

> Favorite combination:

-`ps aux` and `ps fax`.

#### Interpreting ps Output

- `Username`: The name of the user who runs the programs.
- `PID`: The process ID (PID) is a number that’s associated with the process. (useful for modify or kill the process).
- `PPID`: parent process ID (PPID) identifies the process’s parent.
- `TTY`: The teletype (TTY) is a code used to identify a terminal.
- CPU time: The `TIME` and `%CPU` headings are two measures of CPU time used.
  - First indicates the total amount of CPU time consumed
  - Second represents the percentage of CPU time the process is using when ps executes.
- CPU priority (nice level): The `NI` column lists these priority codes. Default value is 0.
  - *Positive* values represent **reduced** priority, while *negative* values represent **increased** priority.
- Memory use
  - `RSS`   is resident set size (the memory used by the program and its data)
  - `MEM`   is the percentage of memory the program is using
  - `SHARE` column is memory that’s shared with other processes (such as shared libraries)
- `Command`: is the command used to launch the process

### top

> Switches

- `-d delay`: This specifies the delay between updates, which is normally 5 seconds.
- `-p pid`: If you want to monitor specific processes, you can list them using this option.
- `-n iter`: ou can tell top to display a certain number of updates (iter) and then quit.
- `-b`: This specifies batch mode, in which top doesn’t use the normal screen update commands

> Commands

- `h or ?`:  These keystrokes display help information
- `k`: You can kill a process with this command
- `q`: This option quits from top
- `r`: You can change a process’s priority with this command.
- `s`: This command changes the display’s update rate.
- `P`: This sets the display to sort by **CPU usage**
- `M`: You can change the display to sort by **memory usage** with this command.

### Jobs, Foreground and Background Processes

- `jobs`: shows  all jobs in this terminal.
- Ctrl+Z normally pauses the program and gives you control of the terminal
- `fg [num]`: restore job [num to foreground]
- An alternative to launching a program, using Ctrl+Z, and typing bg to run a program in the background is to append an ampersand `&` to the command when launching the program.

### Managing Process Priorities

`nice [argument] [command [command-arguments]]`

3 ways to give a specific nice-level:

nice-level's range is: **-20 to +19**

```bash
nice -12 number-crunch data.txt               # by -num (dash and nice level)
nice -n 12 number-crunch data.txt             # by -n num
nice --adjustment=12 number-crunch data.txt   # by --adjustment=num
```

To renice a running process:

`renice priority [[-p] pids] [[-g] pgrps] [[-u] users]`

```bash
renice 7 16580 -u pdavison tbaker

#This command sets the priority to 7 for PID 16580 and for all processes owned by pdavison and tbaker.
```

- specify one or more PIDs (pids)

- one or more group IDs (pgrps)

- one or more usernames (users).

### Killing Processes

```bash
kill -l               # Shows all signals
kill -s signal pid    # Sends a signal to the process (specified by pid) 
```

`killall [options] [--] name [...]`

```bash
killall vi            # kills all progrmas named vi
```

## END
