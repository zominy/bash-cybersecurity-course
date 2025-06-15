# Module 2: Navigating Like a Pro - Filesystems & Permissions 🔍🗝️

## Introduction

In this tutorial, I cover how to navigate filesystems, how to find files, and how to change ownership and the permissions of files from the module 2 video. Feel free to use these to read up on the material or follow along in your own time.

## 🎯 Aims
By the end of this module, you should be able to:

Navigate the Linux filesystem with confidence

View and modify file permissions

Change file ownership

Use tools like find to efficiently search for files.

#### Note: Please check out the [command file](commands.md) or [errors file](errors.md) for help if you are struggling to understand any concepts.

---

## Module 2 Lab 🧪💻

### Part 1 - Adding ourselves to the sudoers file 🧍🛡️

The lab first begins by using the `whoami` command to learn our username. If it is a complex username then please keep a note of it.

Next, we type `su -` to switch to the root user. This will then prompt you to enter your root password that you set up the virtual environment with.

We then type in `visudo` which opens the sudoers file and allows us to edit it. Using the arrow keys we then navigate to the bottom of the file and enter a new line to where the command:
```bash
max ALL=(ALL:ALL) ALL
```
Where this command adds the user `max` or your username to the sudoers file and gives them all privileges.

After this use the keybind `Crtl + O` then enter to save your changes and then `Crtl + X` to exit the sudoers file. Rest assured your changes are saved.

Then type `exit` to log out from root and then `exit` again to close the terminal and restart the virtual machine. For me, this has always been necessary for this change to take place so hence why I am doing it this way but you are better off just restarting anyways.

Once back in the terminal, enter the `groups` command and you should see your username displayed alongside the word `sudo` meaning you have successfully added yourself to the sudoers file. Good work.

---

### Part 2 - Installing tree and navigating filesystems 🌲📂🧭

