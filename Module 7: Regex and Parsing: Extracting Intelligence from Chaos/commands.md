
# 📖 List of Commands used in Module 7: Regex and Parsing: Extracting Intelligence from Chaos

---

### Setup ⚙️

Due to the content of this module, you will most likely be required to install some new tools. Again, I would like to advise that if you are following along you use the same distro as me as covered in [Module 1](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%201%3A%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/notes.md) (look at the setup section).

Please run the following commands:
```bash
sudo apt update
```
- Updates the package list

```bash
sudo apt install -y grep awk sed coreutils
```
- This installs `grep`, `awk`, `sed`, `coreutils` (coreutils is just things like `cat`, `ls`, `cp`, `mv`, `rm`, and many others.): the tools we want for this module.

---

## sudo journalctl -u ssh.service | grep "Failed password" | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' 🛡️📜🔎

Explanation:

The first part of this command:
```bash
journalctl -u ssh.service
```
Shows logs related to the ssh.service unit. Basically, we are filtering the logs for the SSH entries.

`|`: pipes the output into the next command.

The second part of the command is one we are familiar with:
```bash
grep "Failed password"
```
We are parsing specifically the SSH entries for lines containing 'Failed password'. Basically filtering the log entries to only show lines that mention a failed login attempt (SSH authentication failures typically include this string).

The third part:
```bash
grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'
```
Uses grep with:

`-E`: Enables extended regular expressions (regex).

`-o`: Outputs only the matched parts of a line (not the full line). 

The regex `'([0-9]{1,3}\.){3}[0-9]{1,3}'`: Matches IPv4 addresses.

I do not advise that you try to memorise regexes. It is hard, and very unrealistic. Just google the one that you need. For example, google something like `regex for grepping ipv4 addresses`. Unless you have an exam for whatever reason that does not allow you to Google stuff and you need to remember it. Good luck to you guys.

For the regex: `'([0-9]{1,3}\.){3}[0-9]{1,3}'` to actually work. You need to generate IPv4 entries which you will see in the video.

Normally the logs will record the IPv6 address not the IPv4. You can use the regex: `'([0-9]{1,3}\.){3}[0-9]{1,3}|([a-fA-F0-9:]+:+)+[a-fA-F0-9]+'` to target IPv6 addresses if for some reason you cannot generate IPv4 addresses.

---

## sudo journalctl -u ssh.service | grep "Failed password" | awk '{print $(NF-5)}' 📜🧮🕵️

`NF`: Number of fields (words in the line).

`$(NF-5)`: The username is usually 5 fields before the end in a typical SSH failed login log entry. This line tells the system to count backwards from the end.

This begs the question, 'Why not use `awk '{print $9}'`? Because if we look at this line from the logs:

```bash
Jun 29 02:09:27 zom sshd[3242]: Failed password for maxz from ::1 port 45766 ssh2
```
The username `maxz` is the 9th field. So why don't we just print the 9th field all the time?

The answer is, logs are never the same length. The username is SOMETIMES the 9th field but not always. But the username is ALWAYS 5 fields from the end. So it makes sense to tell the system to print the field 5 from the end (always the username) rather than to print the 9th field of the line which could be the username but there is a good chance its not. Examine the line below:

```bash
Jul 09 12:22:29 zom sshd[3504]: Failed password for invalid user fake from 127.0.0.1 port 36698 ssh2
```
The username `fake` is now in the 11th field. So `awk '{print $9}'` would have printed `invalid`. However, the username is 5 fields from the end, as always. So `awk '{print $(NF-5)}'` would have printed `fake`, the username.

#### Note: A field in Bash is a piece of text separated by spaces or another delimiter. For example if you took the line:
```bash
Jul 09 12:22:29
```
#### There are three seperate fields, 'Jul', '09', and '12:22:29'

---

## sudo journalctl -u ssh.service | grep "Failed password" | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort | uniq -c | sort -nr 🛡️📊📈

Explanation:

- `sort`: Sort IPs alphabetically.
- `uniq -c`: Count how many times each appears.
- `sort -nr`: Sort by number, reverse (most common IPs first).

Example:
```bash
maxz@zom:~$ sudo journalctl -u ssh.service | grep "Failed password" | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort | uniq -c | sort -nr
[sudo] password for maxz: 
      3 127.0.0.1
```

---

## The Regex and Parsing Script

Here is what it looks like from the video:
```bash
#!/bin/bash
echo "Extracting IPs from failed SSH logins:"
journalctl -u ssh.service | grep "Failed password" | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort | uniq -c

echo "Extracting usernames:"
journalctl -u ssh.service | grep "Failed password" | awk '{print $(NF-5)}' | sort | uniq -c
```
`#!/bin/bash`: Shebang; tells the system to run this script using the Bash shell.

---
```bash
journalctl -u ssh.service | grep "Failed password" | grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}' | sort | uniq -c
```
This pipeline extracts and counts IP addresses from failed SSH login attempts:

`journalctl -u ssh.service`: Show logs for the SSH service

`grep "Failed password"`: Filter only lines with failed login attempts

`grep -Eo '([0-9]{1,3}\.){3}[0-9]{1,3}'`: Extract the IPv4 addresses using regex

`sort`: Sort the IPs so identical ones are grouped together

`uniq -c`: Count how many times each IP appears

---
```bash
journalctl -u ssh.service | grep "Failed password" | awk '{print $(NF-5)}' | sort | uniq -c
```
This pipeline extracts and counts usernames:

Same first two commands as above

`awk '{print $(NF-5)}'`: Print the username (5 fields from the end of the line).

`sort` and `uniq -c` → Same idea; group and count identical usernames

---

Nice.
