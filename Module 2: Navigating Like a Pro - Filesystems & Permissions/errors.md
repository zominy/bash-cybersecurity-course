## 🐞 Common Errors in Module 2 Lab

### 1. **`mkdir lab/module2/test`**
**Reason:** The parent folders don't exist yet.  
**Fix:** Use `mkdir -p lab/module2/test` to create everything in one go, including any missing folders.

### 2. **`cd lab/module2/test/../../module2` confusion**
**Reason:** Bit of a confusing path – can be easy to get lost.  
**Fix:** Try navigating step-by-step or use `tree lab` to see the folder layout.

### 3. **`tree` command not found**
**Reason:** `tree` isn’t installed by default.  
**Fix:** Just run `sudo apt install tree` to get it set up.

### 4. **Using `tree` without a level limit**
**Reason:** The output can be overwhelming if you've got lots of stuff.  
**Fix:** Use `tree -L 1` to only show the top level – much tidier.

### 5. **Not sure what numbers like `700` or `400` mean in `chmod`**
**Reason:** The permissions format can be a bit confusing at first.  
**Fix:** Just remember: `r = 4`, `w = 2`, `x = 1`. Add them up per group (owner, group, others).

### 6. **`chmod 100 newfile.txt`**
**Reason:** File ends up only executable – no read or write access.  
**Fix:** If you're after more control, try something like `chmod 600` (read/write) or `chmod 700` (read/write/execute).

### 7. **`touch` gives "Permission denied"**
**Reason:** Trying to make a file where you’re not allowed to.  
**Fix:** Stick to your home directory or use `sudo` if you really need to write elsewhere.

### 8. **`sudo chown $USER:$USER newfile.txt` doesn’t work**
**Reason:** `$USER` might not expand properly in some shells.  
**Fix:** Use `echo $USER` to check, or just type your username directly, e.g. `sudo chown max:max newfile.txt`.

### 9. **`find /etc -name "*log*conf"` gives "Permission denied"**
**Reason:** You’re hitting folders you don’t have access to.  
**Fix:** Just pop `sudo` at the start of the command to run it as a superuser.

### 10. **Using `find /etc -name *.conf` without quotes**
**Reason:** The shell tries to expand `*` before `find` even runs.  
**Fix:** Always put wildcards in quotes: `find /etc -name "*.conf"`

### 11. **Getting mixed up with `cd /` vs `cd ..`**
**Reason:** Easy to confuse absolute and relative paths.  
**Fix:** `cd /` takes you to the root folder. `cd ..` goes up one level from wherever you are.

### 12. **Created a file with `touch` but can’t find it**
**Reason:** You may have made it in a different folder than you thought.  
**Fix:** Run `pwd` to check where you are, or try `find . -name newfile.txt`.

### 13. **Thinking `chmod` changes permissions inside folders too**
**Reason:** By default, it only changes the folder/file you target.  
**Fix:** Add `-R` for recursive changes, like `chmod -R 700 myfolder/`.

### 14. **`chown` fails without sudo**
**Reason:** Only the root user can change file ownership.  
**Fix:** Use `sudo chown $USER:$USER newfile.txt` to do it properly.

