# Module 3: Variables, Loops, and Conditionals - Building Logic for Defense 🔁🛡️

## Introduction

In this tutorial I go over the basics of bash scripting; including variables, if statements, for loops, and touching on some new directories such as /tmp and /var/log.

## Aims 🎯
- To be able to use variables freely
- To be able to use if statements and for loops efficiently
- Nest loops
- Create a simple log scanner

## Important | What is a syntax? ❗💬

A syntax is the rules about how you have to write things in a programming or scripting language. Think of it like grammar in English. If you were to write "I going store to the" its not correct grammar. It has the right words, but the order and structure are wrong. The same idea applies to Bash scripts and other languages. You have to write things in a specific way so the computer understands what you mean, if you write things incorrectly then the script will not run.

#### Note: Please check out the [command file](commands.md) or [errors file](errors.md) for help if you are struggling to understand any concepts.

---

## Module 3 Lab 🛡️💻

### Important | When saving scripts always use Ctrl + O, Enter, then Ctrl X. Not saving will result in lost progress.
---

### Part 1 - Creating a script file 📄⚙️

In the beginning of the lab we use the command:
```bash
touch s1.sh; chmod 700 s1.sh; nano s1.sh
```
This creates a new empty file called `s1.sh` then gives us executable privileges (`chmod 700`) and lastly opens the text editor (`nano s1.sh`).

When creating script files it is best practice to make them end in `.sh`. Also we need executable rights so that we can actually run these files.

---

### Part 2 - The Fundamentals 📚🧠

The very first thing we write in this new file is a shebang and it looks like this:
```bash
#!/bin/bash
```
A shebang `#!` tells the system which interpreter to use to run a script. By writing `#!/bin/bash`, you're telling the system to use the Bash shell (the location is '/bin/bash') to interpret and execute the script's commands. It's the first line in many shell scripts and ensures the correct program runs the script. We use it for this course because it is a Bash course after all.

Then come down to lines by pressing enter twice (not necessary, just looks nice which is important to ensure readability as the scripts get more complicated) and write:

```bash
echo "Hello"
```
We know from module 1 that echo is essentially just the print command.

Your script should now look like this:
```bash
#!/bin/bash

echo "Hello"
```
Press Ctrl + O and enter to save, then Ctrl + X to exit.

Now back in the terminal, write `./s1.sh` and press enter, and you will see it runs and prints 'Hello'.

This is how scripts are executed. You essentially write `./<script name>.sh`.

Now lets go back into the file by typing `nano s1.sh` to open the text editor again. Delete what is there but leave the shebang, it must always be there. Write:
```bash
#!/bin/bash
USERNAME=$(whoami)
DATE=$(date +%F)
```
These are variables, they store information under a name and can be reused throughout a program.

Lets talk about `USERNAME=$(whoami)`. This example stores the text output of the whoami command under the variable 'USERNAME'. The command must be encapsulated like so: $(command). So it will run the whoami command in the terminal and just hold the result. If your username is 'david' then it will store the name 'david'.

It is the same for the date. It stores the date in YYYY-MM-DD format. Lets actually do something with these new variables. Write:

```bash
echo "Your name is, $USERNAME. The date is $DATE"
```
So your script should now look like this:
```bash
#!/bin/bash
USERNAME=$(whoami)
DATE=$(date +%F)

echo "Your name is, $USERNAME. The date is $DATE"
```
This will now use the variable results as substitutes. Save and exit and then run the script.

The output will be: `Your name is, david. The date is 2025-06-20`

---

### Part 3 - If statements 🤔📍

Open the text editor for `s1.sh` once again.

Delete the echo command. Write in this:
```bash
if [ "$USERNAME" = "root" ]; then
  echo "You are the admin"
else
  echo "You are not the admin"
fi
```
Ensure that it is written the exact same way, that spaces are in the correct places or else it will cause a syntax error and the script will not run. Make sure to tab in the correct places to correctly integrate the script to ensure readability.

Now that you've added the if statement, your script isn’t just printing a message anymore its now making a decision based on something, and in this case it is the value of USERNAME.

The script checks if the value of USERNAME is "root" (the system admin)

If it is, it shows the message: "You are the admin".

If its not, it shows: "You are not the admin".

This is called a conditional, which means the script will do one thing or another based on a certain condition.

Before this, the script just ran one command. Now its flexible and it can take different paths. Be careful with spacing: There must be a space after the [ and before the ], or the script won’t work due to a syntax error.

---

Loops are written in a certain way, examine the diagram to learn how:

