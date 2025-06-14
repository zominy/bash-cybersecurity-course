# 📖 List of Commands used in Module 2: Navigating Like a Pro - Filesystems & Permissions

## tree 🌲📂

Explanation: Shows a nice little tree view of all the files and folders starting from where you are. Handy for seeing the layout of things.

Example:
```bash
maxz@zom:~$ tree dira
dira
├── dirb
├── dirc
│   ├── data1.txt
│   ├── data2.txt
│   ├── data3.txt
│   ├── data4.txt
│   ├── dird
│   └── file2.txt
├── dird
├── dire
├── file1.txt
├── file3.txt
├── file4a.txt
├── info
└── sortedfile1.txt

6 directories, 10 files
maxz@zom:~$ 
```

I like this command, it is very useful and great when working with complex file structures.

## sudo 🛡️👑

Explanation: Lets you run stuff as the superuser (admin), so you can do things that normally need higher permissions such as installing tools, or checking certain files.

---

### Uninstalling a Tool:

```bash
maxz@zom:~$ sudo apt remove tree
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following package was automatically installed and is no longer required:
  linux-image-6.1.0-29-amd64
Use 'sudo apt autoremove' to remove it.
The following packages will be REMOVED:
  tree
0 upgraded, 0 newly installed, 1 to remove and 33 not upgraded.
After this operation, 116 kB disk space will be freed.
Do you want to continue? [Y/n] y
(Reading database ... 159230 files and directories currently installed.)
Removing tree (2.1.0-1) ...
Processing triggers for man-db (2.11.2-2) ...
```

---

### Comparison between searching the /etc directory with sudo, vs without 🔒🧱

Without:

```bash
maxz@zom:~$ find /etc -name "*.sh"
/etc/rc6.d/K01hwclock.sh
/etc/rc5.d/S01console-setup.sh
/etc/profile.d/im-config_wayland.sh
/etc/profile.d/bash_completion.sh
/etc/profile.d/vte-2.91.sh
/etc/profile.d/gnome-session_gnomerc.sh
/etc/xdg/plasma-workspace/env/env.sh
/etc/alternatives/desktop-grub.sh
/etc/rc0.d/K01hwclock.sh
/etc/rc2.d/S01console-setup.sh
find: ‘/etc/ssl/private’: Permission denied
/etc/wpa_supplicant/functions.sh
/etc/wpa_supplicant/action_wpa.sh
/etc/wpa_supplicant/ifupdown.sh
/etc/rcS.d/S01keyboard-setup.sh
/etc/rcS.d/S01hwclock.sh
find: ‘/etc/polkit-1/rules.d’: Permission denied
/etc/libreoffice/soffice.sh
/etc/console-setup/cached_setup_font.sh
/etc/console-setup/cached_setup_terminal.sh
/etc/console-setup/cached_setup_keyboard.sh
find: ‘/etc/cups/ssl’: Permission denied
/etc/init.d/hwclock.sh
/etc/init.d/keyboard-setup.sh
/etc/init.d/console-setup.sh
/etc/rc3.d/S01console-setup.sh
/etc/rc4.d/S01console-setup.sh
```
---

You can see, it is stated several times, 'permission denied'. Lets try running with sudo:

```bash
maxz@zom:~$ sudo find /etc -name "*.sh"
/etc/rc6.d/K01hwclock.sh
/etc/rc5.d/S01console-setup.sh
/etc/profile.d/im-config_wayland.sh
/etc/profile.d/bash_completion.sh
/etc/profile.d/vte-2.91.sh
/etc/profile.d/gnome-session_gnomerc.sh
/etc/xdg/plasma-workspace/env/env.sh
/etc/alternatives/desktop-grub.sh
/etc/rc0.d/K01hwclock.sh
/etc/rc2.d/S01console-setup.sh
/etc/wpa_supplicant/functions.sh
/etc/wpa_supplicant/action_wpa.sh
/etc/wpa_supplicant/ifupdown.sh
/etc/rcS.d/S01keyboard-setup.sh
/etc/rcS.d/S01hwclock.sh
/etc/libreoffice/soffice.sh
/etc/console-setup/cached_setup_font.sh
/etc/console-setup/cached_setup_terminal.sh
/etc/console-setup/cached_setup_keyboard.sh
/etc/init.d/hwclock.sh
/etc/init.d/keyboard-setup.sh
/etc/init.d/console-setup.sh
/etc/rc3.d/S01console-setup.sh
/etc/rc4.d/S01console-setup.sh
```

Now, there is no more 'permission denied' due to me running the command with the appropriate privileges.

---

There will be instances where an error will pop up saying something like:

```bash
maxz@zom:~$ apt install tree
E: Could not open lock file /var/lib/dpkg/lock-frontend - open (13: Permission denied)
E: Unable to acquire the dpkg frontend lock (/var/lib/dpkg/lock-frontend), are you root?
```

This error will arise due to you not running the command with the necessary privilege. If you are having issues with sudo, or adding yourself to the sudoers file. Please check out the [errors](errors.md) file for this module.


## ls -lh 📁📐

Explanation: Lists files in a folder, with human-friendly sizes (MB/KB). The `-l` gives more detail, and `-h` makes it readable.

Example:
```bash
maxz@zom:~$ ls -lh dirlist1.txt
-rw-r--r-- 1 maxz maxz 1.8K May 11 07:18 dirlist1.txt
```

