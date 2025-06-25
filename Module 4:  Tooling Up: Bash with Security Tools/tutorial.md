# Module 4: Tooling Up: Bash with Security Tools 🛠️🐚

## Introduction

In this tutorial, I go over some tools used in the cybersecurity space. We also build a scanner wrapper.

## Aims 🎯
- Understand how to use nmap, netstat, and whois for network scanning and information gathering.
- Automate common tasks by integrating these tools into scripts.
- Store the results of commands for later analysis or processing.
- Parse and extract relevant data from command output
- Develop a wrapper script

---

## Disclaimer ⚠️📢

You must have explicit permission to scan or access any area or data on this map. Unauthorised scanning or access is strictly prohibited and may be subject to legal action. Use this tool responsibly and within the bounds of the law.

---

## Module 4 Lab 📦

---

## Part 1 - Setup ⚙️🛠️

The video begins by first installing the requied tools for this lab. So, we run the command:
```bash
sudo apt update && sudo apt install -y nmap net-tools whois
```
Explanation: `sudo apt update` means to check for available updates and updates the package list as is necessary becuase we are installing some tools, (this is also being ran as the sudoer which is required to run this command).`sudo apt install -y` to install the the tools and the `-y` is to just say yes to any prompts during installation. Lastly, the `nmap net-tools whois` section is just the names of the tools we will be installing for this module.

## Part 2 - Intro to Scanning 🔍📡

After successfully installing the required tools. We then run the command:
```bash
nmap localhost
```
Explanation: `nmap` is the first tool used. It stands for 'network mapper' and is a tool that scans networks. It is commonly used to discover hosts and services on a network.

This command scans the local machine you are currently using, (localhost) IP address 127.0.0.1 (the loopback). So basically it scans your own computer (the local machine) for open ports and services. Works as `nmap <target>`.

Once this is ran, you will see an output that looks something like this:
```
Starting Nmap 7.93 ( https://nmap.org ) at 2025-06-24 21:52 BST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00010s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 999 closed tcp ports (conn-refused)
PORT    STATE SERVICE
631/tcp open  ipp

Nmap done: 1 IP address (1 host up) scanned in 0.14 seconds
```
This means that you're computer is online and reachable with very fast latency (this is because you are scanning locally). Out of 1000 commonly used ports that were scanner only port 631 is open.

Note: A host is a device connected to a network while a service is a program or process running on a host that provides some functionality over the network.

Note: A port is like a doorway into a computer that allows specific types of network communication to happen. Different ports allow for different things. For example, port 80 is HTTP (Hyperyext Transfer Protocol)

---

Next, we enter the command:
```bash
RESULT=$(nmap -p 22,80,443 localhost)
```
First of all, `nmap -p 22,80,443 localhost` means to scan specific ports. As you can see, I listed `nmap -p 22,80,443` bascially saying only scan port 22(SSH), 80(HTTP), 443(HTTPS) and the target is localhost.

Now let's talk about the other stuff:

`RESULT=`: This assigns the output of the command on the right to the variable named RESULT. This is a shell variable. Once the terminal is closed it is erased.

`$( ... )`: Command substitution - runs the command inside and captures its output as a string.

`nmap`: The network scanning tool as we know.

`-p 22,80,443`: Scan only specific ports - 22 (SSH), 80 (HTTP), and 443 (HTTPS) as specified.

`localhost`: Scan your local machine. I'd like to say again that I am scanning only my own machine in this lab because it is illegal and unethical to scan other devices without explicit permission.

If you are now to enter the command:
```bash
echo "$RESULT"
```
You will see an output such as this:
```
Starting Nmap 7.93 ( https://nmap.org ) at 2025-06-25 01:48 BST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00015s latency).
Other addresses for localhost (not scanned): ::1

PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
```
Now you can see it in action. The value of `nmap -p 22,80,443 localhost` has been stored under RESULT. We can re-use this as we wish throughout this session.

---

Let's now try parsing. Parsing in this example means reading the scan results and picking out just the information about certain ports.

In the video, we run this:
```bash
echo "$RESULT" | grep "closed"
```
`echo "$RESULT"`: Prints the content of the variable RESULT as seen previously. The quotes preserve line breaks and spacing.

`|`: Pipe - sends the output of the command on the left as input to the command on the right.

`grep "closed"`: Searches the input for lines containing the word "closed" and prints those lines. This is parsing, parsing means extracting or processing specific pieces of information from raw data or text. In this case we are extracting open ports from an nmap scan.

When this is ran, you will see an output that looks like this:
```
max@debian:~$ echo "$RESULT" | grep "closed"
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https
```
As can be seen, we are parsing the output of RESULT for only lines containing 'closed'. You would use this in cases where you are scanning multiple ports and want to only see certain ports. e.g. open ports only to see which you could exploit, closed ports only, certain protocols, port numbers, etc.

---

## Part 3 - The Scanner Wrapper 🌀🔄

In the video, we next create a new empty file, make it executable and open the text editor by using the command:
```bash
max@debian:~$ touch scanner.sh; chmod +x scanner.sh; nano scanner.sh
```
Then write this script:
```bash
#!/bin/bash

HOST=$1

if [ -z "$HOST" ]; then
	echo "Host Not Found"
	exit 1
fi

nmap -p 22,80,443 $HOST | grep "closed"
```
`#!/bin/bash` - Shebang. Tells the system to run the script using the Bash shell.

`HOST=$1` - Basically, you write `./scanner.sh <target>`. In our case it is `./scanner.sh localhost`, the 'localhost' is the first command-line argument. The `$1` means to assign the first command-line argument to HOST.

`if [ -z "$HOST" ]; then` - Checks if HOST is empty (no argument was passed), `-z` means 'zero length'.

```
echo "Host Not Found"
exit 1
```
If no host was provided, it prints an error message and exit with code 1 (this is to indicate failure).

`nmap -p 22,80,443 $HOST | grep "closed"` - Scans the specified host ($HOST) for ports 22(SSH), 80(HTTP), and 443(HTTPS). Pipes the output to grep "closed" to show only ports that are closed.

When you run it normally like this: `./scanner.sh` you will see `Host Not Found` due to there being no argument following the executions.

When you run it with a target like this: `./scanner.sh localhost` then you will get an output like this:
```
max@debian:~$ ./scanner.sh localhost
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https
```

---

## Part 4 - Cut, Awk, Whois, and Netstat 🌐📊



