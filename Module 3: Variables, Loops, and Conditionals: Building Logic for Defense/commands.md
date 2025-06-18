# 📖 List of Commands used in Module 3: Variables, Loops, and Conditionals: Building Logic for Defense

## Shebang 🐚🚀

Explanation: A shebang `#!` tells the system which interpreter to use to run a script. By writing `#!/bin/bash`, you're telling the system to use the Bash shell (the location is '/bin/bash') to interpret and execute the script's commands. It's the first line in many shell scripts and ensures the correct program runs the script. We use it for this course because it is a Bash course after all.

Example:
```bash
#!/bin/bash

echo "Hello!"
```
The example tells the system to interpret the script using the Bash shell. This means the next command, `echo "Hello!"` will be ran using the Bash shell. You write this as the first thing you write when writing a script that you want to be ran with the Bash shell.

## Variables 📦🔢

Explanation: A variable in Bash stores data (like text, numbers, or command outputs) that can be used and changed later in the script.

Example:
```bash
#!/bin/bash

NUMBER=3

echo "Your number is, $NUMBER"
```
This example stores the number 3 under the variable 'NUMBER' where it can be re-used throughout the script. Lets move onto storing commands:
```bash
#!/bin/bash

USERNAME=$(whoami)

echo "Your username is, $USERNAME
```
This example stores the text output of the `whoami` command under the variable 'USERNAME'. The command must be encapsulated like so: `$(<command>)`.

#### Note: Variables names are case sensitive and there must not be any spaces when writing them out due to the syntax rules of Bash.

## If Statements ➡️✅

Explanation: An if statement in Bash lets you run commands if a certain condition is true.

Example:
```bash
#!/bin/bash

# Admin Validator

USERNAME=$(whoami)

if [ "$USERNAME" = "root" ]; then
  echo "You are the admin"
else
  echo "You are not the admin"
fi
```
`#Admin Validator`
- This is a comment, it does not actually do anything to the code, it is just there for clarity sake. If you put a `#` then you may write freely after it.

`USERNAME=$(whoami)`
- Runs `whoami` to get the current username and stores it in USERNAME as previosuly mentioned.

`if [ "$USERNAME" = "root" ]; then`
- Checks if the username is exactly "root". If true, proceeds to next line. You must put spaces between the square brackets and the text otherwise it will not work due to the syntax and you must encase the conditional expression in quotations (`"$USERNAME" = "root"`).

`echo "You are the admin"`
- Prints this message if the user is root.

`else`
- Defines the block to run if the condition is false.

`echo "You are not the admin"`
- Prints this if the user is not root.

`fi`
- Ends the if block. This must be put to tell the system that we are done.

#### Note: If statements work by saying `if [ x ]; then` and then end them with `fi`. For loops use `for x in y; do` and they end in `done`.

## For Loops 🔄➡️📋

Explanation: A `for` loop in Bash repeats commands for each item in a list.

Example:
```bash
#!/bin/bash

for x in 1 2 3 4 5; do
  echo "Number: $x"
done
```
This will print:

Number:1

Number:2

Number:3

Number:4

Number:5

We can use a brace expansion to make this easier or if we had a higher count of numbers, as such:
```bash
#!/bin/bash

for x in {1..30}; do
  echo "Number: $x"
done
```
This will repeat the process for all numbers up to 30.

Here is an actual use case for the for loop:

```bash
#!/bin/bash

for file in /var/log/*.log; do
  echo "Scanning: $file"
  grep "error" "$file"
done
```
This is a simple loop that scans log files for the word 'error'.
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

## The Scanner 🔍📥

The scanner at the end of the video looks like this:
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
Here is a line-by-line breakdown of what is happening:
```bash
1. #!/bin/bash
```
- Tells the system to run the script using Bash.
```bash
2. USERNAME=$(whoami)
```
- Gets the current user's name and stores it in USERNAME.
```bash
3. DATE=$(date +%F)
```
- Gets today’s date in YYYY-MM-DD format and stores it in DATE.
```bash
4. echo "Welcome, $USERNAME. Today's date is $DATE"
```
- Greets the user and shows the date.
```bash
5. sleep 1
```
- Pauses for 1 second. Looks cool.
```bash
6. echo "Initialising..."
```
- Prints an initialisation message.
```bash
7. sleep 3
```
- Waits 3 seconds.
```bash
8. if [ "$USERNAME" = "root" ]; then
```
- Checks if the script is run by the root user.
```bash
9. echo "Root User Detected"
```
- If root, print confirmation.
```bash
10. sleep 2
```
- Pauses for 2 seconds.
```bash
11. for dir in /home /tmp /var; do
```
- Loops through the listed directories.
```bash
12. echo ""
```
- Prints a blank line.
```bash
13. echo "Scanning $dir for world-writable files"
```
- Notifies which directory is being scanned.
```bash
14. sleep 2
```
- Waits 2 seconds.
```bash
15. find "$dir" -type f -perm -o=w 2>/dev/null
```
- Finds files in the directory that are world-writable (anyone can write), hides errors.
```bash
16. sleep 1
```
- Pauses for 1 second.
```bash
17. done
```
- Ends directory scanning loop.
```bash
18. for logfile in /var/log/*.log; do
```
- Loops through .log files in /var/log.
```bash
19. echo "Checking $logfile..."
```
- Prints which log file is being checked.
```bash
20. grep "error" "$logfile"
```
- Searches for the word "error" in the current log file.
```bash
21. done
```
- Ends the log file loop.
```bash
22. echo "Scan Complete!"
```
- Prints completion message.
```bash
23. else
```
- If not root:
```bash
24. echo ""
```
- Prints a blank line.
```bash
25. echo "You are not the root user, so you cannot use this script"
```
- Warns the user they don’t have permission.
```bash
26. fi
```
- Ends the if block.
```bash
27. echo "Done"
```
- Final message, script ends.
