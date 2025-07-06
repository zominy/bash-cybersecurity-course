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
