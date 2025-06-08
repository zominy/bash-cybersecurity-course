# 📖 List of Commands used in Module 1: Intro to Bash - The Cybersecurity Shell

## ls 📋

Explanation: 'ls' stands for 'list' and when entered, it displays all the files and directories in the current directory, or a selected one.

Example:

```bash
maxz@zom:~$ ls
Documents
Downloads
Music
dira
newfile1.txt
maxz@zom:~$
```
As you can see in the demo above, I entered 'ls' while in my home directory and it listed all the contents of the directory.
View the example below to see how to list a selected directory.

Example 2:

```bash
maxz@zom:~$ ls ./dira/dirc
data1.txt  
data2.txt  
data3.txt  
data4.txt  
file2.txt
maxz@zom:~$
```
This example is a little more complicated but the exact same thing. The './' prefix refers to the current directory. It ensures that you are targeting a path relative to where you are.
As can be seen in the first example, 'dira' is a subdirectory of the home directory; Think of it as navigating deeper: ./ (current directory) → dira/ → dirc/.

Here is the file hierarchy:
```
HOME
└── dira
    └── dirc
```
And here is the path:
```
HOME > DIRA > DIRC
```
## pwd 📂

Explanation: 'pwd' stands for 'print working directory' which displays the full absolute path of your current directory.
It is pretty useful to know where you are in the filesystem, especially when the file structure is complicated.

Example:

```bash
maxz@zom:~$ 
maxz@zom:~$ pwd
/home/maxz
maxz@zom:~$ cd Downloads
maxz@zom:~/Downloads$ pwd
/home/maxz/Downloads
maxz@zom:~/Downloads$
```
At the beginning, I am idle in the home directory in my terminal. I then enter the 'pwd' command and it shows me my absolute path which is /home/maxz.

I then use the cd command to change into the Downloads directory (cd Downloads) which is a subdirectory of my home folder. Running the 'pwd' command again shows me my new absolute path /home/maxz/Downloads.

## cd 📁➡️

Explanation: 'cd' stands for change directory and is essential for navigating the file system in Linux.

Example:
Here is the directory structure:

```
HOME
└── dira
    └── dirc
        └── dird
```
### There are two ways you can do this, here is an inefficient method of navigating a path one level at a time:
```bash
maxz@zom:~$ cd dira
maxz@zom:~/dira$ cd dirc
maxz@zom:~/dira/dirc$ cd dird
maxz@zom:~/dira/dirc/dird$
```
### One by one is very inefficient. You can string these together to do it in one:
```bash
maxz@zom:~$ cd dira/dirc/dird
maxz@zom:~/dira/dirc/dird$
```
### You can see with a single command, I am in the same directory, 'dird', as if I were to use 3.
To follow this up, you can type 'cd' and enter to return to the home directory regardless where you are.

If we use 'pwd' we can determine where we are:
```bash
maxz@zom:~/dira/dirc/dird$ pwd 
/home/maxz/dira/dirc/dird
```
### From here we can just enter 'cd' as is, and return back to the home directory:
```bash
maxz@zom:~/dira/dirc/dird$ cd
maxz@zom:~$ pwd
/home/maxz
```
### Now, let's try navigating up and down the file hierarchy
When you enter 'cd ..', you go up one level in the directory tree. So, if I were in dirc and entered 'cd ..' I would simply just go up to dira.

Examine:
```bash
maxz@zom:~/dira/dirc$ cd ..
maxz@zom:~/dira$
```
### With this in mind, consider the file structure below:
```
HOME
└── dira
    ├── dirc
    │   └── dird
    └── dire
```
So let's navigate from dird to dire in one command. Here is how it is done:
```bash
maxz@zom:~/dira/dirc/dird$ cd ../../dire
maxz@zom:~/dira/dire$
```
As you can see, we go up twice to dira, then into dire as dire is a subdirectory of dira.

## mkdir / rmdir ➕📁 / ❌📁

Explanation: 'mkdir' stands for 'make directory'. It's a simple command where you can enter: mkdir 'name of directory' and it will create a new directory under your current directory.

Example:

```bash
maxz@zom:~/Downloads$ ls
dirlist1.txt 
dirlist2.txt

maxz@zom:~/Downloads$ mkdir DownloadsNEW

maxz@zom:~/Downloads$ ls
dirlist1.txt 
dirlist2.txt 
DownloadsNEW

maxz@zom:~/Downloads$ rmdir DownloadsNEW

maxz@zom:~/Downloads$ ls
dirlist1.txt  
dirlist2.txt
```
We begin in the Downloads directory. Then enter 'ls' to see the contents of that directory.
As shown, there are two text files.

Next, I enter 'mkdir DownloadsNEW' and once I re-list the Downloads directory, you can see the new directory I created called DownloadsNEW. Then, to remove this directory I use 'rmdir' which removes the directory.

Note: rmdir only works if the directory is empty. To delete a directory with contents you will have to use 'rm -rf' if the directory is not empty. Be careful though because that command incinerates anything it's used on and it does not come back!

## nano 📝

Explanation: nano is a lightweight command-line text editor which can be used to create or modify files.

If you type 'nano' followed by an existing file name in that directory then you edit that file. If the file does not exist it creates a new file and places you in the text editor. It is pretty good for scripting and what will be used in this course due to its simplicity and speed in comparison to the vi editor which can feel janky especially to new users.

## echo 💬

Explanation: echo is used to print text or variables to the terminal.

For example:

```bash
maxz@zom:~$ echo I am a terminal
I am a terminal
maxz@zom:~$
```
Example 2 (Variables):

```bash
#!/bin/bash     # This is a shebang, this must be always included as the first thing in your script to make it executable. This specifies the script interpreter.

name="Max"      # Stores Max under the variable 'name'

echo "Hello, $name"   # Basically saying to print Hello, Max. If the value of 'name' changes, the output will change accordingly.
```
Let's run the script:

```bash
maxz@zom:~$ ./echo.sh
Hello, Max
```
Cool!

## Clear 🧹
Explanation: Using the clear command just clears the current terminal. It is useful to clear a cluttered terminal output without affecting files or processes.

## whoami 🙋‍♂️
Explanation: 'whoami' is a simple command that prints the username of the current user logged into the shell. To verify elevated privileges, use 'sudo whoami' and if the output is 'root' you are operating with administrative privileges.







