# 📘 Project Evaluation: Bash from Scratch to IDS

Project Date of Completion: 6th August 2025

Level: Completed after Year 1 of BEng (Hons) Cybersecurity (HNC)

Project Type: Self-directed learning & teaching project

Technologies: Bash, Linux, Shell Tools, Log Analysis, API integration, GitHub, IDS scripting

**📢 Project Purpose & Motivation:**

I built this project from a clean slate after finishing my first year of university. I had very little experience with Bash, scripting, GitHub, or structuring a course, but I knew I wanted to create something tangible that not only showcased my technical growth but would also stand out on a CV and teaching is quite enjoyable. This course started with the absolute basics and developed into a complete Bash-based Intrusion Detection System (IDS) that I structured myself.

The goal wasn’t just to learn Bash, but to teach it from the ground up. This meant I had to structure the course, break down concepts for beginners, write scripts, test everything, make video tutorials, and publish the entire thing in a presentable way.

## 🔁 Learning Outcomes

**🔧 Technical Skills Gained**

Despite my incredible teachers whom I am grateful for, I began not knowing much compared to the scope of this course. But now I can confidently say I’ve developed a solid foundation in Bash scripting and its application in security.

Over 12 structured modules, I learned and taught:

Terminal basics: ls, pwd, cd, combining commands with ;, &&, and ||.

Filesystem navigation & permissions: including chmod, chown, find, and locate.

Control structures: variables, loops, conditionals — though Module 3 was my least favourite (felt repetitive and dry).

Security tooling: used Bash to run and parse tools like nmap, whois, grep, awk, sed.

Log parsing: focused on real-world logs from /var/log/auth.log using journalctl and filters like grep "Failed password".

Network scripting: checking connectivity, banner grabbing, scanning with nc and curl.

Regex: used to extract IPs, usernames, patterns from logs.

Process surveillance: detecting suspicious runtime activity with ps, watch, and process diffing.

Scheduling tasks: crontab to automate daily checks or system scans.

API integration: using curl and jq to parse JSON from services like AbuseIPDB.

Building an IDS: Module 12 was by far the most enjoyable - putting everything together into a functioning threat detection system.

I finished with a proper hands-on script that watches system logs live and alerts on suspicious SSH login attempts via email, Slack, and logs, all handled through Bash.

**💬 Communication & Teaching**

Explaining technical material to people with presumingly no background was more difficult than I expected. I had to strip back jargon and focus on clarity. Making the content digestible and enjoyable in video format took time and revision and every section had to be relevant, clear, and concise. E.g. trying to stop myself from rambling on.

It massively improved my technical communication and my ability to break down complex tasks into bite-sized, teachable concepts.

### 🧪 Modules Overview (Summary)

| Module | Title                            | Focus                                                             |
|--------|----------------------------------|-------------------------------------------------------------------|
| 1      | Intro to Bash                    | Terminal basics, combining commands, keyboard shortcuts           |
| 2      | Navigating Like a Hacker         | Filesystems, permissions, locating files                          |
| 3      | Variables, Loops, Conditionals   | Control flow, scripting basics (least favourite module)           |
| 4      | Bash with Security Tools         | nmap, whois, parsing outputs with grep, awk, etc.                 |
| 5      | Automating Log Monitoring        | journalctl, grep, log parsing, scripting failed logins            |
| 6      | Bash & Networking                | ping, curl, nc, port scanning, connectivity testing               |
| 7      | Regex and Parsing                | Regex to extract IPs, usernames from logs                         |
| 8      | Process Surveillance             | Watching for rogue processes using Bash                           |
| 9      | Crontab and Scheduling Tasks     | Setting up automated scans and reports                            |
| 10     | API-Driven Security Monitoring   | APIs, JSON parsing with jq, fetching threat info                  |
| 11     | Offensive Bash                   | Basic recon scripts for CTFs/ethical hacking                      |
| 12     | Defensive Bash - IDS Lite        | Log analysis, alerting, automation – most satisfying module       |


**💥 What Worked Well**

Course structure: Going from zero to IDS made it feel like a complete journey, not just a list of scripts.

Final product: A real-time Bash-based IDS with logging, reporting, email and Slack alerts.

Teaching by doing: Learning was reinforced by the need to teach it clearly to others.

Live terminal videos: Edited to be clean, concise (as possible), and less boring than your typical tutorials.

Repo organisation: Tutorials, script examples, folders, logs | all included in the GitHub with care.

**⚠️ What Could Be Improved**

GitHub knowledge upfront: I had to teach myself how to properly structure and publish everything, and that slowed me down more than expected.

More time scripting modules before recording: Some sections could’ve been more efficient if I’d mapped them out better first.

Maybe include interactive challenges or quizzes for people following along to make it more engaging.

**😅 Final Thoughts**

This project took a stupid amount of time, and I’m honestly relieved it’s finished. But I’m genuinely proud of it. I didn’t just learn Bash; I learned how to:

- Build and ship a course from scratch

- Teach technical concepts to beginners

- Use GitHub to manage a project

- Script and automate tasks like a real Linux admin

- Communicate, edit, write, and structure technical content in a way that’s actually useful

Whether or not anyone ever uses this course I don't really care because, it helped me grow. I started with no clue and ended with a working IDS, a full training series, and a deeper confidence in my own ability to self-learn.

And at the very least, it looks good on my CV.
