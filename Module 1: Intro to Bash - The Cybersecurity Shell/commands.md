# 📖List of Commands used in Module 1

## ls

Explanation: 'ls' stands for 'list' and when entered, it lists all the files and directories in the current directory, or a selected one.

Example:

maxz@zom:~$ ls

Documents

Downloads

Music

dira

newfile1.txt

maxz@zom:~$

As you can see in the demo above, I entred 'ls' into my home directory and it listed all the contents of the directory.
View the example below to see how to list a selected directory.

Example 2:

maxz@zom:~$ ls ./dira/dirc

data1.txt  
data2.txt  
data3.txt  
data4.txt  
file2.txt

maxz@zom:~$ 

This example is a little more complicated but the exact same thing. The './' at the beginning of the command just means 'start right where I am' pretty much.
As can be seen in the first example, 'dira' is a subdirectory of the home directory; picture in your head that we went from './ - current directory' to '/dira - We are now inside the dira directory' then again '/dirc - We are now in the dirc directory'. 'dirc' is a subdirectory of dira.

Here is the file hierarchy:
HOME

-dira

--dirc

And here is the path:

HOME > DIRA > DIRC


## pwd

Explanation: 'pwd' stands for 'print working directory'. When this command is entered it will display the absolute path of the current directory that you are in, in that moment.
It is pretty useful to know where you are in the filesystem, especially when the file structure is complicated.

Example:

maxz@zom:~$ 

maxz@zom:~$ pwd

/home/maxz

maxz@zom:~$ cd Downloads

maxz@zom:~/Downloads$ pwd

/home/maxz/Downloads

maxz@zom:~/Downloads$ 

At the beginning, I am idle in the home directory in my terminal. I then enter the 'pwd' command and it shows me my absolute path which is /home/maxz.

I then use the cd command to change into the Downloads directory; a subdiractory of my home directory. After that, I re-enter the 'pwd' command to which it shows me my new absolute path /home/maxz/Downloads.
  
## cd

Explanation: 'cd' stands for change directory and is going to be a very commonly used command for this course and in general as it enables you to navigate the file system. Very important to undertsand 'cd' and how effective it can be.

### Example:

Here is the directory structure:
HOME

-dira

--dirc

---dird

### There are two ways you can do this, there is first the inefficient method of doing it as such:

maxz@zom:~$ cd dira

maxz@zom:~/dira$ cd dirc

maxz@zom:~/dira/dirc$ cd dird

maxz@zom:~/dira/dirc/dird$ 

### One by one is very inefficient. You can string these together to do it in one:

maxz@zom:~$ cd dira/dirc/dird

maxz@zom:~/dira/dirc/dird$ 

### You can see with one singular command I am in the same directory, 'dird', as if I were to use 3.

To follow this up, you can type 'cd' and enter to return to the home directory regardless where you are.

If we use 'pwd' we can determine where we are: 

maxz@zom:~/dira/dirc/dird$ pwd 

/home/maxz/dira/dirc/dird

### From here we can just enter 'cd' as is, and return back home:

maxz@zom:~/dira/dirc/dird$ cd

maxz@zom:~$ pwd

/home/maxz

### Now, let's try navigating up and down the file hierarchy

When you enter 'cd ..', you go up one level. So, if I were in dirc and entered 'cd ..' I would simply just go up to dira.

Examine:

maxz@zom:~/dira/dirc$ cd ..

maxz@zom:~/dira$ 

### With this in mind, let's try a new file structure:

HOME

-dira

--dirc & dire

---dird

So let's navigate from dird to dire in one command. Here is how it is done:

maxz@zom:~/dira/dirc/dird$ cd ../../dire

maxz@zom:~/dira/dire$ 

As you can see, we go up twice to dira, then into dire as dire is a subdirectory of dira.

## mkdir / rmdir

Explanation: 'mkdir' stands for 'make directory'. It a simple command where you can enter: mkdir 'name of directory' and it will create a new directory under your current directory.

Example: 

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

maxz@zom:~/Downloads$

We begin in the Downloads directory. Then enter 'ls' to see the contents of that directory.
As can be seen, there is only two text files.

Next, I enter 'mkdir DownloadsNEW' and once I re-list the Downloads directory, you can see the new directory I created called DownloadsNEW. Then, to remove this directory I use 'rmdir' which removes the directory.

Note: rmdir only works if the directory is empty, you will have to use 'rm -rf' if the directory is not empty. Be careful though becuase that command incinerates anything it's used on and it does not come back!

## nano

Explanation: nano is a command line text editor which is very simple and fast to use. It can create or edit text files.

If you type 'nano' followed by an existing file name in that directory then you edit that file. If the file does not exist it creates a new file and places you in the text editor. It is pretty good for scripting and what will be used in this course due to it's simplicity and speed in comparison to the vi editor which can feel janky especially to new users.

## echo

Explanation: echo is used to print text or variables to the terminal.

### For example:

maxz@zom:~$ echo I am a terminal

I am a terminal

maxz@zom:~$ 

### Example 2 (Variables):

Here is a simple script (First 'nano echo.sh' to make the file.):

#!/bin/bash - This is a shebang (cool name), this must be always included as the first thing in your script to make it executable.

name="Max" - Stores Max under the variable 'name'

echo "Hello, $name" - Basically saying to print Hello, Max. If the variable 'name' was assigned a different string the answer wouldnt be the same.

Let's run the script

maxz@zom:~$ ./echo.sh

Hello, Max

Cool!

## Clear

Explanation: Using the clear command just clears the current terminal. Pretty handy if things are messy.

## whoami

Explanation: 'whoami' is a simple command that prints the username of the current user logged into the shell. In cybersecurity, you can use 'sudo whoami' to determine whether you are the root user when penetrating systems. Pretty handy. 
