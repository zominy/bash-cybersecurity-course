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

After this use the keybind `Ctrl + O` then enter to save your changes and then `Ctrl + X` to exit the sudoers file. Rest assured your changes are saved.

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
The command will search for a directory called `lab`, then a subdirectory within lab called `module2` and try create the `test` directory in there. This will return an error message since these directories do not exist. However if we instead enter:
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

You will notice that there are several new contents here including the `etc` directory which has been touched on in [Module 1](./Module%201:%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/) where the `etc` directory holds many important files for the operating system.

---

Now enter the command:
```bash
tree -L 1
```
This will just list the first level of the contents of the root directory. If we used the `tree` command itself then the output would be absolutely gigantic but since we are using `-L` (level) and `1` to clarify we just want to see the first level, then you will see:
```bash
.
├── bin -> usr/bin
├── boot
├── dev
├── etc
├── home
├── initrd.img -> boot/initrd.img-6.1.0-35-amd64
├── initrd.img.old -> boot/initrd.img-6.1.0-34-amd64
├── lib -> usr/lib
├── lib64 -> usr/lib64
├── lost+found
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin -> usr/sbin
├── srv
├── sys
├── tmp
├── trials
├── usr
├── var
├── vmlinuz -> boot/vmlinuz-6.1.0-35-amd64
└── vmlinuz.old -> boot/vmlinuz-6.1.0-34-amd64

22 directories, 4 files
```
Notice it is only the first level, as we stated. The tree command is not going into any of these directories and listing contents.

---

Now, use `cd /etc` to get into the `etc` directory and enter the command:
```bash
ls -lh
```
Look at one of the files' file size section, notice it is now in a readable size such as 4.0K for kilobytes instead of 4076. We still get the same output as `ls -l` but the `h` adds the file sizes as it stands for 'human-readable'.

### Part 3 - File permissions & ownership 🔐👤📄

Begin with `cd` to go back home and clear the terminal.

We will be using a new command to create an empty file like so:
```bash
touch newfile.txt
```
This creates a new empty file, or updates the “last touched” time of an existing one. Think of it as giving a file a gentle nudge.

Next, get a long listing of this file and examine the permissions section, specifically focussing on the `-rw-r--r--` section of the file since this is these are the permissions that we will be changing.

It is important to note that `r` stands for read where we can read the contents of a file, `w` stands for write where we can write to a file, and `x` for execute where you can execute the file.

The string of characters `-rw-r--r--` breaks down into three sections that refer to different types of users. The first group of three letters applies to the owner of the file, the second group applies to the group associated with the file, and the third applies to others, which means everyone else essentially. In this case, the file starts with read and write permissions for the owner, read only for the group, and read only for others.

Next, enter:
```bash
chmod 700 newfile.txt
```
Get a long listing of this file too to see the permissions. Please split the 700 into three separate digits where the first digit applies to the owner, the second digit to the group, and the third digit others (anyone else).

Each permission type corresponds to a number:

read (r) is worth 4

write (w) is worth 2

execute (x) is worth 1

To set permissions, you add these values together. So if you want the owner to have read, write, and execute, you would add 4 + 2 + 1, giving you 7. If the group should only have read access, that’s 4. No permissions at all would be 0. That’s how you get numbers like 700, 400, or 100.

Remember when we first listed the new file. We started with default permissions: `-rw-r--r--`, which means the owner can read and write, and both the group and others can only read.

When we run chmod 100 newfile1, we change the permission to `---x------`. This means only the owner can execute the file, but no one else, including the group and others, has any access.

With chmod 300 newfile1, the permissions become `--wx------`. Now the owner can write and execute, but still cannot read the file, and the group and others have no access.

Then, chmod 400 newfile1 changes it to `-r--------`. This is read-only access for the owner.

Finally, chmod 700 newfile1 gives full access to the owner — read, write, and execute — and no access for the group or others, shown as `-rwx------`.

Understanding these numbers helps you control exactly who can do what with your files. It is a good habit to be deliberate about file permissions to keep your data secure.

---

Next we will change ownership of this file to ourselves. The file is already owned by us but this is under the assumption of where it would not be.

From Video:
```bash
max@maxz:~$ sudo chown $USER:$USER newfile.txt
[sudo] password for max: 
max@maxz:~$ 
```

This command required sudo privileges and changes the owner to us. Lets break it down.

`sudo` – Runs the command that follows with superuser privileges.

`chown` – Stands for "change owner" and is used here to change the ownership of the file to me.

`$USER:$USER` – This uses a shell variable (`$USER`) to insert the name of the current user. The first `$USER` sets the file’s owner, and the second `$USER` after the colon sets the group owner to the same user.

`newfile1` – This is the name of the target file whose ownership is being changed to us.

---

### Part 4 - Find & Wildcards 🔍🌟

Next, we then use the command:
```bash
find /etc -name "*.conf"
```
The `find` command searches for files and folders based on different criteria, in this case it is for all files that end in `.conf`.

The asterisk/star is a 'wildcard' and works as so:

Using asterisks or question marks to match filenames. For example, `*.txt` grabs all text files. Super useful for bulk stuff such as the /etc directory.

The find command is then used with wildcards like `*` which basically means 'anything' and specific patterns to match file names. For example, `*.txt` grabs everything that ends in .txt, while `file2*` finds anything that starts with file2. The pattern `*1*` matches anything with a 1 anywhere in the name.

This is a fast way to search for groups of files by name, type, or number without needing to look through each one manually. In this case, we are searching through the etc directory for files ending in `.conf`

---

We then run through a few more examples:

```bash
find /etc -name "*ssh*"
```
- Searching for files containing 'ssh'
```bash
find /etc -name "*s*"
```
- Searching for files that begin with 's'
```bash
find /etc -name "*log*conf"
```
- Searching for files that contain the word 'log' and ends in 'conf'. Returns permission denied.
```bash
sudo find /etc -name "*log*conf"
```
- Since the command returned permission denied, we must run it as the sudoer; hence the sudo at the beginning.

---

#### An actual use case of the 'find' command

In the video, we then use the command:
```bash
find / -type f -perm -o=w 2>/dev/null
```

This is an actual use case of the find command. This command searches the whole system (since we put root (`/`)) for files (`-type f`) that anyone can write to (`-perm -o=w`). The `2>/dev/null` bit hides error messages. Good for spotting dodgy file permissions.

#### Note: This command is used in module 3 again.
