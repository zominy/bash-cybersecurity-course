# 🛡️⚙️ Module 12: Defensive Bash: Build Your Own IDS Lite 🟥

This module brings together everything you've learned so far; parsing, logging, thresholds, and alerting to build a lightweight intrusion detection system (IDS) entirely in Bash.

It watches your system logs in real time and alerts you when patterns like brute-force login attempts are detected. The goal isn’t to replace tools like Fail2Ban or Snort, it’s to **build something simple, functional, and expandable** using fundamentals.

🎥 **Watch Module 12 Here**: [Watch](https://www.youtube.com/watch?v=St0tsvmYnaY&t=1693s)

---

## 🧭 Before You Start

Make sure you’ve:
- Read the [Getting Started guide](../GETTING_STARTED.md)
- Booted into a **Debian-based** Linux distro (e.g. Kali Linux or Ubuntu)
- Opened a terminal and are ready to start

🕒 **Estimated Effort:** ~1h-1h:30 minutes

---

## 🎯 Aims

- Detect suspicious behaviour
- Count failed login attempts from IPs over time
- Trigger alerts when thresholds are crossed
- Build and customise your own alerting workflow (email, terminal, webhook, etc.)
- Expand your Bash scripting with real-world defensive use cases

---

## 📚 Material for Module 12

1. [📖 Commands](./commands.md)  
   Explanations of the key parts of the script: how it detects brute force patterns, manages thresholds, and sends alerts.

2. [⚠ Possible errors and how to fix them](./errors.md)  
   A list of common issues you might face while testing or deploying the script.

3. 🤔For those who cannot get the `mail` section to work. Click [Here](./email_config.md)

---

Thanks for checking out my project, I sincerely hope you picked something up.
