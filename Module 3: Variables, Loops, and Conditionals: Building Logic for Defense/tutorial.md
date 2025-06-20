# Module 3: Variables, Loops, and Conditionals - Building Logic for Defense 🔁🛡️

## Introduction

In this tutorial I go over the basics of bash scripting; including variables, if statements, for loops, and touching on some new directories such as /tmp and /var/log.

## Aims 🎯
- To be able to use variables freely
- To be able to use if statements and for loops efficiently
- Nest loops
- Create a simple log scanner

#### Note: Please check out the [command file](commands.md) or [errors file](errors.md) for help if you are struggling to understand any concepts.

---

## Module 3 Lab 🛡️💻

### Part 1 - Creating a script file 📄⚙️

In the beginning of the lab we use the commmand:
```bash
touch s1.sh; chmod 700 s1.sh; nano s1.sh
```
This creates a new empty file called `s1.sh` then gives us executable privileges (`chmod 700`) and lastly opens the text editor (`nano s1.sh`).

When creating script files it is best practice to make them end in `.sh`. Also we need executable rights so that we can actually run these files.

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

Lets talk about `USERNAME=$(whoami)`. This example stores the text output of the whoami command under the variable 'USERNAME'. The command must be encapsulated like so: $(<command>). So it will run the whoami command in the terminal and just hold the result. If your username is 'david' then it will store the name 'david'.

It is the same for the date. It stores the date in YYYY-MM-DD format. Lets actually do something with these new variables. Write:























## appendix temp

s1.sh
```bash
#!/bin/bash
USERNAME=$(whoami)
DATE=$(date +%F)

if [ "$USERNAME" = "root" ]; then
        echo "You are the root user"
else
        echo "You are not the root user you scumbag"
fi
```

s2.sh
```bash
#!/bin/bash

for int in {1..25}; do
        echo "Number: $int"
done
```

```bash
#!/bin/bash

for int in 1 2 3 4 5; do
        echo "Number: $int"
done
```

s3.sh
```bash
#!/bin/bash

for file in /var/log/*.log; do
        echo "Scanning: $file"
        grep "error" "$file"
done
```


s4.sh
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
