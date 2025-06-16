## 🐞 Common Errors in Module 1 Lab

### 1. **`lss`**
- **Reason:** Typo in command.
- **Fix:** Use `ls` to list directory contents.

### 2. **`pwd)`**
- **Reason:** Invalid syntax due to extra character.
- **Fix:** Remove the closing parenthesis so basically just use `pwd`.

### 3. **`ls -z`**
- **Reason:** Invalid flag for the `ls` command.
- **Fix:** Use supported options like `-l`, `-a`, `-h`.

### 4. **`cd` with no directory**
- **Reason:** Directory argument missing.
- **Fix:** Use `cd <directory>` or `cd` alone to go to the home directory.

### 5. **`echo Hello >`**
- **Reason:** Missing filename after redirection operator.
- **Fix:** Add a filename, e.g. `echo Hello > file1`.

### 6. **`mkdir` with no name**
- **Reason:** Directory name missing.
- **Fix:** Use `mkdir <name>` to create a directory.

### 7. **`nano` command not found**
- **Reason:** Nano is not installed.
- **Fix:** Install it using `sudo apt install nano`.

### 8. **`sudo` command not found**
- **Reason:** `sudo` not installed or configured improperly.
- **Fix:** Install or configure `sudo` with root access.

### 9. **`Permission denied` on file operations**
- **Reason:** Lacking write/delete permissions.
- **Fix:** Use `sudo`, or change file permissions with `chmod`.

### 10. **`nano file1` after `rm file1`**
- **Reason:** File no longer exists.
- **Fix:** Recreate the file using `touch file1` or `echo` before editing.

### 11. **`rmdir lab1` fails**
- **Reason:** Directory not empty.
- **Fix:** Use `rm -r lab1` (carefully) to remove non-empty directories.

### 12. **`cd .. ..`**
- **Reason:** Incorrect syntax for going up two directories.
- **Fix:** Use `cd ../..`.

### 13. **`su -` returns “Authentication failure”**
- **Reason:** Wrong root password or user not allowed to `su`.
- **Fix:** Use correct root password or try `sudo -i` if permitted.

### 14. **`sudo -i` gives “not in sudoers”**
- **Reason:** Your user is not in the sudo group.
- **Fix:** Add your user with `usermod -aG sudo <username>` (as root).

### 15. **File not created after `echo > file1`**
- **Reason:** Empty input or redirect error.
- **Fix:** Add content, e.g. `echo "Hi" > file1`.

### 16. **`ls` shows no output**
- **Reason:** Directory is empty.
- **Fix:** Create files or check another path.

### 17. **Tab autocomplete doesn't work**
- **Reason:** Incorrect command or bash not working properly.
- **Fix:** Check your shell config or retype the command.

### 18. **`Ctrl + L` doesn't clear terminal**
- **Reason:** Terminal emulator doesn't support it.
- **Fix:** Use the `clear` command manually.

### 19. **`ping` keeps running**
- **Reason:** No stop limit set.
- **Fix:** Stop with `Ctrl + C` or add `-c <count>`, e.g. `ping -c 4 google.com`.

### 20. **`bash: command not found`**
- **Reason:** Command is typed incorrectly or not installed.
- **Fix:** Check spelling or install the relevant package.

### 21. **“No such file or directory”**
- **Reason:** Incorrect path or file name.
- **Fix:** Double-check spelling and existence of path.

### 22. **Can't edit system files in `/etc`**
- **Reason:** Requires elevated permissions.
- **Fix:** Use `sudo nano <file>` with caution.

### 23. **Misuse of shell operators like `&&`, `;`, `||`**
- **Reason:** Incorrect syntax or spacing.
- **Fix:** Ensure spacing and order of commands is correct.

### 24. **`rm -rf` used on wrong path**
- **Reason:** Recursive deletion without checking.
- **Fix:** Double-check the path before using `rm -rf` to avoid accidental loss.
