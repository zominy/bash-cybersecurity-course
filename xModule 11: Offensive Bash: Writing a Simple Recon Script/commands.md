# 📖 List of Commands used in Module 11: Offensive Bash: Writing a Simple Recon Script

This script is a foundational system reconnaissance tool, a collection of basic commands bundled together to quickly gather essential information about a Linux system. It’s not meant to be flashy or overly complex, but rather a starting point or a baseline that can be expanded and customised depending on the environment or objective.

The goal here wasn’t to build something groundbreaking, but to demonstrate an understanding of:

- Core system inspection commands
- Simple Bash scripting structure
- The ability to automate repetitive tasks cleanly

Scripts like this form the backbone of many larger post-exploitation or system auditing tools. From here, you can easily expand it to:

- Output to files
- Run conditionally based on privilege level
- Parse or upload results
- Or tie into enumeration frameworks

##### 📘 Bottom line: This is intentionally simple, but purposefully so. It shows you know what you’re looking for and can translate that into something repeatable and extensible.

---

## Setup ⚙️

This script uses only basic, built-in Linux commands; so it should work straight  on nearly any Debian-based distribution.

However, to ensure full functionality (specifically for the open ports section), you’ll want to make sure nmap is installed:

```bash
sudo apt update; sudo apt install -y nmap
```
If nmap isn’t installed, the script will notify you and suggest how to install it.

---

# Full Recon Script 📜🔍

```bash
#!/bin/bash

echo "==== SYSTEM INFORMATION ===="
echo "Hostname: $(hostname)"
echo "OS: $(uname -o)"
echo "Kernel: $(uname -r)"
echo "Uptime: $(uname -p)"

echo -e "\n==== USERS ===="
echo "Current User: $(whoami)"
echo "All Users:"
cut -d: -f1 /etc/passwd

echo -e "\nLast Logged in Users"
last -a | head -n 5

echo -e "\n==== NETWORK INFORMATION ===="
ip a | grep inet

echo -e "\nDefault Gateway"
ip route | grep default

echo -e "\nDNS Servers"
cat /etc/resolv.conf | grep nameserver

echo -e "\n==== OPEN PORTS ===="
if command -v nmap >/dev/null 2>&1; then
        nmap -F localhost
else
        echo "Nmap not found: try sudo apt install nmap"
fi

echo -e "\n==== SERVICES ===="
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head

echo -e "\n==== INTERESTING FILES ===="
find /home -type f \( -name "*.conf" -o -name "*.log" -o -name "*.bak" \) 2>/dev/null | head -n 10

echo -e "\n==== DONE ===="
```
### Full Sectioned Breakdown:

- SYSTEM INFORMATION 💻🔎
```bash
echo "Hostname: $(hostname)"
echo "OS: $(uname -o)"
echo "Kernel: $(uname -r)"
echo "Uptime: $(uname -p)"
```
`hostname`: Shows the system's network name.

`uname -o`: Displays the operating system (e.g. `GNU/Linux`).

`uname -r`: Shows the kernel version.

`uname -p`: Time since last reboot.

- USERS 👥🔐
```bash
echo "Current User: $(whoami)"
echo "All Users:"
cut -d: -f1 /etc/passwd
```
`whoami`: Prints the current effective user.

`cut -d: -f1 /etc/passwd`: Lists all system users (one per line) by cutting out just the username field from /etc/passwd.

- Last Logged-in Users ⏳🧾
```bash
last -a | head -n 5
```
`last`: Shows login history.

`-a`: Appends the hostname where the session came from.

`head -n 5`: Limits output to the 5 most recent entries.

Useful for identifying recent interactive access to the system.

- NETWORK INFORMATION 🌐📡
```bash
ip a | grep inet
```
`ip a`: Lists all IP addresses and network interfaces.

`grep inet`: Filters for only IPv4/IPv6 address entries.

This tells you what IPs the machine currently has assigned.

- Default Gateway & DNS 🚪🧭
```bash
ip route | grep default
```
Shows the default route used to reach the internet.

```bash
cat /etc/resolv.conf | grep nameserver
```
Lists configured DNS servers for domain name resolution.

- OPEN PORTS 🔓📡
```bash
if command -v nmap >/dev/null 2>&1; then
    nmap -F localhost
else
    echo "Nmap not found: try sudo apt install nmap"
fi
```
Checks if nmap is installed using `command -v`.

If found, runs a fast scan (`-F`) against localhost.

If not found, prints an install suggestion.

This is helpful for seeing which services are listening locally / a quick recon of exposed attack surfaces.

- RUNNING SERVICES 🧠⚙️
```bash
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
```
`ps -eo`: Lists all processes with selected columns.

`--sort=-%mem`: Sorts by highest memory usage.

`head`: Only shows the top 10 entries.

A quick way to see what's running and which processes are hogging system resources.

- INTERESTING FILES 🗃️🧐
```bash
find /home -type f \( -name "*.conf" -o -name "*.log" -o -name "*.bak" \) 2>/dev/null | head -n 10
```
`find`: Searches for files under /home with common extensions used in config, logging, or backups.

`2>/dev/null`: Suppresses permission errors.

`head -n 10`: Just shows the first 10 results.

These files might could credentials, API keys, logs, or misconfigurations which is gold for a recon script.

- Finalising Message
```bash
echo -e "\n==== DONE ===="
```
Marks the end of the script output clearly.

Nice.
