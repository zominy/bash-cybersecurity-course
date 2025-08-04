# 📖 Commands | Module 12: Defensive Bash: Build Your Own IDS Lite (with journalctl)

This lab builds an IDS (Intrusion Detection System) using system logs streamed via `journalctl`. Below is a detailed breakdown of each key component, including how and why it works.

---

## 🛠️ General Setup

Before you start the IDS Lite lab, make sure your environment is ready with the following:

Update your system packages:

```bash
sudo apt update && sudo apt upgrade -y
```
Ensure you have Bash and core utilities installed:

```bash
bash --version
which journalctl
```
Both should be present by default on most modern Linux systems.

Verify you have access to journalctl:

This script uses journalctl to read logs in real-time from the systemd journal.

```bash
journalctl -u ssh.service -f
```
If this command outputs live SSH logs, you’re good to go.

(Optional) Install dependencies for enhanced alerts:

To send Slack or Webhook alerts, ensure curl is installed:

```bash
sudo apt install curl -y
```
To send email alerts from the command line, you’ll need an MTA (Mail Transfer Agent) like mailutils or sendmail. For example:

```bash
sudo apt install mailutils -y
```
Permissions:

Run the script with a user that has permission to read SSH logs via journalctl. If needed, you may run the script with sudo:

```bash
sudo ./your-ids-script.sh
```

---

## Full Script: `ids-lite.sh` 📸📣

```bash
#!/bin/bash

RED='\033[1;31m'
NC='\033[0m'

ALERT_LOG="/usr/local/var/ids_monitor/logs/ids_alerts.log"
THRESHOLD=5
INTERVAL=60
EMAIL="you@example.com"
TMP_DIR="/tmp/ids_temp_journal"
WEBHOOK_URL="https://hooks.slack.com/services/XXX/YYY/ZZZ"
OUTPUT_DIR="/usr/local/var/ids_monitor"

sudo mkdir -p "$OUTPUT_DIR"/{logs,reports} && 
sudo touch "$ALERT_LOG" && 
sudo chmod -R 755 "$OUTPUT_DIR" && 
sudo chmod 644 "$ALERT_LOG"

send_alert() {
    local ip=$1 count=$2 timestamp=$(date "+%F %T") 
    local report_file="$OUTPUT_DIR/reports/alert_${timestamp//[: ]/-}_$ip.txt"
    
    echo -e "[!] Alert: $ip had $count failed attempts at $timestamp\n${RED}[ALERT] $ip triggered $count failed SSH logins${NC}" | 
    sudo tee -a "$ALERT_LOG"
    
    printf "=== IDS ALERT REPORT ===\nTimestamp: %s\nIP: %s\nAttempts: %d\nThreshold: %d\nInterval: %d\n\nRecent activity:\n%s\n" \
        "$timestamp" "$ip" "$count" "$THRESHOLD" "$INTERVAL" \
        "$(journalctl -u ssh.service --since "5 minutes ago" | grep "$ip")" | \
    sudo tee "$report_file" >/dev/null
    
    echo "Suspicious SSH activity from $ip at $timestamp" | mail -s "IDS Alert: $ip" "$EMAIL"
    curl -s -X POST -H 'Content-type: application/json' \
        --data "{\"text\":\"!! IDS Alert: $ip had $count failed SSH logins at $timestamp.\"}" \
        "$WEBHOOK_URL"
}

monitor_logs() {
    echo "[*] Monitoring SSH logs... Output directory: $OUTPUT_DIR"
    local last_line="" current_line ip now ip_file count
    
    while true; do
        current_line=$(journalctl -u ssh.service -n 1 --no-pager)
        [[ "$current_line" == "$last_line" ]] && { sleep 1; continue; }
        
        last_line="$current_line"
        echo "[LOG] $current_line"
        
        if grep -Eqi "Failed password|Invalid user" <<< "$current_line"; then
            ip=$(grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" <<< "$current_line")
            [[ -z "$ip" ]] && continue
            
            now=$(date +%s)
            ip_file="$TMP_DIR/$ip"
            echo "$now" >> "$ip_file"
            
            awk -v cutoff=$((now - INTERVAL)) '$1 >= cutoff' "$ip_file" > "${ip_file}.tmp" 2>/dev/null
            mv "${ip_file}.tmp" "$ip_file"
            
            count=$(wc -l < "$ip_file")
            if (( count >= THRESHOLD )); then
                send_alert "$ip" "$count"
                > "$ip_file"
            fi
        fi
        sleep 1
    done
}

monitor_logs
```
***🚩 It is important to mention that sudo is not to be used in scripts in real-world cases. It was used here purely for consistency for the sake of demonstration.***


