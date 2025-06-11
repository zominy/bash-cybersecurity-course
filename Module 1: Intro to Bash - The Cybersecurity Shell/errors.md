## 🐞 Common Errors in Module 1 Lab

### 1. **`lss`**
- **Reason:** Typo in command.
- **Fix:** Use `ls` to list directory contents.

### 2. **`cd /notarealplace`**
- **Reason:** Directory does not exist.
- **Fix:** Check with `ls` or use Tab to autocomplete a valid path.

### 3. **`pwd)`**
- **Reason:** Invalid syntax due to extra character.
- **Fix:** Remove the closing parenthesis – just use `pwd`.

### 4. **`ls -z`**
- **Reason:** Invalid flag for the `ls` command.
- **Fix:** Use supported options like `-l`, `-a`, `-h`.

### 5. **`cd` with no directory**
- **Reason:** Directory argument missing.
- **Fix:** Use `cd <directory>` or `cd` alone to go to the home directory.

### 6. **`echo Hello >`**
- **Reason:** Missing filename after redirection operator.
- **Fix:** Add a filename, e.g. `echo Hello > file1`.

### 7. **`mkdir` with no name**
- **Reason:** Directory name missing.
- **Fix:** Use `mkdir <name>` to create a directory.

### 8. **`nano` command not found**
- **Reason:** Nano is not installed.
- **Fix:** Install it using `sudo apt install nano`.

### 9. **`sudo` command not found**
- **Reason:** `sudo` not installed or configured improperly.
- **Fix:** Install or configure `sudo` with root access.

### 10. **`Permission denied` on file operations**
- **Reason:** Lacking write/delete permissions.
- **Fix:** Use `sudo`, or change file permissions with `chmod`.

### 11. **`nano file1` after `rm file1`**
- **Reason:** File no longer exists.
- **Fix:** Recreate the file using `touch file1` or `echo` before editing.

### 12. **`rmdir lab1` fails**
- **Reason:** Directory not empty.
- **Fix:** Use `rm -r lab1` (carefully) to remove non-empty directories.

### 13. **`cd .. ..`**
- **Reason:** Incorrect syntax for going up two directories.
- **Fix:** Use `cd ../..`.

### 14. **`su -` returns “Authentication failure”**
- **Reason:** Wrong root password or user not allowed to `su`.
- **Fix:** Use correct root password or try `sudo -i` if permitted.

### 15. **`sudo -i` gives “not in sudoers”**
- **Reason:** Your user is not in the sudo group.
- **Fix:** Add your user with `usermod -aG sudo <username>` (as root).

### 16. **File not created after `echo > file1`**
- **Reason:** Empty input or redirect error.
- **Fix:** Add content, e.g. `echo "Hi" > file1`.

### 17. **`ls` shows no output**
- **Reason:** Directory is empty.
- **Fix:** Create files or check another path.

### 18. **Tab autocomplete doesn't work**
- **Reason:** Incorrect command or bash not working properly.
- **Fix:** Check your shell config or retype the command.

### 19. **`Ctrl + L` doesn't clear terminal**
- **Reason:** Terminal emulator doesn't support it.
- **Fix:** Use the `clear` command manually.

### 20. **`ping` keeps running**
- **Reason:** No stop limit set.
- **Fix:** Stop with `Ctrl + C` or add `-c <count>`, e.g. `ping -c 4 google.com`.

### 21. **`bash: command not found`**
- **Reason:** Command is typed incorrectly or not installed.
- **Fix:** Check spelling or install the relevant package.

### 22. **“No such file or directory”**
- **Reason:** Incorrect path or file name.
- **Fix:** Double-check spelling and existence of path.

### 23. **Can't edit system files in `/etc`**
- **Reason:** Requires elevated permissions.
- **Fix:** Use `sudo nano <file>` with caution.

### 24. **Misuse of shell operators like `&&`, `;`, `||`**
- **Reason:** Incorrect syntax or spacing.
- **Fix:** Ensure spacing and order of commands is correct.

### 25. **`rm -rf` used on wrong path**
- **Reason:** Recursive deletion without checking.
- **Fix:** Double-check the path before using `rm -rf` to avoid accidental loss.
