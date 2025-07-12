
# 📖 List of Commands used in Module 7: Regex and Parsing: Extracting Intelligence from Chaos

---

### Setup ⚙️

Due to the content of this module, you will most likely be required to install some new tools. Again, I would like to advise that if you are following along you use the same distro as me as covered in [Module 1](https://github.com/zominy/bash-cybersecurity-course/blob/main/Module%201%3A%20Intro%20to%20Bash%20-%20The%20Cybersecurity%20Shell/notes.md) (look at the setup section).

Please run the following commands:
```bash
sudo apt update
```
- Updates the package list

```bash
sudo apt install -y procps htop netcat-traditional
```
- These packages install tools like `ps`, `htop`, `watch`, and `nc`.

---

## watch "ps aux | grep -v grep | grep nc" ⏱️⚙️🔍

Explanation:

- `watch "..."`: Runs a command every 2 seconds. The command is contained between the two quotation marks. In this case it is `ps aux | grep -v grep | grep nc`.

- `ps aux`: Lists all running processes for all users. If you ran this alone, you would see a list of every single process that is currently running. An example line could look like this:
  - `maxz        3425  0.0  0.2  11220  4772 pts/0    R+   22:01   0:00 ps aux`
  - You can see the username `maxz`, and the unique process ID (PID) `3425`. The first decimal `0.0` represents CPU usage (%) where here it has used barely any. The next number is the memory usage (%) which is `0.2` in this case. `pts/0` tells us the terminal associated with the process (terminal 0 in this case). The `R+` refers to the process status, and the `ps aux` is the command that was ran. All in all, we can gather that `maxz` ran the `ps aux` command at 22:01 in terminal 0, the command didnt use much memory or processor power.
 
- `grep -v grep`: Excludes the grep command itself because we will be parsing the running processes for NetCat processes.
- `grep nc`: Only shows lines containing `nc`. Basically filters for anything with 'nc' in the command.

Example Output WITHOUT `grep -v grep`:
```bash
Every 2.0s: ps aux | grep nc                                                                                                                                    zom: Sat Jul 12 22:17:06 2025

systemd+     415  0.0  0.3  90260  6964 ?        Ssl  21:51   0:00 /lib/systemd/systemd-timesyncd
maxz        1587  0.0  0.4 311216  9692 ?        Sl   21:54   0:00 /usr/libexec/at-spi-bus-launcher --launch-immediately
maxz        4094  0.0  0.1   6840  3612 pts/0    S+   22:16   0:00 watch ps aux | grep nc
maxz        4218  0.0  0.0   6840  1760 pts/0    S+   22:17   0:00 watch ps aux | grep nc
maxz        4219  0.0  0.0   2580   900 pts/0    S+   22:17   0:00 sh -c ps aux | grep nc
maxz        4221  0.0  0.1   6336  2108 pts/0    S+   22:17   0:00 grep nc
```
You can see why we include the `grep -v grep` section. We do not want to see it as it is not relevant.

Example Output WITH `grep -v grep`:
```bash
Every 2.0s: ps aux | grep -v grep | grep nc                                                                                                                     zom: Sat Jul 12 22:18:08 2025

systemd+     415  0.0  0.3  90260  6964 ?        Ssl  21:51   0:00 /lib/systemd/systemd-timesyncd
maxz        1587  0.0  0.4 311216  9692 ?        Sl   21:54   0:00 /usr/libexec/at-spi-bus-launcher --launch-immediately
```
This is much better. Now we will open up a second terminal. Remeber this is always going to run as we used the `watch` command. This keeps it updating every 2 seconds until we use Ctrl + C to stop the command.

See below for the next part to this.

---

## nc -lvp 4444 🎧🛠️

Explanation: NetCat is a great all-rounder of a tool. However, it can also be used maliciously. This command opens a listener on port 4444 (you can use other ports too), waiting for incoming connections; like a reverse shell. This can be a big security risk. So this is why we are using the previous command `watch "ps aux | grep -v grep | grep nc"` to spot these listeners.

In the new terminal please enter the command. Then look at the original terminal and you will see a new process running, that looks something like this:
```bash
Every 2.0s: ps aux | grep -v grep | grep nc                                                                                                                     zom: Sat Jul 12 22:23:02 2025

systemd+     415  0.0  0.3  90260  6964 ?        Ssl  21:51   0:00 /lib/systemd/systemd-timesyncd
maxz        1587  0.0  0.4 311216  9692 ?        Sl   21:54   0:00 /usr/libexec/at-spi-bus-launcher --launch-immediately
maxz        5048  0.0  0.0   2484  1756 pts/1    S+   22:22   0:00 nc -lvp 4444
```
You can now see the new line. We have captured a listener in a different terminal.

As previously mentioned. Note the PID of this listener is `5048`. We can actually use the command `kill` to stop the listener. By running `kill 5048` you will see that the process dissapears, indicating that it is no longer running. (I just thought of that while writing this)

---

## Process Surveillance Script 👁️‍🗨️🔧

Here is the script:
```bash
#!/bin/bash

previous_snapshot=$(ps -e)

while true; do
    sleep 5
    current_snapshot=$(ps -e)
    diff <(echo "$previous_snapshot") <(echo "$current_snapshot")
    previous_snapshot=$current_snapshot
done
```
`#!/bin/bash`: Shebang. Tells the system this is a Bash script.

`previous_snapshot=$(ps -e)`: Stores the current list of processes. The `ps -e` command only displays default columns: `PID`, `TTY`, `TIME`, `COMMAND`. It excludes things like memory usage and CPU usage etc.

`while true; do`: This is a while loop. In this case, it starts an infinite loop that mimics how the `watch` command works. Except there is 5 second intervals.

`sleep 5`: Waits 5 seconds between checks. Also necessary to not overload the output.

`current_snapshot=$(ps -e)`: Gets the updated list of processes. 

`diff <(echo "$previous_snapshot") <(echo "$current_snapshot")`: Shows the difference between old and new process lists.

`previous_snapshot=$current_snapshot`: Updates the snapshot for the next loop.

`done`: This marks the end of the while loop (like in for loops). The script will jump back to the while true condition (infinite loop).

Basically, we take the first snapshot, then get a new one and compare them to see what has changed between intervals. We then update the previous_snapshot variable to hold the last taken snapshot. Then the current_snapshot variable is updated again and they are compared again. This loops until it is interrupted. (e.g. Ctrl + C).

This took me a good minute to actually understand. Check out the video, and you may understand it better there.

Cheeky.
