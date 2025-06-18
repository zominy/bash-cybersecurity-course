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


## Brace Expansions 🧱➕


## The Scanner 🔍📥
