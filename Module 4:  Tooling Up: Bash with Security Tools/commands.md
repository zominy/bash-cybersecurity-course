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
`RESULT=`: This assigns the output of the command on the right to the variable named RESULT. This is a shell variable. Once the terminal is closed it is erased.

`$( ... )`: Command substitution — runs the command inside and captures its output as a string.

`nmap`: The network scanning tool.

`-p 22,80,443`: Scan only specific ports — 22 (SSH), 80 (HTTP), and 443 (HTTPS).

`localhost`: Scan your local machine (IP 127.0.0.1). I am only scanning myself as I am allowed. Scanning other devices I have no permission to scan is illegal and unethical.

---

Example 3 (Parsing):
```bash
echo "$RESULT" | grep "open"
```
`echo "$RESULT"`: Prints the content of the variable RESULT. The quotes preserve line breaks and spacing.

`|`: Pipe — sends the output of the command on the left as input to the command on the right.

`grep "open"`: Searches the input for lines containing the word "open" and prints those lines. This is parsing, parsing means extracting or processing specific pieces of information from raw data or text. In this case we are extracting open ports from an nmap scan.
