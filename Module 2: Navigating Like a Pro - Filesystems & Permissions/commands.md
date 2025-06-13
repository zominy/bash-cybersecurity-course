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


## touch 🆕📄

Explanation: Creates a new empty file, or updates the “last touched” time of an existing one. Think of it as giving a file a gentle nudge.



## chmod 🔐🛠️

Explanation: Changes file or folder permissions. Such as who can read, write, or run (execute) it.


## chown 👤🔑

Explanation: Changes the owner of a file or folder. Useful if something’s owned by the wrong user and needs a change of hands.



## Wildcards 🌟🃏

Explanation: Special characters (like * or ?) used to match filenames. For example, *.txt grabs all text files. Super useful for bulk stuff such as the /etc directory.



## find 🔍📂

Explanation: Searches for files and folders based on different criteria (name, size, permissions, etc.).



## find / -type f -perm -o=w 2>/dev/null 🚨📝

Explanation: An actual use case of the find command. This command searches the whole system (since we put root (`/`)) for files (`-type f`) that anyone can write to (`-perm -o=w`). The `2>/dev/null` bit hides error messages. Good for spotting dodgy file permissions.
