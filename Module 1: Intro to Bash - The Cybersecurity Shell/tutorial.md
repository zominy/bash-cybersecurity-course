# Module 1: Intro to BASH - The Cybersecurity Shell 🐚📚
---

## Introduction

In this tutorial, I cover the basics of Bash scripting and command line usage from the module 1 video. You can use this to read up on the material or go through the labs in your own time.

I will also be using a lot of information from my commands.md file for this module.

### 🎯 Aims
### By the end of this module, you should be able to:

Use simple commands such as ls, pwd, echo, nano, and cd with knowledge of how they work.

To be able to string commands together.

To have knowledge of shortcuts and what they do.

### Note: Please check out the [command file](commands.md) for help if you are struggling to understand any concepts.

---

## Setup ⚙️🔧

The setup for this lab is minimal. Download a virtual machine software. For this course, I used VMware; I just prefer it... but whichever software you use does not really matter.

If you are unable to get VMware, I recommend Virtual Box instead. Both are great virtualisation software options:

### Installation Resources For Virtualisation Software

- [VirtualBox Official Site](https://www.virtualbox.org/wiki/Downloads)
- [VMware Official Site](https://www.vmware.com/products/workstation-player.html)

In addition to this, here are guides on how to install the OS that I will be using for this course (Debian):

### Public Guides For Installing Debian

- [Installing Debian on VMware](https://www.youtube.com/watch?v=eJHtr1QlDeo)
- [Installing Debian on Virtual Box](https://www.youtube.com/watch?v=GkIb-l1K2FQ)

With this, we are ready to go.

---

## Module 1 Lab 🧪📘

The video first begins with a small introduction to the Debian GUI. Then we go into activities > 9 dots > terminal.

This is the Command-Line Interface (CLI), where all the course work will take place.

The first command entered is:

```bash
pwd
```
Explanation: 'pwd' stands for 'print working directory' which displays the full absolute path of your current directory. It is pretty useful to know where you are in the filesystem, especially when the file structure is complicated.

In the video it looks like this:
```bash
max@maxz:~$ pwd
/home/max
max@maxz:~$ 
```
As can be seen, the pwd command is entered while we are inside the home directory. Thus, it returns /home/max; this is where we are.

---
### Part 2 📁2️⃣📍

Next, we then enter the 'ls' command into the terminal as such:

```bash
max@maxz:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
max@maxz:~$ 
```

Explanation: 'ls' stands for 'list' and when entered, it displays all the files and directories in the current directory, or a selected one.

We can see that once it is entered, it displays the contents of the current directory that we are in. That being subdirectories that are there with a new installation of Debian.

After that, we enter 'ls -l' which provides a long listing which provides the same info but also:

-File permissions

-Number of links

-Owner

-Group

-File size

-Modification date/time

-File name

Here is an example from the video:

```bash
max@maxz:~$ ls -l
total 32
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Desktop
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Documents
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Downloads
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Music
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Pictures
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Public
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Templates
drwxr-xr-x 2 max max 4096 Jun  8 19:51 Videos
max@maxz:~$ 
```
Here is a breakdown of what the random numbers, letters, and names actually mean:

`d` — indicates it's a directory

`rwx` — the owner has read, write, and execute permissions

`r-x` — the group can read and execute, but not write

`r-x` — others can read and execute

`2` — number of hard links (alternate names for the same file)

`max` — owner

`max` — group

`4096` — file size in bytes (directories often use 4096 bytes)

`Jun 8 19:51` — last modification time

`Desktop` — file/directory name

---
### Part 3 📂3️⃣➡️

We are next introduced to the 'cd' command as we attempt to change into the '/etc' directory - the /etc directory stores system configuration files. It contains settings and config files for the system and installed programs.

Here is the example from the video:

```bash
max@maxz:~$ cd /etc
max@maxz:/etc$ 
```
Explanation: 'cd' stands for change directory and is essential for navigating the file system in Linux.

In the example, I enter the 'cd' command followed by '/etc'. So basically 'cd target_directory'.

You can see the prompt is now:
```bash
max@maxz:/etc$
```
Instead of:
```bash
max@maxz:~$
```
Signifying that we are now inside the /etc directory. If this does not work then you will be met with an error. If you encounter errors in this lab, please check out the [file for any errors and how to fix them](errors.md).

This is then followed by another demonstration of the 'ls -l' command. It looks like this:

```bash
max@maxz:/etc$ ls -l
total 1064
-rw-r--r--  1 root root    3040 May 25  2023 adduser.conf
-rw-r--r--  1 root root      44 Jun  8 19:50 adjtime
drwxr-xr-x  3 root root    4096 Jun  8 19:43 alsa
drwxr-xr-x  2 root root    4096 Jun  8 19:47 alternatives
-rw-r--r--  1 root root     401 Jan 11  2023 anacrontab
drwxr-xr-x  4 root root    4096 Jun  8 19:46 apache2
-rw-r--r--  1 root root     433 Aug 23  2020 apg.conf
drwxr-xr-x  2 root root    4096 Jun  8 19:17 apparmor
drwxr-xr-x  8 root root    4096 Jun  8 19:47 apparmor.d
-rw-r--r--  1 root root     833 Feb 10  2023 appstream.conf
```
Note: This is not the entire listing due to the volume of files and subdirectories within the /etc directory.

As can be seen, some contents do not have a 'd' at the start meaning that they are files. Also, some do not have any x's in the permissions section, meaning that they are not executable.

You will also notice that the owner is 'root'. root is the superuser or administrator account on Linux/Unix systems. It has full control and permission to do anything on the system.

---
### Note for later modules 📝⏳

You can become root by running:

```bash
sudo -i
```
or
```bash
sudo su
```
(You’ll need to enter your password and be in the sudoers list.)

Alternatively, you can switch user to root if you know the root password:
```bash
su -
```
---
### Part 4 📂4️⃣➕✏️🗣️

Next, we return to the home directory by simply entering 'cd'. This returns us to the home directory regardless of where we are in the filesystem:

```bash
max@maxz:/etc$ cd
max@maxz:~$ 
```

Then we use the 'mkdir' command to create a new directory like so:

```bash
max@maxz:~$ mkdir lab1
max@maxz:~$ ls
Desktop  Documents  Downloads  lab1  Music  Pictures  Public  Templates  Videos
max@maxz:~$ 
```
Explanation: 'mkdir' stands for 'make directory'. It's a simple command where you can enter: mkdir 'name of directory' and it will create a new directory under your current directory.

We then enter the 'ls' command to re-list the contents of the home directory to see the new subdirectory that we have created.

After that, we then make a new file using the echo command as such:

```bash
max@maxz:~$ cd lab1
max@maxz:~/lab1$ echo "Hello World" > file1
max@maxz:~/lab1$ nano file1
max@maxz:~/lab1$ rm file1
max@maxz:~/lab1$ ls
max@maxz:~/lab1$ pwd
/home/max/lab1
max@maxz:~/lab1$ cd ..
max@maxz:~$ pwd
/home/max
max@maxz:~$ 
```
Explanation:
```bash
max@maxz:~$ cd lab1
```
Changes directory to lab1 inside your home directory.
```bash
max@maxz:~$ echo "Hello World"
```
Prints the text "Hello World" to the terminal (standard output).
```
'>' (redirect operator)
Takes the output of the command on the left (echo "Hello World") and writes it into the file on the right (file1).
```
If file1 doesn’t exist, it creates it.

If file1 exists, it overwrites it.
```bash
max@maxz:~$ nano file1
```
Opens the file file1 in the nano text editor — a simple command-line editor where you can read or modify the file content.
```bash
max@maxz:~$ rm file1
```
Removes (deletes) the file named file1 from the filesystem. After this, the file is gone unless you have backups.
```bash
max@maxz:~/lab1$ ls
```
Lists the contents of lab1 (empty now because you deleted file1).
```bash
max@maxz:~/lab1$ pwd
```
Prints the current directory path: /home/max/lab1.
```bash
max@maxz:~/lab1$ cd ..
```
Puts you up one directory - this is expanded upon in module 2.
```bash
max@maxz:~$ pwd
```
Prints the current directory path: /home/max.

---

Once back in the home directory, we can then remove the 'lab1' directory:

```bash
max@maxz:~$ rmdir lab1
max@maxz:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
max@maxz:~$ 
```

The 'rmdir' command works the same as the mkdir command in terms of structure but removes the directory.

Note: In order for rmdir to work, the directory must be empty. If the directory contains anything then you must use 'rm -rf directory_name' but be careful since this is a very powerful command.

---
### Part 5 5️⃣🔗🧵

Now that we have a grasp on basic commands, we can begin stringing them together.

Explanation: In the Linux terminal, you can combine different commands together for efficiency reasons. Here is how to do it:

Important Concepts:
&& - Logical AND - Run the next process ONLY IF the previous is successful.

|| - Logical OR - Run the second command ONLY IF the previous command fails.

; - Command Separator - This is to separate commands. Runs all commands and does not care if the previous command is successful or fails.

### Example of AND operator from video:
```bash
max@maxz:~$ cd /tmp && echo "Success"
Success
max@maxz:/tmp$ 
```
What's happening:

cd /tmp — Changes to the /tmp directory.

&& — Runs the next command only if the previous one succeeds.

echo "Success" — Prints "Success".

Since cd /tmp was successful, echo "Success" runs.

### Example of the OR operator from video:
```bash
max@maxz:~$ cd /notarealplace || echo "It worked"
bash: cd: /notarealplace: No such file or directory
It worked
max@maxz:~$ 
```
What’s happening:

cd /notarealplace tries to change into a directory that doesn’t exist. It fails.

|| means “if the previous command fails, then run the next command.”

So, echo "It worked" runs because cd failed.

Result: You see the error message from cd, then "It worked" is printed.

### Example of the Command Separator from video:
```bash
max@maxz:~$ cd /tmp; ls; pwd
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-colord.service-QjnKX5
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-geoclue.service-Xw1Xr1
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-low-memory-monitor.service-eTo839
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-ModemManager.service-MHtRcc
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-power-profiles-daemon.service-4dg9Aj
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-switcheroo-control.service-TWEwy4
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-systemd-logind.service-vuOmCa
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-systemd-timesyncd.service-cLUFt8
systemd-private-d2e1e9127f1045a8afaaab5562fedbd8-upower.service-rfqN10
tracker-extract-3-files.1000
tracker-extract-3-files.111
VMwareDnD
vmware-root_572-2999067484
/tmp
max@maxz:/tmp$ 
```
What's happening:
```
cd /tmp;
```
Changes the current directory to /tmp.
```
ls;
```
Lists all files and directories in /tmp.
```
pwd
```
Prints the current working directory — /tmp.

The ; lets you run multiple commands in sequence, regardless of whether the previous ones succeed.

So this line navigates to /tmp, shows its contents, and confirms you're in /tmp.

Combining Operators From Video:
```bash
max@maxz:~$ cd Downloads; cd /notarealplace || echo "Success Again" && cd && pwd
bash: cd: /notarealplace: No such file or directory
Success Again
/home/max
max@maxz:~$ 
```
What's happening:
```
cd Downloads;
```
Changes to the Downloads directory.
```
cd /notarealplace
```
Fails (directory doesn’t exist), so the next part runs because of ||.
```
|| echo "Success Again"
```
Runs because the previous cd failed. So it prints: Success Again.
```
&& cd && pwd
```
This runs only if echo "Success Again" succeeds — which it does.

cd — With no argument, it goes to the home directory.

pwd — Prints /home/max.

---
Part 6 6️⃣⌨️⚡

Begin by typing:
```
ping google.com
```
And enter this into the terminal. It will proceed to ping google indefinitely. To cancel this, simply press Crtl + C. This keybind cancels processes.

Next, type:
```
cd Downl
```
Then press the tab key and it will auto-complete the command you are typing.

Lastly, press Crtl + L to clear the terminal. This is useful to clear the clutter without halting any processes or deleting anything.

---

### Congratulations on Completing Module 1 🎉👏

To summarise, this lab introduces basic Linux terminal commands using the Debian GUI. It begins with opening the terminal and exploring the Command-Line Interface (CLI).

`pwd`: Prints the current directory path.

`ls` & `ls -l`: Lists files and directories. -l provides detailed information like permissions, owner, size, etc.

`cd`: Changes the current directory (e.g., cd /etc). Using cd alone returns you to the home directory.

`mkdir` & `rmdir`: Creates and removes directories. rmdir only works on empty directories.

`echo` & file redirection: echo "Hello" > file1 creates a new file. nano opens files for editing, and rm deletes them.

### Command chaining:

`&&`: Runs the next command only if the previous one succeeds.

`||`: Runs the next command only if the previous one fails.

`;`: Runs all commands in sequence regardless of success/failure.

### Keyboard shortcuts:

`Ctrl + C`: Cancels a running process.

`Tab`: Auto-completes commands or paths.

`Ctrl + L`: Clears the terminal screen.

### By the end of the lab, you’ll have navigated directories, created/deleted files and folders, and combined commands efficiently in Linux. 📁🛠️🔗