## Full Script Breakdown 🛠

### Configuration Section (Lines 1-8) 🔨
```bash
#!/bin/bash

ALERT_LOG="/usr/local/var/ids_monitor/logs/ids_alerts.log"
THRESHOLD=5
INTERVAL=60
EMAIL="you@example.com"
TMP_DIR="/tmp/ids_temp_journal"
WEBHOOK_URL="https://hooks.slack.com/services/XXX/YYY/ZZZ"
OUTPUT_DIR="/usr/local/var/ids_monitor"
```
`#!/bin/bash` - Shebang line specifying this should be executed with bash

`ALERT_LOG` - Defines where alert logs will be stored

`THRESHOLD=5` - Number of failed attempts needed to trigger an alert

`INTERVAL=60` - Time window (in seconds) to count attempts within

`EMAIL` - Email address to send alerts to

`TMP_DIR` - Temporary directory for storing IP attempt timestamps

`WEBHOOK_URL` - Slack webhook URL for notifications

`OUTPUT_DIR` - Base directory for all output files

### Setup Section (Lines 10-13) 🖇
```bash
sudo mkdir -p "$OUTPUT_DIR"/{logs,reports} && 
sudo touch "$ALERT_LOG" && 
sudo chmod -R 755 "$OUTPUT_DIR" && 
sudo chmod 644 "$ALERT_LOG"
```
Creates required directories (logs and reports subdirectories) with `mkdir -p`

Creates the alert log file if it doesn't exist with touch (empty)

Sets permissions (755 for directories, 644 for log file)

Uses sudo to ensure proper permissions (script needs root access).

***🚩 Avoid using sudo in scripts, run the script as `sudo ./<script_name>` if root privilege is required. This is done purely for consistency and demonstration purposes.***

### Alert Function (Lines 15-37) 📢
```bash
send_alert() {
    local ip=$1 count=$2 timestamp=$(date "+%F %T") 
    local report_file="$OUTPUT_DIR/reports/alert_${timestamp//[: ]/-}_$ip.txt"
```
Defines send_alert function that takes two parameters:

`$1` becomes ip (the suspicious IP address)

`$2` becomes count (number of failed attempts)

Creates timestamp in format YYYY-MM-DD HH:MM:SS

Creates report filename with sanitised timestamp (replaces colons/spaces with hyphens (file safe))

```bash
    echo -e "[!] Alert: $ip had $count failed attempts at $timestamp \n${RED} [ALERT] $ip triggered $count failed SSH logins${NC}" | sudo tee -a "$ALERT_LOG"
```
Prints colored alert message to terminal (red text) and appends to alert log then resets to no colour.

Uses `tee -a` to both display and append to file

\n (new line) then `${RED}` starts red text, `${NC}` resets color.

```bash
    printf "=== IDS ALERT REPORT ===\nTimestamp: %s\nIP: %s\nAttempts: %d\nThreshold: %d\nInterval: %d\n\nRecent activity:\n%s\n" \
        "$timestamp" "$ip" "$count" "$THRESHOLD" "$INTERVAL" \
        "$(journalctl -u ssh.service --since "5 minutes ago" | grep "$ip")" | \
    sudo tee "$report_file" >/dev/null
```
Creates detailed report with:

- Timestamp
- IP address
- Attempt count
- Threshold value
- Interval value
- Recent activity (last 5 minutes from journalctl filtered by IP)

Then saves report to file (redirecting standart output to /dev/null after tee)

```bash
    echo "Suspicious SSH activity from $ip at $timestamp" | mail -s "IDS Alert: $ip" "$EMAIL"
    curl -s -X POST -H 'Content-type: application/json' \
        --data "{\"text\":\"!! IDS Alert: $ip had $count failed SSH logins at $timestamp.\"}" \
        "$WEBHOOK_URL"
}
```
Sends email alert using mail command - **🛠 Having problems with getting the Email to work? Click here - [email_config.md](placeholder)**

Sends Slack notification via webhook using curl

***What is slack? 'Slack helps small businesses and teams scale, with less busywork. Teams can collaborate in one space, increase their efficiency with automation, and connect in the way that works best for them' - Google.***

Both include IP, count, and timestamp information

### Monitoring Function (Lines 39-68) 🔎🐱‍💻
```bash
monitor_logs() {
    echo "[*] Monitoring SSH logs... Output directory: $OUTPUT_DIR"
    local last_line="" current_line ip now ip_file count
```
Defines main monitoring function

