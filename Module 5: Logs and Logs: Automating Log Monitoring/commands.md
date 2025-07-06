# 📖 List of Commands used in Module 5: Logs and Logs: Automating Log Monitoring

---

## sudo journalctl -n 50 📘🔎

Explanation: This command will display the last 50 lines of system activity.

- `sudo`: 'Superuser do' runs the command with admin privileges.
- `journalctl`: This is a command line tool used to query and display logs from the systemd journal (this is the central logging system on most modern Linux systems, including Debian).
- `-n`: This flag stands for 'number'. It tells journalctl how many recent lines of logs you want to see.
- `50`: Specifies you want to see only the last 50 lines of the system journal.

For example:
```bash
max@debian:~$ sudo journalctl -n 5
[sudo] password for max: 
Jun 28 19:55:43 debian NetworkManager[592]: <info>  [1751136943.3480] dhcp4 (en>
Jun 28 19:56:46 debian gdm-password][3348]: gkr-pam: unlocked login keyring
Jun 28 19:56:46 debian NetworkManager[592]: <info>  [1751137006.7522] agent-man>
Jun 28 19:56:53 debian sudo[3366]:      max : TTY=pts/0 ; PWD=/home/max ; USER=>
Jun 28 19:56:53 debian sudo[3366]: pam_unix(sudo:session): session opened for u>
lines 1-5/5 (END)
max@debian:~$ 
```
---

Let's focus on the last two lines and try to understand what this means. The 4th line says:
```bash
Jun 28 19:56:53 debian sudo[3366]:      max : TTY=pts/0 ; PWD=/home/max ; USER=root ; COMMAND=/usr/bin/journalctl -n 5
```
Service: `sudo[3366]` (user ran a sudo command)

User: `max`

TTY: pts/0 (the virtual session (or terminal) they used)

PWD: `/home/max` (working directory)

USER: root

COMMAND: States the exact command used and where on.

This is very important. It shows someone used sudo for something, a command often logged and monitored because its associated with privilege escalation.

---

Line 5 says:
```bash
Jun 28 19:56:53 debian sudo[3366]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
```
Logs like this confirm privilege escalation was granted and when it happened. It is important to track admin access among systems.

---

## sudo journalctl -f 📘📡

Explanation: This shows the system logs as they are with real-time updates. So when things are happening they are immediately logged and displayed infront of you.

The `-f` flag stands for follow. It works just like `tail -f` as it continuously prints new log entries as they are written in the journal.

If you were to run this and start clicking around, you will see new entries appear.

---

## sudo journalctl | grep "Failed password" 📘❌🔑

Explanation: Filters journal entries to show only lines containing “Failed password,” indicating failed SSH login attempts. Essentially scans the entire log and returns lines containing "Failed Password".

#### Note: We are already familiar with this concept as covered in [Module 3](https://github.com/zominy/bash-cybersecurity-course/tree/main/Module%203%3A%20Variables%2C%20Loops%2C%20and%20Conditionals%3A%20Building%20Logic%20for%20Defense) and [Module 4](https://github.com/zominy/bash-cybersecurity-course/tree/main/Module%204%3A%20%20Tooling%20Up%3A%20Bash%20with%20Security%20Tools)

---

## sudo journalctl -n 50 > journal.txt 📘📄➡️📝

Explanation: Takes the last 50 lines of the logs and puts it into a new file called `journal.txt`. This has been covered in [Module 1](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%201%3A%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/commands.md).

---

## cat 🐱📄

Explanation: `cat` prints the full contents of a file.

Example:
```bash
maxz@zom:~$ sudo journalctl -n 3 > newjournal.txt; cat newjournal.txt
Jun 29 01:30:05 zom systemd[1]: packagekit.service: Consumed 16.867s CPU time.
Jun 29 01:31:30 zom sudo[2838]:     maxz : TTY=pts/0 ; PWD=/home/maxz ; USER=root ; COMMAND=/usr/bin/journalctl -n 3
Jun 29 01:31:30 zom sudo[2838]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
maxz@zom:~$ 
```
So what this means is, to first take the last 3 lines of the logs and put them in a new file called `newjournal.txt` then `cat newjournal.txt` lists all the contents of this file.

