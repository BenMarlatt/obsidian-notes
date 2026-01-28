Common commands, options, arguments, and uses for different tools in Linux

## Contents
1. [Layout](#layout)
2. [System Information](#system-information)
3. [Version Information](#version-information)
4. [Directories and Files](#directories-and-files)
5. [File Archives](#file-archives)
6. [Permissions](#permissions)
7. [User Info](#user-info)
8. [Hardware](#hardware)
9. [Disks](#disks)
10. [Tools](#tools)
11. [Shells](#shells)
12. [Process Management](#process-management)
13. [Package Management](#package-management)
14. [Network](#network)

## Document layout and how to use

Layout of how commands are displayed. The title below will be displayed in the command's case sensitive format, exactly how it will be used in the command line. These command blocks will contain the command, description of the command, syntax, options, arguments, and examples.
### command

This is an example command. This description will display the purpose of the command. 

**SYNTAX:**

```bash
command [OPTIONAL ARGUMENT] <REQUIRED ARGUMENT>
```

OPTIONS:

    -h
        Description of an argument

EXAMPLES:

```bash
# Example of how to run our command
command -h required.txt

# Example without optional argument
command required.txt
```


## System Information

    hostname - Displays the system's hostname
    date - Shows the current date and time
    uname -a
    lsb_release -a
    cat /etc/lsb-release
    cat /etc/os-release
    cat /proc/version

## Directories and Files

### ls

List the directory structure

SYNTAX:

```bash
ls [OPTION] [DIRECTORY]
```

OPTIONS:

    -a
        List all files including hidden
    -l
        Format output in a vertical list and display extra info
    -h
        Make the output file sizes human-readable

EXAMPLES:

```bash
# Display the contents of the current directory, including all hidden files, in a human-readable list
ls -alh

# Display files in the /etc/systemd/ directory
ls /etc/systemd/
```

### pwd

Print the absolute path to the current working directory

SYNTAX:
```bash
pwd
```

### cd

Change Directory. Primarily used to navigate the linux file system. You can find your current directory with the [[#pwd]] Command

SYNTAX:
```bash
cd [DIRECTORY]
```

> [!INFO]
> SPECIAL DIRECTORIES:
> 
> `.` Current Directory
> `..` Parent Directory 
> `~` Current user's home directory
> `~<USER>` Specified user's home directory
> `\` Root directory
>`-`  Previous directory accessed by cd

EXAMPLES:

```bash
# Without specifying optional directory it will change to current user's home directory. Same as `cd ~`
cd

# Change to the current user's home directory with an absolute path.
cd /home/$USER/

# Change to the top most root directory.
cd /
```

### touch

SYNTAX:
```bash
touch [FILE]
```

EXAMPLES:

```bash
# Create a file named `notes.txt` in the current working directory
touch notes.txt

# Create a file named 'notes.txt' in the current user's home directory
touch /home/$USER/notes.txt
```

### rm

SYNTAX:
```bash
rm [OPTIONS] <FILE>
```

OPTIONS:

`-r`
	Recursively remove files.
`-f`
	Forcefully remove files without prompting the user for confirmation.

EXAMPLES:

```bash
# Remove file named `notes.txt`
rm notes.txt

# Remove the directory named `myfiles` and all files within it
rm -r myfiles
```


## File Archives

### tar

Combine multiple files or directories together into a single tar 'archive'. Common on Linux for compressing and sharing files over a network.

**SYNTAX:**

```bash
tar [OPTIONS]
```

`-c`
	Create an archive
`-x`
	Extract files from archive
`-f`
	Specify archive file
`-v`
	Increase the output
`-z`
	Filter the archive through gzip

**EXAMPLES:**

```bash
# Extract a tar archive
tar -xf archive.tar

# Extract a tar archive through gzip
tar -xzf archive.tar.gz


# Compress a folder to an archive
tar -vzf archive.tar directory
```

### 7z

Combine multiple files or directories together into a single 7z 'archive'. More universal than `tar` when sharing archives to other operating systems.

**SYNTAX:**

```bash
7z [OPTIONS]
```


`a <ARCHIVE> [INPUT FILE/DIR]`
	Compress a selected file/directory to an archive.
`e <ARCHIVE>
	Extract a selected archive to the current directory.
`l <ARCHIVE>`
	List the contents of the selected archive.

**EXAMPLES:**

```bash
# Extract a 7z archive into a dictory with the same filename without the 7z extension
7z e archive.7z

# Compress a directory to an archive
7z a archive.7z directory

# Archive all files ending in .txt to archive.7z
7z a archive.7z *.txt
```

## Permissions
### chmod

Change file permissions

SYNTAX:

```bash
chmod [OPTION] <FILE>
```

OPTIONS:

    -R
	    Recursively change permissions

EXAMPLES:

``` bash
# Give read and write permissions to the owner and everyone just read
chmod 644 notes.txt 

# Recursively add execute permissions to all files inside the scripts directory
chmod -R a+x scripts/ 
```

### chown

Change file owner and group

SYNTAX:

``` bash
chown [OPTIONS] <USER>:<GROUP> <FILE>
```

OPTIONS:

    -R
        Recursively change ownership

EXAMPLES:

``` bash

# Transfer ownership of notes.txt to the root user and group
chown root:root notes.txt 
```

## User Management

### whoami

Displays the current user.

**SYNTAX:**

```bash
whoami
```

### who

Displays information about users currently logged in.

**SYNTAX:**

```bash
who
```

### w

Shows who is logged in and what they are doing.

**SYNTAX:**

```bash
w [USER]
```

**EXAMPLES:**

```bash
# Provides a detailed list of all users and their current activity.
w

# List what the root user is doing
w root	 
```

### id

Shows user and group IDs.

**SYNTAX:**

```bash
id
```

**EXAMPLES:**

```bash
# Displays the user ID (UID) and group ID (GID) of the current user.
id
```

### su

Switch user.

**SYNTAX:**

```bash
su [USER]
```

**EXAMPLES:**

```bash
# Switches to the root user (prompt for root's password).
su root
```

### sudo

Execute commands as another user.

**SYNTAX:**

```bash
sudo [COMMAND]
```

**EXAMPLES:**

```bash
# Executes the `ls` command in the `/root` directory with superuser privileges.
sudo ls /root
```

### useradd

Add a new user.

**SYNTAX:**

```bash
useradd <USER>
```

**EXAMPLES:**

```bash
# Creates a new user named `newuser1`.
useradd newuser1
```
### usermod

Modify a user account.

**OPTIONS:**

`-a`
	Add user to the supplied groups
`-G`
	List of groups

**SYNTAX:**

```bash
usermod [OPTIONS] <USER>
```

**EXAMPLES:**

```bash
# Adds `newuser` to the `sudo` group.
usermod -aG sudo newuser
```

### userdel

Delete a user.

**SYNTAX:**

```bash
userdel <USER>
```

**EXAMPLES:**

```
# Deletes the user `newuser1`.
userdel newuser1
```

### passwd

Change user password.

**SYNTAX:**

```bash
passwd [USER]
```

**EXAMPLES:**

```
# Changes the password for the user `newuser1`
passwd newuser1
```

### groupadd

Add a new group.

**SYNTAX:**

```bash
groupadd [GROUPNAME]
```

**EXAMPLES:**

```bash
# Creates a new group named `developers`
groupadd developers
```

### groups

Show user groups.

**SYNTAX:**

```bash
groups [USER]
```

**EXAMPLES:**

```bash
# Displays the groups that the current user belongs to.
groups

# Displays the groups that `newuser1` belongs to.
groups newuser1
```

---

## Hardware

### Special Files

Special files that contain details about the hardware

**FILES:**
	`/proc/cpuinfo`
	`/proc/meminfo`

**EXAMPLES:**

```bash
# Shows details about the CPU architecture and cores.
cat /proc/cpuinfo

# Provides information about system memory usage.
cat /proc/meminfo
```

### dmidecode

Display hardware information.

**SYNTAX:**

```bash
dmidecode
```

### lsusb

List USB devices.

**SYNTAX:**

```bash
lsusb
```

### lspci

List PCI devices.

**SYNTAX:**

```bash
lspci
```

---

## Disks

### mount

Mount a filesystem.

**SYNTAX:**

```bash
mount [OPTIONS] <DEVICE> <MOUNTPOINT>
```

**EXAMPLES:**

```bash
# Mounts the `/dev/sda1` partition to the `/mnt` directory.
mount /dev/sda1 /mnt
```

### umount

Unmount a filesystem.

**SYNTAX:**

```bash
umount [OPTIONS] <MOUNTPOINT>
```

**EXAMPLES:**

```bash
# Unmounts the filesystem mounted at `/mnt`.
umount /mnt
```

### df

Display disk space usage.

**SYNTAX:**

```bash
df [OPTIONS]
```

**OPTIONS**:
	`-h`
		Display storage space in human readable units.


**EXAMPLES:**
`
```
# Shows disk space usage in human-readable format.
df -h
```

### free

Display memory usage.

**SYNTAX:**

```bash
free [OPTIONS]
```

**OPTIONS:**

`-b`
	Display the amount of memory in bytes.
`-k`
	Display the amount of memory in kibibytes.  This is the default.
`-m`
	Display the amount of memory in mebibytes.
`-g`
    Display the amount of memory in gibibytes.
`-h`
	Make the output human readable

**EXAMPLES:**

```bash
# Displays memory usage in megabytes.
free -m
```

### du

Estimate file space usage.

**SYNTAX:**

```bash
du [OPTIONS] [FILE]
```

### cfdisk

Partition table manipulator.

**SYNTAX:**

```bash
cfdisk
```

---

## Tools

### grep

Search for text in files.

**SYNTAX:**

```bash
grep [OPTIONS] <STRING> <FILE>
```

**OPTIONS:**

- `-i`
  - Case insensitive.
- `-r`
  - Recursive.

**EXAMPLES:**

```bash
# Searches for the term "error" in `logfile.txt`, ignoring case.
grep -i "error" logfile.txt
```

### find

Search for files and directories.

**SYNTAX:**

```bash
find <PATH> <TEST> <PATTERN>
```

**TESTS**:
`-name`
	Search by filenames
`-group`
	Search by group permissions
`-user`
	Search by user owner
`-path`
	Search for specific paths within specified PATH

**EXAMPLES:**

```bash
# Finds all `.txt` files under the `/home` directory.
find /home -name "*.txt"

# Finds all `.txt` files under the `/home` directory and export errors to /dev/null.
find /home -name "*.txt" 2> /dev/null
```

### which

Locate the path to the binary (executable) of a command.

**SYNTAX:**

```bash
which [OPTIONS] <FILE>
```

**OPTIONS:**

`-a`
	Print all matching pathnames.

**EXAMPLES:**

```bash
# Shows the path to the `bash` executable.
which bash
```

### file

Attempts to determine file type.

**SYNTAX:**

```bash
file <FILENAME>
```

### nano

Command line text editor

**SYNTAX**:
```bash
nano [FILENAME]
```


---

## Shells
### sh

Bourne Shell.

**SYNTAX:**

```bash
sh [COMMAND]
```

### bash

Bourne Again Shell.

**SYNTAX:**

```bash
bash [COMMAND]
```

### zsh

Z Shell.

**SYNTAX:**

```bash
zsh [COMMAND]
```

### history

Display command history. History is saved to a local hidden file usually located in the home directory and will have a different name depending on the shell being used.

**SYNTAX:**

```bash
history [OPTION]
```

**OPTIONS**:
`-c`
	Clears bash history

### Redirections `<` and `>`

Redirect input and output to and from files.

**SYNTAX:**

```bash
command > file # Send output of the command to file
command >> file # Append output of the cmmmand to file
command1 < command2 # send output of command2 to the input of command1 
```

**EXAMPLES:**

```bash
# Redirects the output of `ls` to `output.txt`.
ls > output.txt

# Reads from `input.txt` and sends it to the `cat` command.
cat < input.txt
```

### IO Streams `0 1 2`

Standard input, output, and error.

**SYNTAX:**

```bash
command 1> output.txt 2> error.txt
```

**EXAMPLES:**

```bash
# Redirects standard output to `output.txt` and standard error to `error.txt`.
command 1> output.txt 2> error.txt
```
### Pipes `|`

Send output of one command to another.

**SYNTAX:**

```bash
command1 | command2
```

**EXAMPLES:**

```bash
# Filters the output of `ls` through `grep` to find matches.
ls | grep "pattern"
```

### Command separators `;` and `&&`

The `;` (semi-colon) is used to split multiple commands on a single line that will be ran sequentially in order. The `&&` does the same but will only continue if the previous command succeeded.

**SYNTAX:**

```bash
command1; command2
command1 && command2
```

**EXAMPLES:**

```bash
# Executes `echo "Hello"` and `echo "World"` sequentially.
echo "Hello"; echo "World"

# Creates a directory and, if successful, run cd to change into it.
mkdir newdir && cd newdir
```

### Command substitution `` ` `` and `$()`

Use the output of one command as input to another.

**SYNTAX:**

```bash
result=$(command)
result=`command`
```

**EXAMPLES:**

```bash
Assigns the current date to the variable CURRENT_DATE.
CURRENT_DATE=$(date)
```

---

## Process Management

### Linux Signals
Signals can be sent to running processes to interrupt its execution. Some signals can be sent via keyboard to a processes running in the foreground.
- X - `SIGINT`: Gracefully terminate the running application. (CTRL+C)
- X - `SIGSTOP`: Pause a process to be resumed later. (CTRL+Z)
- X - `SIGCONT`: Resume the processed paused earlier.
- 9 - `SIGKILL`: Forcefully terminate a process immediately.

### ps

Display information about the active processes.

**SYNTAX:**

```bash
ps [OPTIONS]
```

**EXAMPLES:**

```bash
# Shows detailed information about all running processes.
ps aux

# Display proccesses running under the current user.
ps -u $USER
```

### lsof

List open files or directories being used by processes.

**SYNTAX:**

```bash
lsof [OPTIONS]
```

**EXAMPLES:**

```bash
# Lists all open network connections.
lsof -i
```

### kill

Terminate processes.

**SYNTAX:**

```bash
kill [OPTIONS] <PID>
```

**EXAMPLES:**

```bash
# Sends the default signal (TERM) to terminate the process with PID 1234.
kill 1234

# Sends the SIGKILL signal to forfully terminate the process with PID 1234.
kill -9 1234

# Sends the SIGCONT signal to resume execution of the process with PID 1234 if it was stopped with SIGSTOP.
kill -SIGCONT 1234
```

### killall

Kill processes by name.

**SYNTAX:**

```bash
killall [OPTIONS] <NAME>
```

**EXAMPLES:**

```bash
# Terminates all processes with the name `firefox`.
killall firefox

# Sends the SIGKILL signal to forfully terminate the process with the name `firefox`.
killall -9 firefox
```

### wait

Wait for process completion.

**SYNTAX:**

```bash
wait [PID]
```

### sleep

Wait for a specified amount of time before execution continues.

**SYNTAX**:
```bash
sleep <SECONDS>
```

### jobs

List jobs.

**SYNTAX:**

```bash
jobs
```

### fg

Bring job to foreground.

**SYNTAX:**

```bash
fg [JOB_ID]
```

### bg

Send job to background.

**SYNTAX:**

```bash
bg [JOB_ID]
```

### crontab

Schedule recurring tasks.

**SYNTAX:**

```bash
crontab [OPTIONS]
```

**OPTIONS**:
	`-e`
		Edits the current user's cron jobs.
	`-l`
		Lists current user's cron jobs
	`-r`
		Deletes current user's cron jobs
	`-u`
		Specify which user's cron jobs to manage 

**EXAMPLES:**

```bash
# Edits the root user's cron jobs.
sudo crontab -e

# Lists all cron jobs for the `www` user
crontab -u www -l
```

### systemctl

Control the systemd system and service manager.

**SYNTAX:**

```bash
systemctl <COMMAND> <SERVICE>
```

**COMMANDS**
	`start`
		Bring service to an active state and run in the background
	`stop`
		Gracefully deactivates the service and stops running in the background
	`restart
		Stop then start the service
	`status`
		Displays the current state of the service and output logs
	`enable`
		Configures the service to start on boot
	`disable`
		Configures the service to not start on boot
	`mask`
	`unmask`

---

## Package Management

Tools for managing installed packaged on linux systems. These tools will vary based on the linux distro being used.

* Ubuntu
	* dpkg
	* apt
	* snap
* Debian/Kali/Mint/PopOS!
	* dpkg
	* apt
* Fedora/CentOS/RHEL
	* yum
	* dnf
	* rpm
* Arch/Monjaro
	* pacman

### dpkg

Debian package manager.

**SYNTAX:**

```bash
dpkg [OPTIONS] [PACKAGE]
```

**EXAMPLES:**

```bash
# Lists all installed packages.
dpkg -l
```

### apt

Advanced Package Tool.

**SYNTAX:**

```bash
apt [OPTIONS] [COMMAND]
```

**OPTIONS**:
	`-y`
		Do not prompt asking to install and default any questions to `yes`
		

**COMMANDS**
	`update`
		Pull latest available packages from external repos
	`upgrade [PACKAGE]`
		Upgrade installed packages or specific package if specified
	`full-upgrade`
		Upgrade the entire linux distribution (More likely for breaking changes)
	`install <PACKAGE>`
		Installs a package 
	`remove <PACKAGE>`
		Remove specified package with all binaries and configurations tied to it. May leave some user modified configurations.
	`autoremove`
		Remove packages that were automatically installed to satisfy dependencies for other packages and are now no longer needed.
	`search <TEXT>`
		Search for packages in our repos
	`show <PACKAGE>`
		Display information about a package

**EXAMPLES:**

```bash
# Updates the package index.
sudo apt update

# Automatically update all packages
sudo apt update -y && sudo apt upgrade -y

# Install a packaged named `nginx` without prompting the user
sudo apt install -y nginx
```

### snap

Package management system for snaps.

**SYNTAX:**

```bash
snap [OPTIONS] [COMMAND]
```

**EXAMPLES:**

```
# Installs the `vlc` snap package.
snap install vlc
```

---

## Network

### ifconfig

Configure network interfaces.

**SYNTAX:**

```bash
ifconfig [INTERFACE]
```

**EXAMPLES:**

```bash
# Displays configuration details for the `eth0` network interface.
ifconfig eth0
```

### iwconfig

Configure wireless network interfaces.

**SYNTAX:**

```bash
iwconfig [INTERFACE]
```

**EXAMPLES:**

```bash
# Displays wireless configuration for `wlan0`.
iwconfig wlan0
```

### ip

Show/manipulate routing, devices, policy routing, and tunnels.

**SYNTAX:**

```bash
ip [COMMAND]
```

**EXAMPLES:**

```bash
# Displays IP address information for all interfaces.
ip address
```

### iw

Show/manipulate wireless devices.

**SYNTAX:**

```bash
iw [COMMAND]
```

**EXAMPLES:**

```bash
# Lists wireless devices and their details.
iw dev
```

### nmcli

Command-line tool for NetworkManager.

**SYNTAX:**

```bash
nmcli [OPTIONS] [COMMAND]
```

**EXAMPLES:**

```
# Lists network devices and their status.
nmcli d
```

### netstat

Network statistics.

**SYNTAX:**

```bash
netstat [OPTIONS]
```

**EXAMPLES:**

```bash
# Displays all listening ports and their services.
netstat -tuln
```

### ss

Utility to investigate sockets.

**SYNTAX:**

```bash
ss [OPTIONS]
```

**EXAMPLES:**

```bash
# Shows detailed information about listening TCP and UDP sockets.
ss -tuln
```

### ufw

Uncomplicated Firewall.

**SYNTAX:**

```bash
ufw [COMMAND] [OPTIONS]
```

**EXAMPLES:**

```bash
# Enables the UFW firewall.
ufw enable
```

### iptables

Administration tool for IPv4 packet filtering and NAT. (The complicated alternative to UFW)

**SYNTAX:**

```bash
iptables [OPTIONS]
```

**EXAMPLES:**

```bash
# Lists all current iptables rules.
iptables -L
```