| Line               | Meaning                                |
|--------------------|----------------------------------------|
| if [ condition ];  | Start the check. Must have spaces and variables and strings must be encased in "".     |
| then               | If the condition is true, do this next.|
| ... (commands)     | What to do if condition is true.       |
| else               | (Optional) What to do if it's false. You could just end it here if you wanted.   |
| ... (commands)     | Commands for the "false" case if the condition isnt met.         |
| fi                 | Ends the if-statement block. (Mandatory to tell the system it is done)           |

Your script should now look like this:
```bash
#!/bin/bash
USERNAME=$(whoami)
DATE=$(date +%F)

if [ "$USERNAME" = "root" ]; then
        echo "You are the admin"
else
        echo "You are not the admin"
fi
```
So lets break it down individually as a recap:

`#!/bin/bash`
- The shebang tells the system to use Bash to run the script.

`USERNAME=$(whoami)`
- This stores the current user's name into the USERNAME variable.

`DATE=$(date +%F)`
- This stores the current date in YYYY-MM-DD format into the DATE variable.

`if [ "$USERNAME" = "root" ]; then`
- if the value of USERNAME is "root" then...

`echo "You are the admin"`
- If it is true print "You are the admin"

`else`
- Otherwise...

`echo "You are not the admin"`
- Print "you are not the admin"

`fi`
- End of the if statement

Please now save and exit and then run the file normally. The script will then tell you that you are not the admin assuming you ran it using `./s1.sh`.

To run a script as an admin, simply type `sudo` before the script execution. Example: `sudo ./s1.sh`. Running it like this will then print "You are the admin" successfully verifying you as the admin.

---

### Part 4 - For Loops 🔄📊

A `for` loop in Bash repeats commands for each item in a list.

Please create a new file called `s2.sh` and give yourself executable privileges, then open the text editor as in the video. Begin by putting a shebang at the beginning of the script. Please note that variables are case sensitive.

Then proceed to write:
```bash
for int in 1 2 3 4 5; do
  echo "Number: $int"
done
```
So that your script should look like this:
```bash
#!/bin/bash

for int in 1 2 3 4 5; do
  echo "Number: $int"
done
```
Please save and exit, then run it.

This will print:

Number:1

Number:2

Number:3

Number:4

Number:5

It works like so:

`for int in 1 2 3 4 5; do`
- Start a loop where the variable `int` takes each value in the list `1 2 3 4 5` one by one. The `do` means to start running the commands below for each value.

`  echo "Number: $int"`
- Print the current value of `int` with the text "Number:". The `$int` inserts the value of the variable `int`.

`done`
- Ends the loop

Here is how for loops function:

| Line                      | What it Does                                        |
|---------------------------|----------------------------------------------------|
| for 'variable' in list; do  | Starts the loop. The variable takes each value in the list, one at a time. The `do` marks the start of the loop's commands. |
| ... (commands)            | Commands that run for each value of the variable.  |
| done                     | Ends the loop. The script continues as normal after this.    |

Please open the editor for `s2.sh` again.

We can use a brace expansion to make this easier or if we had a higher count of numbers, as such:
```bash
#!/bin/bash

for int in {1..30}; do
  echo "Number: $int"
done
```
This will repeat the process for all numbers from 1 (The lowest stated value) up to 30 (The highest value). Please save it and run it to view the output.

Then create a new script called `s3.sh`. I know the names are bad. Then write:
```bash
#!/bin/bash

for file in /var/log/*.log; do
  echo "Scanning: $file"
  grep "error" "$file"
done
```
Note: /var/log is a directory (folder) on Linux systems where the system stores log files.

This is a simple loop that scans log files in /var/log for the word 'error'.