Prints startup message showing output directory

Declares local variables:

`last_line` - stores previous log line for comparison

`current_line` - stores current log line

`ip` - will store extracted IP address

`now` - will store current timestamp

`ip_file` - will store path to IP-specific temp file

`count` - will store attempt count

```bash
    while true; do
        current_line=$(journalctl -u ssh.service -n 1 --no-pager)
        [[ "$current_line" == "$last_line" ]] && { sleep 1; continue; }
```
Starts infinite loop

Gets most recent SSH log entry from journalctl

Compares to last line - if same, sleeps 1 second and continues (avoids duplicate processing)

```bash
        last_line="$current_line"
        echo "[LOG] $current_line"
        
        if grep -Eqi "Failed password|Invalid user" <<< "$current_line"; then
```
Updates `last_line` for next comparison

Logs the line to terminal

Checks if line contains "Failed password" or "Invalid user" (case insensitive)

```bash
            ip=$(grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" <<< "$current_line")
            [[ -z "$ip" ]] && continue
            
            now=$(date +%s)
            ip_file="$TMP_DIR/$ip"
            echo "$now" >> "$ip_file"
```
Extracts IP address from log line using regex (a pattern matching an IPv4 address)

Skips if no IP found

Gets current Unix timestamp (seconds since epoch)

Sets `ip_file` path as temporary directory plus IP address

Appends current timestamp to IP's file (records attempt time)

```bash
            awk -v cutoff=$((now - INTERVAL)) '$1 >= cutoff' "$ip_file" > "${ip_file}.tmp" 2>/dev/null
            mv "${ip_file}.tmp" "$ip_file"
            
            count=$(wc -l < "$ip_file")
```
This code block is designed to filter out old login attempts and only keep recent ones (within the last 60 seconds). Here's how it works:

1. It takes a file containing timestamps of login attempts ($ip_file)

2. Calculates the cutoff time (current time minus 60 seconds)

3. Uses awk to keep only lines with timestamps newer than the cutoff

4. Saves the filtered results to a temporary file

5. Replaces the original file with the filtered one

6. Counts how many recent attempts remain

***The Mistake I Made in the Video📢:***

In the video, I accidentally used `$cutoff` instead of just `cutoff` in the `awk` command. This is wrong because:

In awk syntax, `$` before a variable means "use this as a column number"

So `$cutoff` tried to look for column 1,690,000,000 (for example) which doesn't exist

This made the command compare timestamps against 0 instead of the actual cutoff time

Surprisingly, the code still "worked" because:

All valid timestamps are greater than 0

The script clears the file completely when attacks are detected

Most attacks happen in quick bursts anyway

Why It Mattered:
While the code appeared to work, it wasn't actually doing proper time-window filtering. The fixed version (using just `cutoff`) properly removes old attempts and gives more accurate results.

Key Takeaway:
A shame, but noted! Because we silenced `awk`, even if there was an error we probably would not have seen it. Normally, code would be submitted for code reviews and this would most likely be caught by fresh eyes, but this is how you learn I suppose.


```bash
            if (( count >= THRESHOLD )); then
                send_alert "$ip" "$count"
                > "$ip_file"
            fi
        fi
        sleep 1
    done
}
```

Checks if count meets/exceeds threshold (5)

If so, calls `send_alert` with `IP` and `count`

Clears the IP's file after alert (resets counter)

Sleeps 1 second between iterations

### Main Execution (Line 70)📸
```bash
monitor_logs
```
Calls the `monitor_logs` function to start the monitoring process

This is the only command outside of function definitions, so it's what runs when the script executes

**Key Observations👁‍🗨:**

Order of Operations: The `send_alert` function is defined first but isn't executed until called conditionally from `monitor_logs` when the threshold is met.

IP and Count Parameters: In `send_alert`, `ip` and `count` come from the calling code in `monitor_logs` where:

- `ip` is extracted from log lines matching the failed login pattern

- `count` is calculated from the number of recent attempts in the IP's temp file

Rate Limiting: The script maintains separate files per IP and only counts attempts within the `INTERVAL` window, preventing persistent high counts from old attempts.

Multi-channel Alerts: When triggered, alerts go to:

- Terminal (with color coding)

- Log file

- Detailed report file

- Email

- Slack

- Temporary Files: Uses /tmp/ids_temp_journal to store attempt timestamps per IP, cleaned up automatically based on the INTERVAL.


😤
