# 📖 List of Commands used in Module 6: Bash and Networking

### Disclaimer ⚠️📢

You must have explicit permission to scan or access any area or data on this map. Unauthorised scanning or access is strictly prohibited and may be subject to legal action. Use this tool responsibly and within the bounds of the law.

---

### Setup ⚙️

Due to the content of this module, you will most likely be required to install some new tools. Again, I would like to advise that if you are following along you use the same distro as me as covered in [Module 1](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%201%3A%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/notes.md) (look at the setup section).

Please run the following commands:
```bash
sudo apt update
```
- Updates the package list

```bash
sudo apt install curl netcat traceroute telnet -y
```
- This installs `curl`, `netcat`, `traceroute`, `telnet`: the tools we want for this module.

---

## ping -c 3 google.com 📡

Explanation:

- `ping`: This command sends ICMP (Internet Control Message Protocol) echo requests, which is a way to identify if a host is up and reachable. When sending these requests you will be fed back information such as:
    - `64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=14.2 ms` - Successful.
    - `Request timed out` - TTL (Time To Live: IPv4's mechanism to limits the lifetime of data) expired.
    - `Destination host unreachable` - ICMP might be blocked by a firewall or the host cannot be found.
  Please examine the image below to grasp how this works in a network:

![image](https://github.com/user-attachments/assets/f3487b02-6dcf-42b0-8e54-1e2856b509ee)

  If a ping fails, it is a good way of troubleshooting a network, it can provide the network technician/engineer with valuable information as to what the problem may be.

- `-c` - Caps the amount of packets at `x`.
- `3` - Specifies the amount of packets to send. Or else, it will run forever (Ctrl + C to end).
- `google.com` - The target domain. This can sometimes be an IP address instead if you are trying to ping for example another host on a LAN.

Example Output:
```
maxz@zom:~$ ping -c 3 google.com
PING google.com (172.217.169.46) 56(84) bytes of data.
64 bytes from lhr48s08-in-f14.1e100.net (172.217.169.46): icmp_seq=1 ttl=128 time=25.0 ms
64 bytes from lhr48s08-in-f14.1e100.net (172.217.169.46): icmp_seq=2 ttl=128 time=25.3 ms
64 bytes from lhr48s08-in-f14.1e100.net (172.217.169.46): icmp_seq=3 ttl=128 time=26.2 ms

--- google.com ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 24.988/25.494/26.175/0.499 ms
```

---

## curl -I https://example.com 🌐📥🧾

- `curl`: Transfers data from or to a URL (like a browser without GUI).
    - Note: A GUI is a graphical user interface, a type of user interface you can see and interact with graphically.
- `-I`: Fetch headers ONLY, not the full page body. If you did not use this `-I` it would return the HTML for that target.
- `https://example.com`: You probably already guessed this is the target (can be any).

Example Output:
```bash
maxz@zom:~$ curl -I https://example.com
HTTP/2 200 
content-type: text/html
etag: "84238dfc8092e5d9c0dac8ef93371a07:1736799080.121134"
last-modified: Mon, 13 Jan 2025 20:11:20 GMT
cache-control: max-age=379
date: Sun, 06 Jul 2025 20:14:23 GMT
alt-svc: h3=":443"; ma=93600,h3-29=":443"; ma=93600
```

So using `curl -I`, we can check if a web server is up, what HTTP version it uses, when the page was last changed, how long it is cached, and even whether it supports faster protocols like HTTP/3. All without downloading the full page.

---

## nc -zv localhost 22 80 443 🧪🔌📡

- `nc`: Shorthand for 'netcat'.
- `-z`: Stands for "Zero I/O mode"; just check if port is open, no data sent.
- `-v`: Verbose; show output details.
- `localhost`: Test your own machine (127.0.0.1) - loopback IP.
`22 80 443`: Scan these specific ports (written the same as the `nmap` command from [Module 4](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%204%3A%20%20Tooling%20Up%3A%20Bash%20with%20Security%20Tools/commands.md)):
    - 22: SSH
    - 80: HTTP
    - 443: HTTPS
 
Example Output:
```bash
maxz@zom:~$ nc -zv localhost 22 80 443
localhost [127.0.0.1] 22 (ssh) open
```
If you followed along with [Module 5](https://www.youtube.com/watch?v=RYclAh6ZAX4&t=993s) then port 22 will already be open. If not click the link and skip to (7:40).

Now, what does this output mean?

- `localhost [127.0.0.1]`: Netcat resolved localhost to the IP address 127.0.0.1.
- `22 (ssh)`: We asked it to check port 22 (SSH).
- `open`: Netcat successfully connected.

Notice how it did not mention anything about port 80, or 443. This is because they are closed. Netcat only prints output for ports that are open.

---

## nc -v localhost 22 🔍🖧🔐

Explanation:
- `nc`: Netcat.
- `-v`: Verbose mode; makes netcat print connection info, error messages, or service banners (if any).
- `localhost`: This is the target we are trying to connect to.
- `22`: TCP port number we are targeting. Port 22 is the default port for SSH.

Example Output:
```bash
maxz@zom:~$ nc -v localhost 22
localhost [127.0.0.1] 22 (ssh) open
SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u6
```
As we know already, `localhost [127.0.0.1] 22 (ssh) open` means that the connection was successful.

The next line, `SSH-2.0-OpenSSH_9.2p1 Debian-2+deb12u6` is called a banner. This is the banner sent by the SSH server. What does this mean?

- `SSH-2.0`: It speaks SSH version 2.
- `OpenSSH`: The software handling the connection is OpenSSH. We did this in Module 5.
- `9.2p1`: This is the version.
- `Debian-2+deb12u6`: This tells us it is a Debian machine.

---

## Multi IP Port-Scanner 🌐🛠️📊

First create a seperate file in the same directory and call it something like `ips.txt`. Then write the following IP addresses in:
```
127.0.0.1
8.8.8.8
1.1.1.1
```
- 127.0.0.1 - Loopback IP, or localhost IP. This is us.
- 8.8.8.8 - Google public DNS.
- 1.1.1.1 - Cloudflare public DNS.

We then create a new executable file, name it something relevant then write the following script:
```bash
#!/bin/bash
for ip in $(cat ips.txt); do
  nc -zv $ip 22 80 443 >> scan_results.txt
done
```
Explanation:
- `#!/bin/bash`: Shebang. Tells the system to run the script using Bash.

- `for ip in $(cat ips.txt); do`: This loops over each IP in the `ips.txt` file.

- `cat ips.txt`: Outputs the list of IPs (Module 5).

- `$(...)`: Command substitution as we know already. In this case it replaces with the output of cat instead of dumping it.

- `ip`: Variable holding the current IP in the loop

- `nc -zv $ip 22 80 443`: scan ports 22, 80, and 443 on the current IP

- `>> scan_results.txt`: Append the scan output to scan_results.txt. If you are wondering why there is two `>>` instead of one `>`. This is because the double greater-than symbol `>>` means:

Append the output to the file.

Whereas a single `>` means:

Overwrite the file with new output.

- `done`: Ends the for loop (Module 3).

---

## traceroute google.com 📍🗺️

Explanation: Shows each hop (router) between you and google.com, which helps diagnose network routing issues in a network.

Example Output:
```bash
maxz@zom:~$ traceroute google.com
traceroute to google.com (142.250.187.238), 30 hops max, 60 byte packets
 1  _gateway (192.168.226.2)  0.089 ms  0.049 ms  0.062 ms
 2  * * *
 3  * * *
 4  * * *
```
The first line shows the route and that we have successfully pinged the address.

The asterisks mean no response. Becuase the request has went through already, there is no more response.

Awesome.
