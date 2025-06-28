# 📖 List of Commands used in Module 5: Logs and Logs: Automating Log Monitoring

### Introduction

This is gonna be a deep dive into a crucial aspect of cybersecurity: log monitoring and log analysis. The system logs are the digital fingerprints of everything that happens on a machine. Understanding this stuff is pretty important.

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