```bash
`#!/bin/bash`
```
- Shebang: uses Bash to run the script.
```bash
for file in /var/log/*.log; do
```
- Starts a loop. file takes each .log file in /var/log. We know this because of wildcards in module 2. We are scanning all files ending in `.log`.
```bash
echo "Scanning: $file"
```
- Prints which file is being scanned.
```bash
grep "error" "$file"
```
- Searches for the word "error" inside the current file.

`done`
- Ends the loop.

---

### Part 5 - Getting Our Hands Dirty 🧤🛠️

Create a new file called `simple_scanner.sh` as it is in the video. Apply appropriate privileges and open the editor.

This may feel like a little shove in the water but trust me here, it is nothing we have not covered. Here is what we will write but try typing it out yourself following the steps, and feel free to reference the code if needed. For now have a peek:
```bash
#!/bin/bash
USERNAME=$(whoami)
DATE=$(date +%F)

echo "Welcome, $USERNAME. Today's date is $DATE"
sleep 1
echo "Initialising..."
sleep 3

if [ "$USERNAME" = "root" ]; then
    echo "Root User Detected"
    sleep 2

    for dir in /home /tmp /var; do
        echo ""
        echo "Scanning $dir for world-writable files"
        sleep 2

        find "$dir" -type f -perm -o=w 2>/dev/null
        sleep 1
    done

    for logfile in /var/log/*.log; do
        echo "Checking $logfile..."
        grep "error" "$logfile"
    done

    echo "Scan Complete!"
else
    echo ""
    echo "You are not the root user, so you cannot use this script"
fi

echo "Done"
```
#### Step 1:

Shebang
```bash
#!/bin/bash
```
- Tells system to use Bash for the script, you know this.

#### Step 2:

Set variables
```bash
USERNAME=$(whoami)
DATE=$(date +%F)
```
- USERNAME stores the username of whoever is running the script.
- DATE 	stores the current date in the format YYYY-MM-DD.

#### Step 3:

Greeting the user
```bash
echo "Welcome, $USERNAME. Today's date is $DATE"
```
- This uses variable substitution to insert the user's name and the date into a welcome message.

#### Step 4

Adding a pause for realism
```bash
sleep 1
echo "Initialising..."
sleep 3
```
- `sleep` makes the script feel more interactive, like a real program.
- It's a short delay so that the user can read what's happening also it looks just more realistic, it simulates loading.

#### Step 5

Checking if the user is root
```bash
if [ "$USERNAME" = "root" ]; then
```
Again:

| Line               | Meaning                                |
|--------------------|----------------------------------------|
| if [ condition ];  | Start the check. Must have spaces and variables and strings must be encased in "".     |
| then               | If the condition is true, do this next.|
| ... (commands)     | What to do if condition is true.       |

#### Step 6

Print message acknowledging the root user's presence:
```bash
echo "Root User Detected"
sleep 2
```
- Let the user know they have admin power, then pause briefly before scanning.

#### Step 7

For loop scanning directories:
```bash
for dir in /home /tmp /var; do
    echo ""
    echo "Scanning $dir for world-writable files"
    sleep 2
    find "$dir" -type f -perm -o=w 2>/dev/null
    sleep 1
done
```
| Code                                             | What It's Doing                                                   |
|--------------------------------------------------|--------------------------------------------------------------------|
| `for dir in /home /tmp /var; do`       | Start a loop that goes through each directory: /home, /tmp, /var. The spaces seperate it so that it goes through each directory individually. /tmp is a temporary directory used to store temporary files created by programs or users.   |
| `echo ""`                    | Print a blank line for better spacing.                              |
| `echo "Scanning $dir for world-writable files"`  | Tell the user which directory is being scanned using variable substitution again.                     |
| `sleep 2`                       | Pause for 2 seconds (feels like it's "working").                    |
| `find "$dir" -type f -perm -o=w 2>/dev/null`  | Find all files in the directory that anyone can write to, this was covered in module 2. (security risk). |
| `sleep 1`             | Short pause before continuing to the next directory.                |
| `done`                       | Ends the loop.                                                      |

#### Step 8

For loop scanning log files:
```bash
for logfile in /var/log/*.log; do
    echo "Checking $logfile..."
    grep "error" "$logfile"
done
```
| Code                                  | What It's Doing                                                             |
|---------------------------------------|------------------------------------------------------------------------------|
| `for logfile in /var/log/*.log; do`   | Loop through every `.log` file in `/var/log` using wildcards                                |
| `echo "Checking $logfile..."`         | Print the name of the file being searched using substitution                                   |
| `grep "error" "$logfile"`             | Search the file for the word "error" and print any matching lines           |
| `done`                                | Ends the loop                                                               |

#### Step 9

Print message to signal completion:
```bash
echo "Scan Complete!"
```
- Only shows if root because it is within the if statement.

#### Step 10

In case of not root, show warning:
```bash
else
    echo ""
    echo "You are not the root user, so you cannot use this script"
fi
```
- If the user is not root, we tell them they dont have permission to run the scan.

#### Step 11

Signal Completion:
```bash
echo "Done"
```
No matter who runs this they will be notified that the script is done.

---

Feel free to save it and run it. Admire your hard work. Expect errors, for troubleshooting, check out the [errors](errors.md) file.

All the best.