`ls -l` gives you a detailed list of files, but the file sizes are in raw bytes, which can be difficult to read as a human; for example, `1843` instead of `1.8K`.

`ls -lh` adds the `-h` for “human-readable,” so sizes show up as K, M, G, etc.

So essentially the same info but easier to read.

## touch 🆕📄

Explanation: Creates a new empty file, or updates the “last touched” time of an existing one. Think of it as giving a file a gentle nudge.

Example:
```bash
maxz@zom:~$ touch new_file
```

This file is completely blank, you can use `nano` or `vi` to edit it, regarding your preference.


## chmod 🔐🛠️

Explanation: Changes file or folder permissions. Such as who can read, write, or run (execute) it.

Here is an example:
```bash
max@maxz:~$ touch newfile1 && ls -l newfile1
-rw-r--r-- 1 max max 0 Jun 14 22:14 newfile1
max@maxz:~$ chmod 100 newfile1
max@maxz:~$ ls -l newfile1
---x------ 1 max max 0 Jun 14 22:14 newfile1
max@maxz:~$ chmod 300 newfile1; ls -l newfile1
--wx------ 1 max max 0 Jun 14 22:14 newfile1
max@maxz:~$ chmod 400 newfile1; ls -l newfile1
-r-------- 1 max max 0 Jun 14 22:14 newfile1
max@maxz:~$ chmod 700 newfile1; ls -l newfile1
-rwx------ 1 max max 0 Jun 14 22:14 newfile1
max@maxz:~$ 
```

We first begin by making an empty file and then getting a long listing of the new file. For this, we will be focussing on the `-rw-r--r--` section of the file since this is these are the permissions that we will be changing.

It is important to note that `r` stands for read where we can read the contents of a file, `w` stands for write where we can write to a file, and `x` for execute where you can execute the file.

The string of characters -rw-r--r-- breaks down into three sections that refer to different types of users. The first group of three letters applies to the owner of the file, the second group applies to the group associated with the file, and the third applies to others, which means everyone else essentially. In this case, the file starts with read and write permissions for the owner, read only for the group, and read only for others.

Each permission type corresponds to a number:

read (r) is worth 4

write (w) is worth 2

execute (x) is worth 1

To set permissions, you add these values together. So if you want the owner to have read, write, and execute, you would add 4 + 2 + 1, giving you 7. If the group should only have read access, that’s 4. No permissions at all would be 0. That’s how you get numbers like 700, 400, or 100.

Let’s look at the example. We start with default permissions: -rw-r--r--, which means the owner can read and write, and both the group and others can only read.

When we run chmod 100 newfile1, we change the permission to ---x------. This means only the owner can execute the file, but no one else, including the group and others, has any access.

With chmod 300 newfile1, the permissions become --wx------. Now the owner can write and execute, but still cannot read the file, and the group and others have no access.

Then, chmod 400 newfile1 changes it to -r--------. This is read-only access for the owner.

Finally, chmod 700 newfile1 gives full access to the owner — read, write, and execute — and no access for the group or others, shown as -rwx------.

Understanding these numbers helps you control exactly who can do what with your files. It is a good habit to be deliberate about file permissions to keep your data secure.


## chown 👤🔑

Explanation: Changes the owner of a file or folder. Useful if something’s owned by the wrong user and needs a change of hands.

Example (Using file from chmod definition)
```bash
max@maxz:~$ sudo chown $USER:$USER newfile1
[sudo] password for max: 
max@maxz:~$ 
```

This command required sudo privileges and changes the owner to us. Lets break it down.

`sudo` – Runs the command that follows with superuser privileges.
`chown` – Stands for "change owner" and is used here to change the ownership of the file to me.
`$USER:$USER` – This uses a shell variable (`$USER`) to insert the name of the current user. The first `$USER` sets the file’s owner, and the second `$USER` after the colon sets the group owner to the same user.
`newfile1` – This is the name of the target file whose ownership is being changed to us.


## Wildcards 🌟🃏

Explanation: Using asterisks or question marks to match filenames. For example, *.txt grabs all text files. Super useful for bulk stuff such as the /etc directory.

Example:
```bash
max@maxz:~$ tree chess
chess
├── file1.conf
├── file1.txt
├── file2.conf
├── file2.txt
├── file.conf
└── file.txt

1 directory, 6 files
max@maxz:~$ find ./chess -name "*.txt"
./chess/file2.txt
./chess/file.txt
./chess/file1.txt
max@maxz:~$ find ./chess -name "*.conf"
./chess/file.conf
./chess/file2.conf
./chess/file1.conf
max@maxz:~$ find ./chess -name "*1*"
./chess/file1.conf
./chess/file1.txt
max@maxz:~$ find ./chess -name "file2*"
./chess/file2.txt
./chess/file2.conf
max@maxz:~$ 

```



## find 🔍📂

Explanation: Searches for files and folders based on different criteria (name, size, permissions, etc.).



## find / -type f -perm -o=w 2>/dev/null 🚨📝

Explanation: An actual use case of the find command. This command searches the whole system (since we put root (`/`)) for files (`-type f`) that anyone can write to (`-perm -o=w`). The `2>/dev/null` bit hides error messages. Good for spotting dodgy file permissions.
