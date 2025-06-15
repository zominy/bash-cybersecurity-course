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

We begin by typing:
```bash
sudo apt install tree
```
This installs the tree tool so you can view your folders laid out nicely like branches of a tree... Then clear your terminal so we can start fresh.

In the video, we make a new file structure using:
```bash
mkdir -p lab/module2/test
```
We already know what `mkdir` does from module 1, makes directories, but what does the `-p` do? Essentially the `-p` just means to create the parent directories if they do not already exist. So lets compare.

If you ran:
```bash
mkdir lab/module2/test
```
The command will seach for a directory called `lab`, then a subdirectory within lab called `module2` and try create the `test` directory in there. This will return an error message since these directories do not exist. However if we instead enter:
```bash
mkdir -p lab/module2/test
```
Then the command will create the `lab`, and `module2` directories automatically if they didnt already exist. It is pretty handy if you are making a large file structure in one go.

---

Next we use:
```bash
cd lab/module2/test
```
To directly change into the test directory. We can then use:
```bash
cd ..
```
To simply go up a level. Using this would then put us in the parent directory, in this case, `lab/module2`.

Next, `cd` back home and use:
```bash
cd lab/module2/test/../../module2
```
This won't be used realistically but is to demo how `..` can be used in navigation. What the command is doing is going into the `lab` directory, then `module2`, then `test`, then up twice back into `lab`, then back into `module2`.

---

After this, `cd` back home, and use the command:
```bash
tree lab
```
This shows what the `tree` command does, as per in the video, you will see a clear display of what the `lab` directory looks like. As I said, this is great to vision file structures especially complex ones.

Now change into the root directory using `cd /` where `/` is the root directory and list the contents.

You will notice that there are several new contents here including the `etc` directory which has been touched on in 
