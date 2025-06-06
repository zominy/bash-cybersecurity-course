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







  

pwd        # Print working directory
ls         # List directory contents
cd         # Change directory
echo       # Output text
clear      # Clear terminal
whoami     # Current user
; && ||    # Combine commands
🧪 Lab:
bash
Copy
Edit
# Open terminal and try the following:
pwd
ls
cd /etc
ls -l
whoami

# Combine commands:
cd /var/log; ls
cd /tmp && echo "We made it!" || echo "Failed to change directory"

# Shortcuts:
# Ctrl + C    → Cancel current process
# Ctrl + L    → Clear screen
# Tab         → Auto-complete commands
🏋️ Practice:
Navigate to /home and list its contents.

Combine: cd /var && pwd && ls

Try breaking a command and fixing it with Ctrl+C.
