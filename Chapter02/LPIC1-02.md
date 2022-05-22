# Managing Software

- [Managing Software](#managing-software)
  - [RPM packages](#rpm-packages)
    - [RPM Distributions and Conventions](#rpm-distributions-and-conventions)
    - [The rpm Command Set](#the-rpm-command-set)
    - [Extracting Data from RPMs](#extracting-data-from-rpms)
  - [Debian packages](#debian-packages)
    - [The `dpkg` Command Set](#the-dpkg-command-set)
    - [Using `apt-get`](#using-apt-get)
      - [Commands](#commands)
      - [Switches](#switches)
  - [Converting between Package Formats](#converting-between-package-formats)
  - [Library Management Commands](#library-management-commands)
    - [Displaying Shared Library Dependencies](#displaying-shared-library-dependencies)
    - [Re-loading the Library Cache](#re-loading-the-library-cache)
  - [Managing Processes](#managing-processes)
    - [ps [options]](#ps-options)
      - [Interpreting ps Output](#interpreting-ps-output)
    - [top](#top)
    - [Jobs, Foreground and Background Processes](#jobs-foreground-and-background-processes)
    - [Managing Process Priorities](#managing-process-priorities)
    - [Killing Processes](#killing-processes)
  - [END](#end)

## RPM packages

### RPM Distributions and Conventions

The convention for naming RPM packages is as follows:

```bash
packagename-a.b.c-x.arch.rpm
```

- **Package name**: is the name of the package.
  > such as samba.

- **Version number**: `a.b.c` is the package version number.
  > such as 2.2.7a.

- **Build number**: `x` is the build number (also known as the *release number*).
  > This number represents minor changes made by the *package maintainer*, **not** by the *program author*.

- **Architecture**: `arch` is a code for the package’s architecture.
  > i386 architecture represents a file compiled for any x86 CPU.  
  > x86_64 for the AMD64 platform.  
  > Scripts, documentation, and other CPU-independent packages generally use the **noarch** architecture code.  
  > Source RPMs, which use the **src** architecture code.

### The rpm Command Set

```bash
$ rpm [operation][options] [package-files|package-names]

rpm -i # Installs a package; system must not contain a package of the same name.
rpm -U # Installs a new package or upgrades an existing one.
rpm -V # or -y or --verify: Verifies a package
rpm -e # Uninstalls a package
rpm -q # Queries a package—finds if a package is installed, what files it con- tains, and so on.
-a or --all # Queries or verifies all packages.
--force # Forces installation of a package even when it means overwriting existing files or packages.
--nodeps # Specifies that no dependency checks be performed.
```

examples:

```bash
rpm -ivh --force # Use this for test your own rpm packages
rpm -Uvh samba-3.0.10-1.fc3.i386.rpm #  Upgrade a package
rpm -qa # query all packages
rpm -qi # displays information such as when and on what computer the binary package was built
```

### Extracting Data from RPMs

A good way to retrieve the original source code from a source RPM for compiling the software.

```bash
rpm2cpio X.rpm > X.cpio # outputs the cpio archive

# -i extract an archive and --make-directories to create directories:
cpio -i --make-directories < X.cpio

# OR

rpm2cpio X.rpm | cpio -i --make-directories
```

## Debian packages

### The `dpkg` Command Set

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

### Using `apt-get`

Repository list location:
`/etc/apt/sources.list`

#### Commands

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

## Converting between Package Formats

```bash
$ alien [options] file[...]

# options
--to-deb
--to-rpm
--to-slp
--to-tgz

--install               # installs the converted package and removes the converted file.
```

## Library Management Commands

### Displaying Shared Library Dependencies

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

### Re-loading the Library Cache

`ldconfig`: Reloads the cache

```bash
# Switches
ldconfig -v           # causes the program to summarize the directories and files it’s registering as it goes about its business.

ldconfig -N           # causes ldconfig to not perform its primary duty of updating the library cache.
                      # It will, though, update symbolic links to libraries, which is a secondary duty of this program.
ldconfig -n           # causes ldconfig to update the links contained in the directories specified on the command line.

ldconfig -X           # is the opposite of -N; it causes ldconfig to update the cache but not manage links.
ldconfig -f conffile  # configuration file from /etc/ld.so.conf by specifying it
ldconfig -C cachefile # Use a new cache file
ldconfig -r dir       # tells ldconfig to treat dir as if it were the root (/) directory.
ldconfig -p           # causes ldconfig to display the current cache
```

## Managing Processes

### ps [options]

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
