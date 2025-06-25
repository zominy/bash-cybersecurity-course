# 📖 List of Commands used in Module 4: Tooling Up: Bash with Security Tools

### Disclaimer ⚠️📢

You must have explicit permission to scan or access any area or data on this map. Unauthorised scanning or access is strictly prohibited and may be subject to legal action. Use this tool responsibly and within the bounds of the law.

---

## sudo apt update && sudo apt install -y nmap net-tools whois ⚙️⬆️📡

Explanation: `sudo apt update` means to check for available updates and updates the package list as is necessary becuase we are installing some tools, (this is also being ran as the sudoer which is required to run this command) then `sudo apt install -y` to install the the tools and the `-y` is to just say yes to any prompts during installation. Lastly, the `nmap net-tools whois` section is just the names of the tools we will be installing for this module.

## nmap 👁️📡

Explanation: `nmap` is the first tool used. It stands for 'network mapper' and is a tool that scans networks. It is commonly used to discover hosts and services on a network.

A host is a device connected to a network while a service is a program or process running on a host that provides some functionality over the network.

Example:
```bash
nmap localhost
```
This command scans the local machine you are currently using, (localhost) IP address 127.0.0.1 (the loopback). So basically it scans your own computer (the local machine) for open ports and services. Works as `nmap <target>`.

---

Example 2:
```bash
RESULT=$(nmap -p 22,80,443 localhost)
```
The output of `nmap -p 22,80,443 localhost` is:
```
PORT    STATE  SERVICE
22/tcp  closed ssh
80/tcp  closed http
443/tcp closed https
```
This is stored in RESULT.

`RESULT=`: This assigns the output of the command on the right to the variable named RESULT. This is a shell variable. Once the terminal is closed it is erased.

`$( ... )`: Command substitution - runs the command inside and captures its output as a string.

`nmap`: The network scanning tool.

`-p 22,80,443`: Scan only specific ports - 22 (SSH), 80 (HTTP), and 443 (HTTPS).

`localhost`: Scan your local machine (IP 127.0.0.1). I am only scanning myself as I am allowed. Scanning other devices I have no permission to scan is illegal and unethical.

---

Example 3 (Parsing):
```bash
echo "$RESULT" | grep "closed"
```
`echo "$RESULT"`: Prints the content of the variable RESULT. The quotes preserve line breaks and spacing.

`|`: Pipe - sends the output of the command on the left as input to the command on the right.

`grep "closed"`: Searches the input for lines containing the word "closed" and prints those lines. This is parsing, parsing means extracting or processing specific pieces of information from raw data or text. In this case we are extracting open ports from an nmap scan.

---

## The Scanner Wrapper 📦🔍

In the video, the scanner looks like this:
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

---

## cut ✂️

Explanation: A tool used to extract specific columns or fields from text inputs.

Example:
```bash
echo "$RESULT" | cut -d "/" -f 1
```
`echo "$RESULT"`: Prints the contents of the RESULT variable (the nmap output from earlier).

`|`: Sends that output to the next command (cut).

`cut -d "/" -f 1`:

`-d "/"`: Use the slash / as the delimiter (what to split on).

`-f 1`: Extract the first field (everything before the first / on each line as specified).

So if you had an output from `nmap -p 80 localhost` that looked like this `80/tcp  closed  http`. You could then run:

`nmap -p 80 localhost | cut -d "/" -f 1` to get just `80`.

---

## awk 🧮🔤

Explanation: `awk` is a powerful text processing tools. It can be used to print specific feilds of an output.

Example:
```bash
echo "$RESULT" | awk '{print $1}'
```
This means to print the first feild and would print:
```
PORT
22/tcp
80/tcp
443/tcp
```
If we want to more than one field we can run:
```bash
echo "$RESULT" | awk '{print $1, $3}'
```
This will print feilds 1 and 3:
```
PORT    SERVICE
22/tcp  ssh
80/tcp  http
443/tcp https
```

---

## whois 🔍

Explanation: `whois` looks up registration details about a domain name or IP address.

Example:
```bash
whois google.com
```
You will see something like:
```
Domain Name: GOOGLE.COM
Registrar: MarkMonitor Inc.
Creation Date: 1997-09-15
Registry Expiry Date: 2028-09-14
Name Server: NS1.GOOGLE.CO
```
This is useful for:

- Checking domain ownership

- Seeing when a domain expires

- Investigating suspicious domains

---

## netstat -tuln 🌐📊

Explanation: `netstat` - Shows network connections, routing tables, interface stats, etc.

`-t` - Show TCP connections.

`-u` - Show UDP connections

`-l` - Show only listening ports.

`-n` - Show numeric addresses and ports. Basically, show 80 instead of HTTP.
