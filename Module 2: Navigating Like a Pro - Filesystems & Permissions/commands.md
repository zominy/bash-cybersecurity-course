# 📖 List of Commands used in Module 2: Navigating Like a Pro - Filesystems & Permissions

## tree 🌲📂

Explanation: Shows a nice little tree view of all the files and folders starting from where you are. Handy for seeing the layout of things.


## sudo 🛡️👑

Explanation: Lets you run stuff as the superuser (admin), so you can do things that normally need higher permissions such as installing tools, or checking certain files.



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
