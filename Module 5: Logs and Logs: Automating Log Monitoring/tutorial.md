# Module 5: Logs and Logs: Automating Log Monitoring 🔍🤖

## Introduction

In this tutorial, I will be covering the Module 5 [video](https://www.youtube.com/watch?v=RYclAh6ZAX4&list=PLGPhFvIx6g8hdTP3fj2GV7qeISOUjYknL). In this video we cover logs, and how to automate log monitoring. Some new commands are covered, and we develop a new script.

## Aims 🎯
- Understand where and how system logs are stored.
- Use tools like `grep`, `tail`, and `journalctl` to read logs.
- Identify failed login attempts in logs.
- Write a basic bash script to automate log scanning.

---

## Module 5 Lab 📦

---

## Part 1 - Intro to Logs 🗃️✨📘

The first command used in the video is:
```bash
sudo journalctl -n 50
```
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
Logs allow you to see all system activity. The logs shown by `sudo journalctl -n 50` are entries from the systemd journal, which records messages from:

- The Linux kernel

- System services and daemons

- Applications running on the system

- User events (if configured)

These logs include things like:

- Boot messages

- Service start/stop info

- Errors and warnings

- Hardware events

- Authentication attempts

They help admins track system health, troubleshoot issues, and audit activity. A use case for this command could be after a sudden server reboot, run `sudo journalctl -n 50` to see recent error messages and find out what caused the crash.

---

#### But what use is being able to see the logs if you cannot understand them?

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

The next command used is:
```bash
sudo journalctl -f
```
Explanation: This shows the system logs as they are with real-time updates. So when things are happening they are immediately logged and displayed infront of you.

The `-f` flag stands for follow. It works just like `tail -f` as it continuously prints new log entries as they are written in the journal.

If you were to run this and start clicking around, you will see new entries appear.

You can see this clearly in the video when I open a new terminal and you see the logs update live with this new event. I also run the command `sudo echo "Hello"` in the new terminal and it is also recorded.

---

The next command entered is:
```bash
sudo journalctl | grep "Failed password"
```
Explanation: Filters journal entries to show only lines containing “Failed password,” indicating failed SSH login attempts. Essentially scans the entire log and returns lines containing "Failed Password".

Note: We are already familiar with this concept as covered in [Module 3](https://github.com/zominy/bash-cybersecurity-course/tree/main/Module%203%3A%20Variables%2C%20Loops%2C%20and%20Conditionals%3A%20Building%20Logic%20for%20Defense) and [Module 4](https://github.com/zominy/bash-cybersecurity-course/tree/main/Module%204%3A%20%20Tooling%20Up%3A%20Bash%20with%20Security%20Tools)

We follow this up by running:
```bash
sudo journalctl -n 10 > newjournal.txt
```
Explanation: Takes the last 50 lines of the logs and puts it into a new file called `journal.txt`. This has been covered in [Module 1](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%201%3A%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/commands.md). Then we run `ls` to verify the file has been created.

We then use the `cat` command on this new file:
```bash
cat newjournal.txt
```
Explanation: `cat` prints the full contents of a file.

Here is the contents from the video:
```bash
max@maxz:~$ cat newjournal.txt
Jun 29 12:30:27 maxz systemd[1]: Started anacron.service - Run anacron jobs.
Jun 29 12:30:27 maxz anacron[2825]: Normal exit (0 jobs run)
Jun 29 12:30:27 maxz systemd[1]: anacron.service: Deactivated successfully.
Jun 29 12:30:51 maxz sudo[2826]:      max : TTY=pts/0 ; PWD=/home/max ; USER=root ; COMMAND=/usr/bin/journalctl
Jun 29 12:30:51 maxz sudo[2826]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
Jun 29 12:30:51 maxz sudo[2826]: pam_unix(sudo:session): session closed for user root
Jun 29 12:31:15 maxz gnome-shell[1579]: Window manager warning: last_user_time (1140322) is greater than comparison timestamp (1140321).  This most likely represents a buggy client sending inaccurate timestamps in messages such as _NET_ACTIVE_WINDOW.  Trying to work around...
Jun 29 12:31:15 maxz gnome-shell[1579]: Window manager warning: W4 appears to be one of the offending windows with a timestamp of 1140322.  Working around...
Jun 29 12:31:48 maxz sudo[2830]:      max : TTY=pts/0 ; PWD=/home/max ; USER=root ; COMMAND=/usr/bin/journalctl -n 10
Jun 29 12:31:48 maxz sudo[2830]: pam_unix(sudo:session): session opened for user root(uid=0) by (uid=1000)
```
So what this means is, to first take the last 3 lines of the logs and put them in a new file called `newjournal.txt` then `cat newjournal.txt` lists all the contents of this file.


