# Error Table for Module 9 🕸

| 🧩 What You’re Doing          | ⚠️ What Goes Wrong                                | ❓ Why It Happens                                          | ✅ How To Fix It                                                                 |
|------------------------------|----------------------------------------------------|------------------------------------------------------------|----------------------------------------------------------------------------------|
| Installing `cron`            | `E: Unable to locate package cron`                | You didn’t update your package list first                  | Run `sudo apt update` before installing anything                                 |
| Starting the cron service    | Nothing happens when you try to start it          | It might already be running or the command was wrong       | Use `sudo systemctl status cron` to check if it’s already running                |
| Script not running at all    | You don’t get the disk report file as expected    | Cron couldn’t run your script or save the output           | Use full paths in the script, make it executable with `chmod +x`                |
| Using `$USER` in the script  | The output file never shows up                    | Cron doesn’t know what `$USER` means                       | Replace `$USER` with your actual username or use full paths                      |
| Editing with `crontab -e`    | Stuck in vim or can’t type                        | Vim opens in normal mode (and it’s confusing at first)     | Press `i` to start typing, then `Esc`, `:wq` to save and exit                    |
| Crontab job never triggers   | It never runs at 8am like you expect              | Wrong path to script or it’s not marked as executable      | Double check the script path and run `chmod +x script.sh`                        |
| Script works in terminal but not in cron | Nothing happens when cron runs it           | Your environment in cron is different                      | Use full paths (like `/usr/bin/df` as mentioned in commands.md) and don’t rely on environment variables      |
| Crontab seems empty          | `no crontab for user` message                     | You haven’t saved anything in `crontab -e` yet             | Open `crontab -e`, add your line, and save properly                              |

