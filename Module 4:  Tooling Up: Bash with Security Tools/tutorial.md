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

As we know, the output of `echo "$RESULT"` displays the output of `nmap -p 22,80,443 <target>`. We can use the cut command to extract specific columns or feilds like so:
```bash
echo "$RESULT" | cut -d "/" -f 1
```
`echo "$RESULT"`: Prints the contents of the RESULT variable (the nmap output from earlier).

`|`: Sends that output to the next command (cut).

`cut -d "/" -f 1`:

`-d "/"`: Use the slash / as the delimiter (what to split on).

`-f 1`: Extract the first field (everything before the first / on each line as specified).

So if you had an output from `nmap -p 80 localhost` where the output looked like this `80/tcp  closed  http`. You could then run:

`nmap -p 80 localhost | cut -d "/" -f 1` to get just `80`.

Let's try:
```bash
max@debian:~$ echo "$RESULT" | cut -d "/" -f 1
```
And you will see an output looking like this:
```
Starting Nmap 7.93 ( https:
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00022s latency).
Other addresses for localhost (not scanned): ::1

PORT    STATE  SERVICE
22
80
443

Nmap done: 1 IP address (1 host up) scanned in 0.03 seconds
```
You can see we have extracted only the port numbers.

---

We can also use `awk` to print specific feilds of an output.

Example from video:
```bash
echo "$RESULT" | awk '{print $1}'
```
This means to print the first feild and would print:
```
Starting
Nmap
Host
Other

PORT
22/tcp
80/tcp
443/tcp

Nmap
```
If we want to more than one field we can run (from video):
```bash
echo "$RESULT" | awk '{print $1, $2}'
```
This will print feilds 1 and 2:
```
Starting Nmap
Nmap scan
Host is
Other addresses
 
PORT STATE
22/tcp closed
80/tcp closed
443/tcp closed
 
Nmap done:
```

---

We then use `whois` as it looks up registration details about a domain name or IP address.

Example (from video):
```bash
whois example.com
```
You will see something like:
```
Domain Name: EXAMPLE.COM
   Registry Domain ID: 2336799_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.iana.org
   Registrar URL: http://res-dom.iana.org
   Updated Date: 2024-08-14T07:01:34Z
   Creation Date: 1995-08-14T04:00:00Z
   Registry Expiry Date: 2025-08-13T04:00:00Z
   Registrar: RESERVED-Internet Assigned Numbers Authority
   Registrar IANA ID: 376
   Registrar Abuse Contact Email:
   Registrar Abuse Contact Phone:
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Name Server: A.IANA-SERVERS.NET
   Name Server: B.IANA-SERVERS.NET
   DNSSEC: signedDelegation
   DNSSEC DS Data: 370 13 2 BE74359954660069D5C63D200C39F5603827D7DD02B56F120EE9F3A86764247C
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2025-06-25T15:42:27Z <<<
```
This is useful for:

- Checking domain ownership

- Seeing when a domain expires

- Investigating suspicious domains

---

The final topic covered is a command called `netstat`. In the video we run the command:
```bash
netstat -tuln
```
`netstat` - Shows network connections, routing tables, interface stats, etc.

`-t` - Show TCP connections.

`-u` - Show UDP connections

`-l` - Show only listening ports.

`-n` - Show numeric addresses and ports. Basically, show 80 instead of HTTP.

The output will look like this:
```
max@debian:~$ netstat -tuln
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN     
tcp6       0      0 ::1:631                 :::*                    LISTEN     
udp        0      0 0.0.0.0:5353            0.0.0.0:*                          
udp        0      0 0.0.0.0:44544           0.0.0.0:*                          
udp6       0      0 :::5353                 :::*                               
udp6       0      0 :::55635                :::*                               
max@debian:~$ 
```
The `netstat -tuln` command displays all listening TCP and UDP ports on the system, showing which services are waiting for network connections. In this output as shown above, the system is running a local printing service on port 631 for both IPv4 (127.0.0.1) and IPv6 (::1), meaning its only accessible from the local machine. Look at the first line, that looks like this: `tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN   ` the numbers `127.0.0.1:631` means `IP 127.0.0.1` and the number after the colon is the port, in this case `631` (the local printing service). It’s also listening on UDP ports 5353, 44544, and 55635 with 5353 commonly used for multicast DNS (device discovery). These UDP ports are open on all interfaces (0.0.0.0 and :::), but since they use UDP (a connectionless protocol), they dontt show a state like TCP does.
