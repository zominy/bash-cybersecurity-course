# 📖 Commands — Module 12: Defensive Bash: Build Your Own IDS Lite (with journalctl)

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

# Configuration
ALERT_LOG="/usr/local/var/ids_monitor/logs/ids_alerts.log"
THRESHOLD=5
INTERVAL=60
EMAIL="you@example.com"
TMP_DIR="/tmp/ids_temp_journal"
WEBHOOK_URL="https://hooks.slack.com/services/XXX/YYY/ZZZ"
OUTPUT_DIR="/usr/local/var/ids_monitor"

# Setup
sudo mkdir -p "$OUTPUT_DIR"/{logs,reports} && 
sudo touch "$ALERT_LOG" && 
sudo chmod -R 755 "$OUTPUT_DIR" && 
sudo chmod 644 "$ALERT_LOG"

send_alert() {
    local ip=$1 count=$2 timestamp=$(date "+%F %T") 
    local report_file="$OUTPUT_DIR/reports/alert_${timestamp//[: ]/-}_$ip.txt"
    
    # Terminal and log output
    echo -e "[!] Alert: $ip had $count failed attempts at $timestamp\n\033[1;31m[ALERT] $ip triggered $count failed SSH logins\033[0m" | 
    sudo tee -a "$ALERT_LOG"
    
    # Detailed report
    printf "=== IDS ALERT REPORT ===\nTimestamp: %s\nIP: %s\nAttempts: %d\nThreshold: %d\nInterval: %d\n\nRecent activity:\n%s\n" \
        "$timestamp" "$ip" "$count" "$THRESHOLD" "$INTERVAL" \
        "$(journalctl -u ssh.service --since "5 minutes ago" | grep "$ip")" |
    sudo tee "$report_file" >/dev/null
    
    # Notifications
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
            
            # Maintain time window
            grep -Ev "^$((now - INTERVAL))" "$ip_file" > "${ip_file}.tmp" 2>/dev/null
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

