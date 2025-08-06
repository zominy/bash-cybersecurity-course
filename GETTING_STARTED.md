# 🚀 Getting Started: Bash for Cybersecurity

Welcome to the **Bash for Cybersecurity** course! If you're new to Linux, the terminal, or cybersecurity tooling, this guide is for you.

By the end of this setup, you'll have a working Debian-based virtual machine with everything you need to follow along with the course — safely and reliably.

---

## 🖥️ Your Learning Environment

To follow this course, you will use a **virtual machine (VM)** running **GNU/Linux**.

This allows you to:
- Practice without affecting your main system
- Use real tools in a real environment
- Break things safely, and fix them again!

### 💡 Minimum Requirements

- A reasonably modern PC (8 GB RAM or more recommended)
- ~30 GB free disk space
- Internet access

---

## 🧰 What You Will Use

| Component      | Description                              |
|----------------|------------------------------------------|
| OS             | **Debian-based GNU/Linux distro** (e.g. Kali Linux, Ubuntu) |
| Kernel         | Target kernel: `6.1.0-37-amd64` (or higher) |
| Virtualization | **VMware Workstation Player** (preferred) or **VirtualBox** |
| Shell          | Bash (installed by default)              |
| Tools          | Preinstalled with most Debian distros    |

---

## 📦 Step 1: Install a Virtual Machine

### 🥇 Option 1: VMware Workstation Player/Pro (Recommended)

1. Download from: [https://www.vmware.com/products/workstation-player.html](https://www.vmware.com/products/workstation-player.html)
2. Install using default settings
3. No license key required for personal/non-commercial use (free).
4. For additional help installing, check this out: [Watch](https://www.youtube.com/watch?v=eJHtr1QlDeo)

### 🥈 Option 2: VirtualBox (Alternative, Free & Open Source)

1. Download from: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. Install VirtualBox and VirtualBox Extension Pack (for USB and network features)

---

## 🧾 Step 2: Download a Debian-Based Linux Distro

This course is designed to work best on **Debian-based distros**.

I recommend:

### ✅ Kali Linux (Best for Cybersecurity Focus)

- Comes preinstalled with security tools
- Great for later modules
- Download here: [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/)

### ✅ Ubuntu (Best for Beginners)

- Clean and user-friendly interface
- Easier if you’re brand new to Linux
- Download here: [https://ubuntu.com/download/desktop](https://ubuntu.com/download/desktop)

After download:
- Import the ISO into your virtual machine software
- Follow the on-screen install instructions (choose **default partitioning**, **ext4 filesystem**, and **standard user account**)

💡 Choose a username like `student` or `cyber` for easier command-line use.

---

## 🧪 Step 3: Confirm Your Environment Matches the Course

After booting into your VM, open the terminal and run:

```bash
uname -a
```
You should see something like:

```bash
Linux kali 6.1.0-37-amd64 #1 SMP Debian 6.1.20-2 (2023-06-01) x86_64 GNU/Linux
```
That confirms you’re on a compatible kernel.

## 🧰 Step 4: Install Basic Tools (if missing)

Most tools are already included on Kali or Ubuntu. If you ever need to manually install something:

```bash
sudo apt update && sudo apt install git curl wget net-tools
```
You’ll use these tools in later modules.

## 📁 Step 5: Create Your Working Directory

You don’t need to clone this GitHub repo. Instead, just follow along using the commands on GitHub and in the video.

To prepare your workspace:

```bash
mkdir ~/bash-cyber-labs
cd ~/bash-cyber-labs
```
Each module **may** ask you to create a subfolder, like:

```bash
mkdir lab1 && cd lab1
```

## 📚 Step 6: Read the Course Structure

Now that you're set up, go back to the main README and begin with Module 1.

Each module contains:

- commands.md – All commands + explanations
- errors.md – Common mistakes and fixes
- A video walkthrough (linked)

## 🛠️ Troubleshooting Tips
If the VM won’t start: Enable virtualisation in BIOS

If terminal commands don’t work: Copy them exactly, Linux is case-sensitive

If internet doesn’t work in the VM: Reboot the VM or check virtual network settings

## 👋 You're Ready

You now have a fully functional environment to follow the Bash for Cybersecurity course.

All the best!

—

Max Zominy

[LinkedIn](https://www.linkedin.com/in/max-zominy-85ba92310/)