---

## less 🔍📄

Explanation: Allows you to scroll up and down through large text files one screen at a time.

Example:
```bash
less newjournal.txt
```
Press Ctrl + C to exit.

---

## vi 📝⌨️

Explanation: It is a lightweight yet powerful text editor that runs in the terminal. So far we have used `nano` due to it being much more user friendly but `vi` is also an option.

Basically, just write `vi <target file>` to open the editor for that file.

Here’s a quick cheat sheet:

- Use arrow keys for navigation
- Press i to start editing (insert mode).
- Press Esc to leave insert mode.
- Type :w to save.
- Type :q to quit.
- Type :wq to save and quit.
- Type :q! to quit without saving.

---

## The Log Scanner 🔍📜🗂️

Below is the scanner:
```bash
#!/bin/bash

OUTPUT_DIR="$HOME/log_scans"
TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")
OUTPUT_FILE="$OUTPUT_DIR/failed_logins_$TIMESTAMP.txt"

mkdir -p "$OUTPUT_DIR"

echo "Scanning journal logs for failed login attempts..."

sudo journalctl | grep "Failed password" > "$OUTPUT_FILE"

ATTEMPT_COUNT=$(wc -l < "$OUTPUT_FILE")

echo "Scan complete."
echo "Found $ATTEMPT_COUNT failed login attempts."
echo "Results saved to $OUTPUT_FILE"
```
Explanation part-by-part:

- `#!/bin/bash` - We all know this is the shebang. Tells the system to use `/bin/bash` to run the script.

- `OUTPUT_DIR="$HOME/log_scans"` - Defines the variable `OUTPUT_DIR`. $HOME is an environment variable that expands to the current user home directory. We set the folder where our results will be stored (bare in mind it is not actually made yet). Using a dedicated directory helps keep output organised and separate from other files plus the home directory is getting kinda crowded now.

- `TIMESTAMP=$(date +"%Y-%m-%d_%H-%M-%S")` Defines `TIMESTAMP`. The + format specifies how the date/time will appear, where in this case it will look like `2025-06-26_15-20-44`.
  - `%Y` - Year
  - `%m` - Month (lower case m)
  - `%d` - Day
  - `%H` - Hour
  - `%M` - Minute (upper M)
  - `%S` - Second
  - `-` & `_` - Seperators so it is not just a bunch of numbers
  
This allows unique, timestamped filenames to avoid overwriting (using the same file name and messing stuff up).

- `OUTPUT_FILE="$OUTPUT_DIR/failed_logins_$TIMESTAMP.txt"` - Constructs the full path to the output file using the directory and timestamp. Example: `/home/maxz/log_scans/failed_logins_2025-06-26_15-20-44.txt`.

- `mkdir -p "$OUTPUT_DIR"` - As I stated earlier, now the `mkdir -p` creates the directory that contains the scans as needed. If it already exists, it is not overwritten. This was covered in [Module 2](https://github.com/zominy/bash-cybersecurity-course/tree/main/Module%202%3A%20Navigating%20Like%20a%20Pro%20-%20Filesystems%20%26%20Permissions)

- `echo "Scanning journal logs for failed login attempts..."` - Prints a message to let the user know it has started.

- `sudo journalctl | grep "Failed password" > "$OUTPUT_FILE"`:
  
  - Runs `journalctl` to dump all logs

  - Pipes `|` the output to grep which filters only lines containing "Failed password"

  - The `>` redirects the output of grep into the specified output file
 
- `ATTEMPT_COUNT=$(wc -l < "$OUTPUT_FILE")`:
  - Runs `wc -l` on the output file to count the number of lines (here each line corresponds to a failed login attempt)

  - The `<` syntax redirects the file content as input to `wc -l` (so basically taking OUT of the output file in a way)

  - The count is assigned to the variable `ATTEMPT_COUNT`
 
---
 
```bash
echo "Scan complete."
echo "Found $ATTEMPT_COUNT failed login attempts."
echo "Results saved to $OUTPUT_FILE"
```
- Prints a small summary of the scan.

---

This lab was good fun, but tricky too.
