# 📖 List of Commands used in Module 9: Crontab and Scheduling Tasks: Set It and Forget It

---

### Setup ⚙️

Due to the content of this module, you will be required to install a new service. Again, I would like to advise that if you are following along you use the same distro as me as covered in [Module 1](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%201%3A%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/notes.md) (look at the setup section).

Please run the following commands:
```bash
sudo apt update; sudo apt upgrade -y
```
- Updates the package list and upgrades all installed packages to the latest versions available from the updated list.

```bash
sudo apt install -y cron
```
- This command installs the `cron` package.

---

# sudo systemctl enable cron

Explanation: This command enables the cron service to automatically start every time the system boots.

Once you have ran this command, you will see confirmation as such:
```bash
maxz@zom:~$ sudo systemctl enable cron
Synchronizing state of cron.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable cron
maxz@zom:~$ 
```

---

# sudo systemctl start cron

Explanation: This command starts the cron service right now without needing to reboot.

---

# sudo systemctl status cron

Explanation: This command shows detailed information about the cron service, such as:
- Whether it's active (running), inactive, or failed.
- When it was last started.
- The PID of the service.
- Any recent logs or errors.

The output will look like this:
```bash
maxz@zom:~$ sudo systemctl status cron
● cron.service - Regular background program processing daemon
     Loaded: loaded (/lib/systemd/system/cron.service; enabled; preset: enabled)
     Active: active (running) since Fri 2025-07-25 18:02:09 BST; 4min 52s ago # This is the line we are focussing on since it tells us cron is active.
       Docs: man:cron(8)
   Main PID: 614 (cron)
      Tasks: 1 (limit: 2237)
     Memory: 436.0K
        CPU: 4ms
     CGroup: /system.slice/cron.service
             └─614 /usr/sbin/cron -f

Jul 25 18:02:09 zom systemd[1]: Started cron.service - Regular background program processing daemon.
Jul 25 18:02:09 zom cron[614]: (CRON) INFO (pidfile fd = 3)
Jul 25 18:02:09 zom cron[614]: (CRON) INFO (Running @reboot jobs)
```

---

# df -h > /home/$USER/disk_report.txt

The next step in the video is to create a script (this is the script we will be scheduling).

The script looks like this:
```bash
#!/bin/bash
df -h > /home/$USER/disk_report.txt
```
Note: When running the script and getting no output try using `/usr/bin/df -h`.

`#!/bin/bash`: This is the shebang. It tells the system to use the Bash shell to run the script.

`df -h`: `df` Displays disk space usage of file systems and `-h` shows human-readable format (e.g., shows sizes in MB, GB instead of blocks).

`> /home/$USER/disk_report.txt`:

- `>`: Redirects the output of df -h to a file (overwrites if it exists).

- `/home/$USER/disk_report.txt`: Saves the output in a file named disk_report.txt in the current user's home directory.

- `$USER`: Automatically replaced with the current username.

*Run the script to test it works.*

---

# crontab -e

Explantion: This command opens your personal crontab file (you will be able to choose either vim or nano, in the video I chose vim because we have done a lot of work with nano. However, feel free to choose whichever you prefer). You can add lines to define when and what command to run.

---

# 0 8 * * * /home/maxz/disk_script.sh

Here is how crontab reads this line:

```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of week (0 - 7) (Sunday = 0 or 7)
# │ │ │ │ │
# * * * * *  command_to_execute
```

Each asterisk corresponds to different times you want the script to run. The command, `0 8 * * * /home/maxz/disk_script.sh` means to run the script we just made every day at 8am.

In cron, an asterisk * means "every possible value" for that field. (Wildcard)

Field	Example	Meaning:
- * in Minute	*	Every minute (0–59)
- * in Hour	*	Every hour (0–23)
- * in Day	*	Every day of the month
- * in Month	*	Every month (1–12)
- * in Weekday	*	Every day of the week (0–7)

For example:
```bash
* * * * * echo "Hi"
```
This runs every minute of every hour of every day.

So:
`*` = “match everything” in that time unit.

---

#### How do we input this in vim?

Open the crontab editor:
```bash
crontab -e
```
If it opens in vim, you’ll be in normal mode (not able to type yet).

Press `i` to enter insert mode (you’ll now be able to type and you will see at the bottom `-- INSERT -- `).

Add this line at the bottom:
```bash
0 8 * * * /home/<YOUR USERNAME>/disk_script.sh
```
Then press `Esc` to leave insert mode.

Type :wq and press Enter to write and quit vim.

You will see a new line saying: `crontab: installing new crontab` once saving.

---

# crontab -l

Explanation: This command lists all current cron jobs for the current user. An example output would look like this:

```bash
maxz@zom:~$ crontab -l
# Edit this file to introduce tasks to be run by cron.
# 
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
# 
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
# 
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
# 
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
# 
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
# 
# For more information see the manual pages of crontab(5) and cron(8)
# 
# m h  dom mon dow   command
0 8 * * * /home/maxz/disk_script.sh
```

Nice and simple.
