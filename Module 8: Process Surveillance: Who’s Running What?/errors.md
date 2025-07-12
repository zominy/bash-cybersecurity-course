# Error Table For Module 8 💢

| 🧩 Area                         | ⚠️ Error Message / Behaviour                          | ❓ Cause                                                    | ✅ Fix                                                                            |
|--------------------------------|------------------------------------------------------|------------------------------------------------------------|-----------------------------------------------------------------------------------|
| Installing tools               | `E: Unable to locate package procps` (or similar)   | Package list not updated or wrong package name             | Run `sudo apt update` first and check package names                              |
| Using `watch` command          | No output updates or stuck on first output           | Command inside watch has syntax errors or quotes missing   | Make sure the command is inside quotes exactly as `watch "ps aux | grep -v grep | grep nc"` |
| Using `grep -v grep`           | Still see `grep` lines in output                      | Missing `grep -v grep` or wrong order of pipes             | Use `grep -v grep` before `grep nc` to exclude the grep process                  |
| Running `nc -lvp 4444`         | `nc: command not found` or listener doesn’t start    | NetCat not installed or wrong version installed            | Install netcat-traditional (`sudo apt install netcat-traditional`)                |
| Running the surveillance script| Script throws syntax errors or loops infinitely without output | Wrong shebang or Bash version incompatible                  | Ensure script is run with Bash (`bash script.sh`), check shebang is `#!/bin/bash` |
| Process snapshot diff          | `diff` shows too many differences or no differences  | Too short sleep time or processes start/stop too fast      | Increase sleep interval or check running processes properly                      |
| Command output formatting      | Columns in `ps` output misaligned or missing         | Using different `ps` options or system differences         | Use `ps -e` as given; avoid extra flags that change output format                |
