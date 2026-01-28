# Block 1: File System and Navigation

## Exercise 1: System Information

1. Use `hostname` to view our current system hostname.
2. View USB and PCI devices with `lsusb` and `lspci`.
3. View Distro and Kernal version with `uname -a` and `lsb_release -a`.
4. Get our current user with `whoami`.
5. Output all your previously entered commands with `history`

## Exercise 2: Exploring the File System

1. Use `cd /` to navigate to our root directory and view files with `ls`
2. Use `cd ~` or `cd` to navigate to your home directory.
3. Type `pwd` to see where you are currently located.
4. Create a new directory called `myfiles` using `mkdir myfiles`.
5. Navigate into the `myfiles` directory using `cd myfiles`.
6. Create a new file called `example.txt` using `touch example.txt`.
7. Use `ls` to view all files in your current directory then run `ls -a` to view the hidden files.
8. Navigate back to our home directory using an absolute path with `cd /home/<USER>`.
9. Explore the file system freely to get familiar with navigation.

## Exercise 3: Creating and Modifying Files

1. Create a new directory called `documents` and navigate into it.
2. Create a new file called `report.txt`.
3. Edit and add text to that file with `nano report.txt`
4. Add text to the file and save with CTRL+O and exit with CTRL+X
5. View the contents of the file with `cat report.txt`

## Exercise 4: Copying and Moving Files

1. Verify you are in your `documents` folder inside your home directory with `pwd`.
2. Use `cp report.txt backup.txt` to create a copy of the `report.txt` file.
3. Move the `backup.txt` file into a new folder called `backup` in your home directory.
4. Verify that the file was moved correctly by checking the contents of your home directory.
5. Delete the original `report.txt` file from earlier using `rm`.
6. Use `rm -r` to delete the entire `documents` directory and all its contents.
7. Create a new file in your home directory called `notes.txt` containing all the commands we have used so far.

# Block 2: User Management and Permissions

## Exercise 1: User Management

1. Create a new user called `labuser` using `sudo adduser labuser`.
2. Set the password for the new user using `sudo passwd labuser`.
3. Verify that the new user exists by checking the contents of `/etc/passwd`.
4. Log into `labuser` with `su labuser` and change to their home directory.
5. Log out of the `labuser` with `exit`

## Exercise 2: Permissions

1. Check permissions in the current directory with `ls -l`.
2. Create a new file called `private.txt` and `public.txt`.
3. Use `chmod` to change permissions for each file.
	1. `chmod 600 private.txt` to give read and write permissions to only the owner.
	2. `chmod +r public.txt` to give read permissions to everyone.
5. Create another file called `root.txt`
6. Use `chown` to change the user and group permissions to the root user and group with `chown root:root root.txt`
7. Check the permissions to verify changes
8. Change back to the previous user with either `su` or `exit` to log out of the current user
9. Try to access and modify files again under the new user

# Block 3: Modifying files and redirection

## Exercise 1: Streams and Redirections

1. Use `echo "Hello World"` to output `Hello World to stdout.
2. Create a new `output.txt` file and redirect the output of `echo` to that new file with `echo "Hello World" > output.txt`.
3. Redirect the output of the `history` command to a file called `history.txt`

## Exercise 2: Search and Wildcards

1. View contents of all files in a directory with `cat *`
2. View contents of all files ending with .txt with `cat *.txt`
3. Find all past uses of the `cat` command in our history by piping the stdout of history to the stdin of grep with a pipe `history | grep "cat"`


# Block 4: Services and Package Management

## Exercise 1: Installing Packages

1. Use `sudo apt update` to refresh the package list.
2. Use `apt search htop` to view packages related to `htop`.
3. Install a new package called `htop` using `sudo apt install htop`.
4. Verify that the package was installed correctly by running `htop`.
5. Install the `apache2` package with `apt`.
## Exercise 2: Background Services
1. View the status of the `apache2` service with `systemctl status apache2`.
2. Configure `apache2` to start on boot with `systemctl enable apache2`.
3. Stop the `apache2` service using `systemctl stop apache2`.
4. Start the `apache2` service again.

## Exercise 3: Working with Processes

1. Use `ps` to view a list of running processes.
3. Use `ps -ef` to view a more detailed list of running processes.
4. Use `top` to interactively view and manage processes.
5. Press `q` to quit back to the shell
6. Send a forceful SIGKILL signal to all zsh processes (including our current one) with `killall -9 zsh`
7. View which processes are using the `/bin/zsh` file with `lsof /bin/zsh`
8. Kill the process with `kill <PID>` and replace `PID` with the output from the previous command
