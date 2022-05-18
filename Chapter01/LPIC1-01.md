# Notes from Exploring Linux Command-Line Tools

- [Notes from Exploring Linux Command-Line Tools](#notes-from-exploring-linux-command-line-tools)
  - [Reaching a Shell](#reaching-a-shell)
  - [Exploring Your Linux Shell Options](#exploring-your-linux-shell-options)
    - [Bash](#bash)
    - [Dash](#dash)
    - [KornShell](#kornshell)
    - [tcsh](#tcsh)
    - [Z shell](#z-shell)
    - [`/bin/sh`](#binsh)
  - [Using a Shell](#using-a-shell)
    - [Quoting Metacharacters](#quoting-metacharacters)
    - [Navigating the Directory Structure](#navigating-the-directory-structure)
    - [Understanding Internal and External Comamands](#understanding-internal-and-external-comamands)
  - [Using Environment Variables](#using-environment-variables)
  - [Editing Text Files](#editing-text-files)
  - [Processing Text Using Filters](#processing-text-using-filters)
    - [File-Combining Commands](#file-combining-commands)
      - [expand and unexpand](#expand-and-unexpand)
    - [File-Transforming Commands](#file-transforming-commands)
      - [Uncovering with `od`](#uncovering-with-od)
      - [Separating with `split`](#separating-with-split)
    - [File-Formatting Commands](#file-formatting-commands)
      - [Organizing with `sort`](#organizing-with-sort)
      - [Numbering with `nl`](#numbering-with-nl)
    - [File-Viewing Commands](#file-viewing-commands)
      - [Using `more`or `less`](#using-moreor-less)
      - [Looking at files with `head`](#looking-at-files-with-head)
      - [Viewing Files with `tail`](#viewing-files-with-tail)
    - [File-Summarizing](#file-summarizing)
      - [Counting with `wc`](#counting-with-wc)
      - [Pulling Out Portions with `cut`](#pulling-out-portions-with-cut)
    - [Using Streams, Redirection, and Pipes](#using-streams-redirection-and-pipes)
      - [Redirecting Input and Output](#redirecting-input-and-output)
    - [Regex](#regex)
      - [grep](#grep)
      - [sed](#sed)
    - [Managing Processes](#managing-processes)
      - [ps [options]](#ps-options)
        - [Interpreting ps Output](#interpreting-ps-output)
      - [top](#top)
      - [Jobs, Foreground and Background Processes](#jobs-foreground-and-background-processes)
      - [Managing Process Priorities](#managing-process-priorities)
      - [Killing Processes](#killing-processes)
  - [END](#end)

## Reaching a Shell

you can typically reach a command-line terminal by pressing the `Ctrl+Alt+F2` key combination.
> (which gets you to the `tty2` terminal)

---

## Exploring Your Linux Shell Options

### Bash

The GNU **Bourne Again shell** (`Bash`), first released in *1989*, is commonly used as the default shell for Linux user accounts.

>The Bash shell was developed by the GNU project as a replacement for the standard Unix operating system shell, called the **Bourne shell** (`sh`)

### Dash

The **Debian Almquist shell** (`Dash`) was originally released in *2002*. This smaller shell **does not allow** command-line editing or command history, but it does provide **faster shell script** execution.

### KornShell

The **KornShell** was initially released in *1983*. It was invented by **David Korn**.

It is a programming shell compatible with the Bourne shell but supports **advanced programming features**, such as those available in the C programming languages.

### tcsh

Originally released in *1981*, the **TENEX C shell** is an upgraded version of the `C Shell`.

### Z shell

The **Z shell** (`zsh`) was first released in *1990*.
> This advanced shell incorporates features from `Bash`, `tcsh`, and `KornShell`.

### `/bin/sh`

Originally, `/bin/sh` was the location of the system’s shell.

On Linux systems, the `/bin/sh` file is now a **symbolic link** to a shell.

Showing to which shell `/bin/sh` points:

```bash
# readlink - print resolved symbolic links or canonical file names
$ readlink /bin/sh
```

To quickly determine what shell you are using at the command line, you can employ the **environment variable** `SHELL` along with the `echo` command.

> The `$` is added prior to the variable’s name in order to tap into the data stored within that variable.  
> Get current version of the Bash shell via the `BASH_VERSION`.

```bash
# echo - display a line of text
$ echo $SHELL
$ echo $BASH_VERSION
```

Learn information about your system’s Linux kernel by `uname` utility.

```bash
# uname - print system information
$ uname # displays only the kernel’s name
$ uname -r # displays the current kernel version (called the revision)
$ uname -a # displays all system information this utility provides
  ```

---

## Using a Shell

### Quoting Metacharacters

Within the Bash shell are several characters that have special meanings and functions.

These characters are called **metacharacters**. Bash shell **metacharacters** include the following:

```bash
* ? [ ] ' " \ $ ; & ( ) | ^ < >
```

**Shell quoting** allows you to use **metacharacters** as regular characters.

- To shell quote a single character, use the backslash (`\`).
- For several metacharacters, consider surrounding them with either **single** or **double quotation** marks.

### Navigating the Directory Structure

Files on a Linux system are stored within a single directory structure, called a **virtual directory**.

The **virtual directory** contains files from all the computer’s storage devices and merges them into a single directory structure.

This structure has a single base directory called the **root directory**, often simply called `root`.

You can navigate through this virtual directory structure via the `cd` command.

A helpful partner to `cd` is the `pwd` command.

```bash
# cd - change the working directory
$ cd /root
# pwd - print name of current/working directory
$ pwd
/root
```

- Press Ctrl+A or Ctrl+E to move the cursor to the start or end of the line

### Understanding Internal and External Comamands

Within a shell, some commands that you type at the command line are part of (**internal to**) the **shell program**. These internal commands are sometimes called **built-in commands**.

Other commands are **external programs**, because they **are not** part of the shell.

You can tell whether a command is an internal or external program via the `type` command.

```bash
# type -write a description of command type
$ type echo
echo is a shell builtin

$ type uname
uname is /usr/bin/uname
```

---

## Using Environment Variables

**Environment variables** track specific system information.

You can display a complete list of active environment variables available in your shell by using the `set` command.

```bash
$ set
# [...] All env var.
```

When you enter a program name (command) at the shell prompt, the shell will search all the directories listed in the `PATH` environment variable for that program.

The `which` utility searches through the `PATH` directories to find the program.

```bash
# which - locate a command
$ which echo
/usr/bin/echo
```

To change the environment variable, you simply enter the variable’s name, followed by an equal sign (`=`), and then type the new value.

```bash
# modify env var
$ $PATH="/usr/sbin/"
```

you can simply reverse any modifications you make to the variable by using the `unset` command.

Using `export` to preserve an environment variable’s definition.

```bash
# export - set the export attributes for variable s
$ export ENV=some_value
```

---

## Editing Text Files

Checkout `Cheat-sheets` repo for overview of vim commands.

---

## Processing Text Using Filters

### File-Combining Commands

To view a small text file, use the `cat` command

```bash
# cat - concatenate files and print on the standard output
cat -E  #   or --show-ends --> Display line ends 
cat -n  #   or --number --> Number lines 
cat -s  #   or --squeeze-blank --> Minimize blank lines 
cat -T  #   or --show-tabs --> Display special characters
```

If you want to display two files side-by-side use the `paste` command.

```bash
# paste - merge lines of files
$ paste random.txt numbers.txt
```

```bash
# join - join lines of two files on a common field
$ join -1 2 -2 3 #    compares first file 2nd field with second file 3rd field
```

#### expand and unexpand

> convert tabs to spaces and vice versa with unexpand

```bash
expand -t nums #    or --tabs=nums  --> tab spacing value
```

### File-Transforming Commands

#### Uncovering with `od`

This command shows octal ,hex, dec and other format of a file

```bash
# od - dump files in octal and other formats
$ od file
```

#### Separating with `split`

This utility allows you to divide a **large file** into **smaller chunks**.
> which is handy when you want to quickly create a smaller text fi le for testing pur- poses.

```bash
# split - split a file into pieces
$ split file.txt
$ split -b size   #   or --bytes=size --> Split by bytes 
$ split -l lines  #   or --lines=lines --> Split by number of lines 
$ split -C=size   #   or --line-bytes=size --> Split by bytes in line-sized chunks 
```

### File-Formatting Commands

#### Organizing with `sort`

The `sort` utility sorts a file’s data.

```bash
# sort - sort lines of text files
$ sort -i         #   or --ignore-case --> Ignore case 
$ sort -M         #   or --month-sort --> Month sort 
$ sort -n         #   Numeric sort 
$ sort -r         #   or --reverse --> Reverse sort order 
$ sort -k field   #   or --key=field --> Sort field 
```

#### Numbering with `nl`

The `nl` command allows you to number lines in a text file in powerful ways.

### File-Viewing Commands

#### Using `more`or `less`

A **pager utility** allows you to view one text page at a time and move through the text at your own pace.

The two most commonly used pagers are the `more` and `less` utilities.

#### Looking at files with `head`

The `head` command displays the first 10 lines of a text file

#### Viewing Files with `tail`

The `tail` command will show a file’s last 10 text lines.

One of the most useful `tail` utility features is its ability to **watch log** files.
Use the `-f` switch on the `tail` command and provide the log filename to watch as the command’s argument.
> You will see a few recent log file entries immediately.

### File-Summarizing

#### Counting with `wc`

The `wc` utility will display the file’s number of lines, words, and bytes in that order.
> output format  [lines] [words] [bytes/chars]

```bash
wc -l   # ==> lines
wc -w   # ==> words
wc -c   # ==> chars
```

#### Pulling Out Portions with `cut`

```bash
cut byte -b list    # or --bytes=list (*same as -c*)
cut field -f list   # or --fields=list , Note: must be used by -d
cut -d char         # or --delim=char  for specify delimiter, don't forget about -f
```

### Using Streams, Redirection, and Pipes

#### Redirecting Input and Output

- **&>** or **2>&1** Creates a new file containing both standard output and standard error.  If the specified file exists, it’s overwritten.
- **<>** Causes the specified file to be used for both standard input and standard output

### Regex

- *Bracket expressions*: choose one of elements in brackets.
    > (for instance: `b[aeiou]g` ==> bag, beg, big, bog, bug)
- *Range expressions*: range expressions list the start and end points separated by a dash.
    > (for instance: `a[f-u]b` ==> afb, agb, ...)
- *Any single character*: The dot (`.`) represents any single character except for a *newline*.
    > (for instance: b.g means any 3 letter word).
- *Start and end of line*: The carat *(`^`)* represents the start of a line and the dollar sign *(`$`)* denotes the end of a line.
    > (for instance: `^include` means it must start with include)
- *Repetition operators*: *(\*)* denotes zero or more occurrences, a plus sign *(`+`)* matches one or more occurrences, and a question mark *(`?`)* specifies zero or one match
    > (for instance: a?b.* means a(some char)b.(any extension))
- *Multiple possible string*: The vertical bar (`|`) separates two possible matches.
    > (for instance: car|truck matches either car or truck).
- *Parentheses*: Ordinary parentheses `()` surround subexpressions.
- *Escaping*: If you want to match one of the special characters, such as a dot precede it with a backslash (`\`).
    > (for instance: to match a computer hostname (say, twain.example.com), you must escape the dots, as in twain\.example\.com.)

#### grep

- *Count matching lines*: displays the number of lines that match the specified pattern if you use the `-c or --count option`.
- *Specify a pattern input file*: The `-f file or --file=file` option takes pattern input from the specified file rather than from the command line.
- *Ignore case*: You can perform a case-insensitive search, rather than the default case-sensitive search, by using the `-i or --ignore-case` option.
- *Search recursively*: The `-r or --recursive` option searches in the specified directory and all subdirectories rather than simply the specified directory.
- *Use an extended regular expression*: To use an extended regular expression, you can pass the `-E or --extended` regexp option.

> Favorite combination:

```bash
grep -irn "pattern" files
```

#### sed

```bash
sed [OPTION]... {script-only-if-no-other-script} [input-file]...
```

> for instance:

```bash
sed "s/old/new" file    # replaces old with new
```

### Managing Processes

#### ps [options]

> CAUTION: watchout switches with dash (-).In other words, *-x* and *x* are **not same**.

- `-A and -e` ==> display all the processes on the system.
- `x` ==> displays all processes owned by the user. Also increases the amount of information that’s displayed.
- `(-u | -U | --User)` User ==> Display one user’s processes. The user variable may be a *username* or a *user ID*.
- *Display extra information*: The `-f, -l, j, l, u, and v` options all expand the information provided in the ps output.
- *Display process hierarchy*: The `-H, -f, and --forest` options group processes and use indentation to show the hierarchy of relationships between processes.
- `-w and w` options tell ps that output *must not* exceeded 80 chars per line. (useful for saving ps output to a file)

> Favorite combination:

-`ps aux` and `ps fax`.

##### Interpreting ps Output

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

#### top

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

#### Jobs, Foreground and Background Processes

- `jobs`: shows  all jobs in this terminal.
- Ctrl+Z normally pauses the program and gives you control of the terminal
- `fg [num]`: restore job [num to foreground]
- An alternative to launching a program, using Ctrl+Z, and typing bg to run a program in the background is to append an ampersand `&` to the command when launching the program.

#### Managing Process Priorities

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

#### Killing Processes

```bash
kill -l               # Shows all signals
kill -s signal pid    # Sends a signal to the process (specified by pid) 
```

`killall [options] [--] name [...]`

```bash
killall vi            # kills all progrmas named vi
```

## END
