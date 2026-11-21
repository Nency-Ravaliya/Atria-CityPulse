# 🐧 Linux Shell

## Table of Contents

1. [Important Linux Concepts](#1-important-linux-concepts)
2. [Command Anatomy](#2-command-anatomy)
3. [File & Directory Commands](#3-file--directory-commands)
4. [File Viewing & Searching Commands](#4-file-viewing--searching-commands)
5. [File Permissions & Ownership](#5-file-permissions--ownership)
6. [Process Management Commands](#6-process-management-commands)
7. [User & Group Management](#7-user--group-management)
8. [Networking Commands](#8-networking-commands)
9. [Disk & System Info Commands](#9-disk--system-info-commands)
10. [Package Management](#10-package-management)
11. [Archiving & Compression](#11-archiving--compression)
12. [Useful Shell Features](#12-useful-shell-features)
13. [Shell Scripting Basics](#13-shell-scripting-basics)
14. [Practice Scripts](#14-practice-scripts)
15. [Small Exercises](#15-small-exercises)
16. [Safety & Good Habits](#16-safety--good-habits)

---

## 1. Important Linux Concepts

### Operating System and Distribution

Linux is the core of the operating system, called the **kernel**. Ubuntu, Debian, Fedora, and Linux Mint are Linux **distributions** (complete operating systems built around Linux). Commands are mostly similar across distributions.

### Terminal and Shell

The **terminal** is the window where we type commands. The **shell** reads those commands and asks the operating system to perform them. **Bash** is the most common beginner shell.

### File System

Linux stores files in one tree that starts at `/`, called the **root directory**. Unlike Windows, it does not normally use drives such as `C:`.

```text
/                   Root of the complete file system
/home/student       A normal user's home folder
/etc                System configuration files
/bin and /usr/bin   Common command programs
/var/log            Log files
/tmp                Temporary files
/dev                Device files (disks, terminals)
/proc               Virtual filesystem for process info
~                   Shortcut for the current user's home folder
.                   Current directory
..                  Parent directory
```

Linux names are **case-sensitive**: `Notes.txt` and `notes.txt` are different files.

### Users, Groups, and Permissions

Every file has an owner, a group, and permissions. Run `ls -l` to see them.

```text
-rwxr-xr--  1 student students 120 Nov 21 hello.sh
```

The three permission groups are **user/owner**, **group**, and **others**. The letters mean:

- `r` — read
- `w` — write/change
- `x` — execute/run
- `-` — permission not granted

### Processes

A **process** is a program that is currently running. `ps` lists processes, `top` shows a live view, and `kill` requests that a process stop.

### Packages

Software is commonly installed through a package manager. On Ubuntu/Debian, use `apt`; on Fedora, use `dnf`. Examples in this README use Ubuntu/Debian.

---

## 2. Command Anatomy

Most commands follow this pattern:

```bash
command [options] [arguments]
```

Example:

```bash
ls -la /home/student
```

- `ls` — the command
- `-la` — options (`l` = detailed list, `a` = include hidden files)
- `/home/student` — the argument

| Tip | Shortcut |
|-----|----------|
| Autocomplete file/folder name | `Tab` |
| Reuse a previous command | `↑ Up Arrow` |
| Stop the running command | `Ctrl + C` |
| Pause a running process | `Ctrl + Z` |
| Clear the terminal screen | `Ctrl + L` |
| Exit the terminal or shell | `Ctrl + D` |
| Search command history | `Ctrl + R` |

---

## 3. File & Directory Commands

### Navigation

| Command | Example | Description |
|---------|---------|-------------|
| `pwd` | `pwd` | Print the current working directory path |
| `ls` | `ls -la` | List files; `-l` = long format, `-a` = include hidden files |
| `ls -lh` | `ls -lh` | Long format with human-readable file sizes |
| `ls -lt` | `ls -lt` | List sorted by modification time (newest first) |
| `cd` | `cd Documents` | Change into a directory |
| `cd ..` | `cd ..` | Go up one directory level |
| `cd ~` | `cd ~` | Go to your home directory |
| `cd -` | `cd -` | Go back to the previous directory |
| `cd /` | `cd /` | Go to the root directory |

```bash
# Examples
pwd                      # /home/student
ls -la                   # lists all files including hidden ones
ls -lh /var/log          # list files in /var/log with readable sizes
cd /etc                  # navigate to /etc
cd ~/projects            # navigate to projects folder in home
```

### Creating Files and Directories

| Command | Example | Description |
|---------|---------|-------------|
| `touch` | `touch notes.txt` | Create an empty file or update its timestamp |
| `mkdir` | `mkdir projects` | Create a new directory |
| `mkdir -p` | `mkdir -p a/b/c` | Create nested directories in one command |
| `nano` | `nano notes.txt` | Open a beginner-friendly terminal text editor |
| `vi` / `vim` | `vi notes.txt` | Open the powerful Vi/Vim text editor |

```bash
touch file1.txt file2.txt file3.txt    # create multiple files at once
mkdir -p project/src/utils             # create nested dirs
```

### Copying, Moving, and Renaming

| Command | Example | Description |
|---------|---------|-------------|
| `cp` | `cp notes.txt backup.txt` | Copy a file |
| `cp -r` | `cp -r folder/ copy_folder/` | Copy a directory recursively |
| `cp -i` | `cp -i a.txt b.txt` | Prompt before overwriting |
| `mv` | `mv old.txt new.txt` | Rename a file |
| `mv` | `mv file.txt /tmp/` | Move a file to another directory |
| `mv -i` | `mv -i a.txt b.txt` | Prompt before overwriting |

```bash
cp report.txt /home/student/backup/    # copy to another folder
cp -r project/ project_backup/        # copy entire directory
mv draft.txt final.txt                 # rename file
mv *.log /var/log/archive/             # move all .log files
```

### Deleting Files and Directories

| Command | Example | Description |
|---------|---------|-------------|
| `rm` | `rm old.txt` | Delete a file |
| `rm -i` | `rm -i old.txt` | Prompt before deleting (safe practice) |
| `rm -f` | `rm -f old.txt` | Force delete without prompting |
| `rm -r` | `rm -r folder/` | Delete a directory and all its contents |
| `rm -rf` | `rm -rf folder/` | Force delete directory recursively — **use with extreme caution!** |
| `rmdir` | `rmdir empty_folder` | Delete an empty directory only |

```bash
rm temp.txt                    # delete a single file
rm -i *.txt                    # ask before deleting each .txt file
rm -r old_project/             # delete entire directory
rmdir empty_dir                # only works if directory is empty
```

> **⚠️ Warning:** `rm -rf` is permanent. Linux does not have a "Recycle Bin" in the terminal. Always double-check before running it.

### Linking Files

| Command | Example | Description |
|---------|---------|-------------|
| `ln` | `ln file.txt hardlink.txt` | Create a hard link |
| `ln -s` | `ln -s /etc/hosts myhosts` | Create a symbolic (soft) link |

```bash
ln -s /var/log/syslog ~/logs/syslog   # symlink to a log file
```

---

## 4. File Viewing & Searching Commands

### Viewing File Contents

| Command | Example | Description |
|---------|---------|-------------|
| `cat` | `cat notes.txt` | Print the entire file content to the screen |
| `cat -n` | `cat -n notes.txt` | Print content with line numbers |
| `tac` | `tac notes.txt` | Print file content in reverse order |
| `less` | `less large.log` | Read a large file page by page; press `q` to exit |
| `more` | `more large.log` | Like `less` but simpler (older tool) |
| `head` | `head notes.txt` | Show the first 10 lines (default) |
| `head -n 5` | `head -n 5 notes.txt` | Show the first 5 lines |
| `tail` | `tail notes.txt` | Show the last 10 lines (default) |
| `tail -n 20` | `tail -n 20 notes.txt` | Show the last 20 lines |
| `tail -f` | `tail -f app.log` | Follow a file live (great for logs) |

```bash
cat /etc/os-release            # view OS info file
less /var/log/syslog           # read a large log file
head -n 3 /etc/passwd          # first 3 lines of passwd file
tail -f /var/log/auth.log      # live-follow authentication log
```

### Searching Text

| Command | Example | Description |
|---------|---------|-------------|
| `grep` | `grep "error" app.log` | Search for a pattern in a file |
| `grep -i` | `grep -i "error" app.log` | Case-insensitive search |
| `grep -r` | `grep -r "TODO" ./src/` | Recursively search in all files under a directory |
| `grep -n` | `grep -n "fail" app.log` | Show line numbers with matches |
| `grep -v` | `grep -v "debug" app.log` | Show lines that do NOT match |
| `grep -c` | `grep -c "error" app.log` | Count matching lines |
| `grep -l` | `grep -rl "TODO" ./` | List only file names with matches |

```bash
grep "login" /var/log/auth.log          # find login events
grep -rn "import" ./project/            # find all import statements recursively
grep -i "warning\|error" app.log        # search for warning OR error
ps aux | grep python                    # find running python processes
```

### Finding Files

| Command | Example | Description |
|---------|---------|-------------|
| `find` | `find . -name "*.py"` | Find all Python files below the current folder |
| `find -type f` | `find /tmp -type f` | Find only regular files |
| `find -type d` | `find . -type d` | Find only directories |
| `find -size` | `find . -size +1M` | Find files larger than 1 MB |
| `find -mtime` | `find . -mtime -7` | Find files modified in the last 7 days |
| `find -empty` | `find . -empty` | Find empty files and directories |
| `find -exec` | `find . -name "*.log" -exec rm {} \;` | Find and execute a command on each result |
| `locate` | `locate notes.txt` | Quickly find a file by name (uses a prebuilt index) |
| `which` | `which python3` | Show where a command binary is located |
| `whereis` | `whereis ls` | Show binary, source, and manual for a command |

```bash
find /home -name "*.txt" -type f            # find all .txt files in /home
find . -name "*.log" -mtime +30             # logs older than 30 days
find /var -size +100M                       # files bigger than 100 MB
find . -empty -type d                       # find empty directories
```

### Text Processing Commands

| Command | Example | Description |
|---------|---------|-------------|
| `wc` | `wc -l notes.txt` | Count lines (`-l`), words (`-w`), characters (`-c`) |
| `sort` | `sort names.txt` | Sort lines alphabetically |
| `sort -n` | `sort -n numbers.txt` | Sort numerically |
| `sort -r` | `sort -r names.txt` | Sort in reverse order |
| `uniq` | `sort names.txt \| uniq` | Remove consecutive duplicate lines |
| `uniq -c` | `sort names.txt \| uniq -c` | Prefix lines with count of occurrences |
| `cut` | `cut -d: -f1 /etc/passwd` | Cut specific fields (delimiter `:`, field 1) |
| `awk` | `awk '{print $1}' file.txt` | Print the first column of each line |
| `sed` | `sed 's/old/new/g' file.txt` | Replace "old" with "new" in every line |
| `tr` | `echo "hello" \| tr 'a-z' 'A-Z'` | Translate (here: lowercase to uppercase) |
| `diff` | `diff file1.txt file2.txt` | Show differences between two files |

```bash
wc -l /etc/passwd                          # how many users are defined
sort -u names.txt                          # sort and remove duplicates
cut -d',' -f2 data.csv                     # extract column 2 from a CSV
awk '{sum += $1} END {print sum}' nums.txt # sum first column
sed -i 's/foo/bar/g' config.txt            # replace in-place
diff original.txt modified.txt             # compare two files
```

---

## 5. File Permissions & Ownership

### Understanding Permissions

```text
-rwxr-xr--  1  student  students  1024  Nov 21  hello.sh
│├──┤├──┤├──┤
│ │   │   └── others: read only (r--)
│ │   └────── group: read + execute (r-x)
│ └────────── user/owner: read + write + execute (rwx)
└──────────── file type: - = file, d = directory, l = symlink
```

### chmod — Change Permissions

| Command | Example | Description |
|---------|---------|-------------|
| `chmod +x` | `chmod +x script.sh` | Add execute permission for all |
| `chmod -w` | `chmod -w file.txt` | Remove write permission for all |
| `chmod 755` | `chmod 755 script.sh` | Owner: rwx, Group: r-x, Others: r-x |
| `chmod 644` | `chmod 644 notes.txt` | Owner: rw-, Group: r--, Others: r-- |
| `chmod 600` | `chmod 600 secret.txt` | Owner: rw-, Group: none, Others: none |
| `chmod -R 755` | `chmod -R 755 folder/` | Recursively apply permissions to a directory |

**Numeric Permission Table:**

| Number | Permission |
|--------|------------|
| `7` | rwx (read + write + execute) |
| `6` | rw- (read + write) |
| `5` | r-x (read + execute) |
| `4` | r-- (read only) |
| `0` | --- (no permissions) |

```bash
chmod +x deploy.sh           # make a script runnable
chmod 700 private/           # only owner can access
chmod 644 index.html         # typical web file permission
chmod -R 755 public/         # apply to entire directory
```

### chown — Change Ownership

| Command | Example | Description |
|---------|---------|-------------|
| `chown` | `chown student file.txt` | Change file owner |
| `chown user:group` | `chown student:students file.txt` | Change owner and group |
| `chown -R` | `chown -R student:students folder/` | Recursively change ownership |
| `chgrp` | `chgrp www-data file.txt` | Change only the group |

```bash
sudo chown root:root /etc/myconfig         # give root ownership
sudo chown -R www-data:www-data /var/www/  # for a web server folder
```

### umask — Default Permission Mask

```bash
umask               # view current default permission mask
umask 022           # set default: files get 644, dirs get 755
```

---

## 6. Process Management Commands

### Viewing Processes

| Command | Example | Description |
|---------|---------|-------------|
| `ps` | `ps` | Show processes for the current shell |
| `ps aux` | `ps aux` | Show all running processes for all users |
| `ps -ef` | `ps -ef` | Show full-format process listing |
| `top` | `top` | Interactive live view of processes and resource use |
| `htop` | `htop` | Enhanced interactive process viewer (needs install) |
| `pgrep` | `pgrep python` | Find process IDs by name |
| `pidof` | `pidof nginx` | Find the PID of a named process |
| `jobs` | `jobs` | List background and suspended jobs in the current shell |

```bash
ps aux | grep nginx              # find nginx process
ps aux --sort=-%mem | head -10   # top 10 memory-consuming processes
top -u student                   # show processes for user 'student'
```

### Controlling Processes

| Command | Example | Description |
|---------|---------|-------------|
| `kill` | `kill 1234` | Send SIGTERM (graceful stop) to process ID 1234 |
| `kill -9` | `kill -9 1234` | Force kill (SIGKILL) — cannot be ignored |
| `kill -l` | `kill -l` | List all available signal names |
| `killall` | `killall python` | Kill all processes with a given name |
| `pkill` | `pkill -f script.py` | Kill processes matching a pattern |
| `bg` | `bg %1` | Resume a suspended job in the background |
| `fg` | `fg %1` | Bring a background job to the foreground |
| `nohup` | `nohup ./server.sh &` | Run a command that continues after logout |
| `&` | `python app.py &` | Run a command in the background |

```bash
sleep 100 &              # run sleep in background
jobs                     # see background jobs
fg %1                    # bring job 1 to foreground
Ctrl + Z                 # suspend the foreground process
bg                       # resume it in background
kill $(pgrep python)     # kill all python processes
```

### Process Priorities

| Command | Example | Description |
|---------|---------|-------------|
| `nice` | `nice -n 10 ./task.sh` | Start a command with lower priority (10) |
| `renice` | `renice -n 5 -p 1234` | Change priority of a running process |

```bash
nice -n 19 ./heavy_script.sh     # run with lowest priority
sudo renice -n -5 -p 2345        # increase priority (needs sudo)
```

---

## 7. User & Group Management

### User Information

| Command | Example | Description |
|---------|---------|-------------|
| `whoami` | `whoami` | Print the current logged-in username |
| `id` | `id` | Show user ID, group ID, and all groups |
| `id username` | `id student` | Show info for a specific user |
| `who` | `who` | Show who is currently logged in |
| `w` | `w` | Show who is logged in and what they are doing |
| `last` | `last` | Show recent login history |
| `finger` | `finger student` | Show user info (needs install) |

```bash
id                           # uid=1000(student) gid=1000(student)
who                          # student   pts/0   Nov 21 09:00
last | head -10              # last 10 login records
```

### Managing Users (requires sudo)

| Command | Example | Description |
|---------|---------|-------------|
| `useradd` | `sudo useradd alice` | Create a new user |
| `useradd -m` | `sudo useradd -m alice` | Create user with a home directory |
| `useradd -s` | `sudo useradd -s /bin/bash alice` | Set the login shell |
| `passwd` | `sudo passwd alice` | Set or change a user's password |
| `passwd` | `passwd` | Change your own password |
| `usermod -aG` | `sudo usermod -aG sudo alice` | Add user to a group |
| `usermod -l` | `sudo usermod -l newname alice` | Rename a user |
| `userdel` | `sudo userdel alice` | Delete a user account |
| `userdel -r` | `sudo userdel -r alice` | Delete user and their home directory |

```bash
sudo useradd -m -s /bin/bash bob      # add user bob with home dir
sudo passwd bob                       # set bob's password
sudo usermod -aG sudo bob             # give bob sudo access
sudo userdel -r bob                   # remove bob and home dir
```

### Managing Groups

| Command | Example | Description |
|---------|---------|-------------|
| `groups` | `groups` | Show groups the current user belongs to |
| `groups username` | `groups alice` | Show groups for a specific user |
| `groupadd` | `sudo groupadd developers` | Create a new group |
| `groupdel` | `sudo groupdel developers` | Delete a group |
| `gpasswd -a` | `sudo gpasswd -a alice developers` | Add user to a group |
| `gpasswd -d` | `sudo gpasswd -d alice developers` | Remove user from a group |

```bash
groups                               # student sudo docker
sudo groupadd webteam                # create new group
sudo gpasswd -a alice webteam        # add alice to webteam
```

### Switching Users

| Command | Example | Description |
|---------|---------|-------------|
| `su` | `su alice` | Switch to another user (requires their password) |
| `su -` | `su - alice` | Switch and load alice's environment |
| `sudo` | `sudo command` | Run a single command as root |
| `sudo -i` | `sudo -i` | Open a root shell session |
| `sudo -u` | `sudo -u alice command` | Run command as another user |
| `exit` | `exit` | Exit the current user session |

```bash
sudo apt update                      # run as root
sudo -u www-data whoami              # run as www-data user
su - alice                           # fully switch to alice
exit                                 # return to previous user
```

### Relevant Files

```bash
cat /etc/passwd     # all user accounts (username:x:uid:gid:info:home:shell)
cat /etc/group      # all groups
cat /etc/shadow     # hashed passwords (root only)
cat /etc/sudoers    # sudo permissions (edit with: sudo visudo)
```

---

## 8. Networking Commands

### Basic Network Info

| Command | Example | Description |
|---------|---------|-------------|
| `ip addr` | `ip addr` | Show all network interfaces and IP addresses |
| `ip addr show` | `ip addr show eth0` | Show info for a specific interface |
| `ip route` | `ip route` | Show the routing table |
| `hostname` | `hostname` | Display the system's hostname |
| `hostname -I` | `hostname -I` | Show all IP addresses of the system |
| `ifconfig` | `ifconfig` | Show network interfaces (older tool, needs `net-tools`) |

```bash
ip addr                         # show all interfaces
ip addr show eth0               # show eth0 details
hostname -I                     # list all IPs
```

### Testing Connectivity

| Command | Example | Description |
|---------|---------|-------------|
| `ping` | `ping google.com` | Continuously ping a host |
| `ping -c 4` | `ping -c 4 google.com` | Ping exactly 4 times |
| `traceroute` | `traceroute google.com` | Show the path packets take to a host |
| `mtr` | `mtr google.com` | Combined ping + traceroute (interactive) |
| `nslookup` | `nslookup google.com` | Query DNS for a domain |
| `dig` | `dig google.com` | Detailed DNS lookup |
| `curl` | `curl https://example.com` | Fetch content from a URL |
| `curl -I` | `curl -I https://example.com` | Fetch only the HTTP headers |
| `wget` | `wget https://example.com/file.zip` | Download a file from the internet |

```bash
ping -c 3 8.8.8.8                    # ping Google's DNS 3 times
traceroute google.com                 # show route to google
dig google.com                        # DNS lookup
curl -s https://api.ipify.org         # see your public IP
wget https://releases.ubuntu.com/ubuntu.iso   # download a file
```

### Ports and Connections

| Command | Example | Description |
|---------|---------|-------------|
| `ss` | `ss -tuln` | Show listening TCP/UDP ports |
| `ss -tp` | `ss -tp` | Show TCP connections with process info |
| `netstat` | `netstat -tuln` | Older command similar to ss (needs `net-tools`) |
| `lsof -i` | `lsof -i :8080` | Show which process is using port 8080 |
| `nmap` | `nmap localhost` | Scan open ports on a host (needs install) |

```bash
ss -tuln                             # see all listening ports
ss -tp | grep ESTAB                  # established connections
sudo lsof -i :80                     # who is using port 80?
```

### File Transfer

| Command | Example | Description |
|---------|---------|-------------|
| `scp` | `scp file.txt user@server:/path/` | Copy file to a remote server via SSH |
| `scp -r` | `scp -r folder/ user@server:/path/` | Copy folder to remote server |
| `rsync` | `rsync -avz src/ user@server:/dst/` | Sync files efficiently (only changed files) |
| `ssh` | `ssh user@server.com` | Connect to a remote machine via SSH |
| `ssh -p` | `ssh -p 2222 user@server.com` | Connect on a specific port |

```bash
scp report.pdf student@192.168.1.10:~/docs/   # copy file to remote
rsync -avz ./project/ user@server:/var/www/   # sync project folder
ssh student@192.168.1.10                       # connect to remote
```

---

## 9. Disk & System Info Commands

### Disk Usage

| Command | Example | Description |
|---------|---------|-------------|
| `df` | `df -h` | Show free disk space (human-readable) |
| `df -T` | `df -T` | Show filesystem type as well |
| `du` | `du -sh Downloads/` | Show total size of a folder |
| `du -h` | `du -h *` | Show sizes of all items in current dir |
| `du --max-depth=1` | `du -h --max-depth=1 /var` | Folder sizes one level deep |
| `lsblk` | `lsblk` | List block devices (disks and partitions) |
| `blkid` | `sudo blkid` | Show UUIDs and types of partitions |
| `fdisk -l` | `sudo fdisk -l` | List all disk partitions |
| `mount` | `mount` | Show currently mounted filesystems |

```bash
df -h                                # disk space overview
du -sh /var/log                      # how big is /var/log?
du -h --max-depth=1 /home/student/   # breakdown of home folder
lsblk                                # list all disks and partitions
```

### System Information

| Command | Example | Description |
|---------|---------|-------------|
| `uname -a` | `uname -a` | Show all system/kernel information |
| `uname -r` | `uname -r` | Show only the kernel version |
| `uname -m` | `uname -m` | Show the machine architecture |
| `lsb_release -a` | `lsb_release -a` | Show Linux distribution details |
| `cat /etc/os-release` | `cat /etc/os-release` | View OS version info |
| `uptime` | `uptime` | Show how long the system has been running |
| `date` | `date` | Show the current date and time |
| `timedatectl` | `timedatectl` | Show and set system time/timezone |
| `cal` | `cal` | Display a calendar |

```bash
uname -a                             # Linux hostname 5.15.0 x86_64 GNU/Linux
lsb_release -a                       # Ubuntu 22.04.1 LTS
uptime                               # up 3 days, 4:21, load average: 0.10
date                                 # Fri Nov 21 09:00:00 IST 2026
```

### Memory and CPU Info

| Command | Example | Description |
|---------|---------|-------------|
| `free -h` | `free -h` | Show RAM and swap usage (human-readable) |
| `vmstat` | `vmstat` | Report memory, processes, and I/O stats |
| `nproc` | `nproc` | Show number of CPU cores available |
| `lscpu` | `lscpu` | Detailed CPU architecture info |
| `cat /proc/cpuinfo` | `cat /proc/cpuinfo` | Raw CPU info from the kernel |
| `cat /proc/meminfo` | `cat /proc/meminfo` | Raw memory info from the kernel |

```bash
free -h                              # total/used/free RAM and swap
lscpu                                # CPU model, cores, threads
nproc                                # e.g., 4
```

### Environment and Variables

| Command | Example | Description |
|---------|---------|-------------|
| `env` | `env` | Show all environment variables |
| `printenv` | `printenv PATH` | Print a specific environment variable |
| `export` | `export MYVAR=hello` | Set and export an environment variable |
| `echo $VAR` | `echo $HOME` | Print the value of a variable |
| `unset` | `unset MYVAR` | Remove a variable |
| `alias` | `alias ll='ls -la'` | Create a command shortcut |
| `unalias` | `unalias ll` | Remove an alias |

```bash
export PATH="$PATH:/opt/mytools"     # add to PATH
echo $HOME                           # /home/student
alias gs='git status'                # shortcut for git status
env | grep JAVA                      # check Java env vars
```

---

## 10. Package Management

### Ubuntu / Debian — `apt`

| Command | Example | Description |
|---------|---------|-------------|
| `apt update` | `sudo apt update` | Refresh the list of available packages |
| `apt upgrade` | `sudo apt upgrade` | Upgrade all installed packages |
| `apt install` | `sudo apt install git` | Install a package |
| `apt remove` | `sudo apt remove git` | Remove a package (keep config files) |
| `apt purge` | `sudo apt purge git` | Remove a package and its config files |
| `apt autoremove` | `sudo apt autoremove` | Remove unused dependencies |
| `apt search` | `apt search nodejs` | Search for a package |
| `apt show` | `apt show git` | Show details about a package |
| `dpkg -l` | `dpkg -l` | List all installed packages |
| `dpkg -i` | `sudo dpkg -i package.deb` | Install a local `.deb` file |

```bash
sudo apt update && sudo apt upgrade    # update and upgrade all
sudo apt install curl git vim htop     # install multiple packages
apt search python3                     # find python packages
dpkg -l | grep nginx                   # check if nginx is installed
```

### Fedora / RHEL — `dnf` / `yum`

```bash
sudo dnf update               # update all packages
sudo dnf install git          # install a package
sudo dnf remove git           # remove a package
sudo dnf search nodejs        # search for a package
rpm -qa                       # list all installed RPM packages
```

---

## 11. Archiving & Compression

| Command | Example | Description |
|---------|---------|-------------|
| `tar -cvf` | `tar -cvf archive.tar folder/` | Create a tar archive |
| `tar -xvf` | `tar -xvf archive.tar` | Extract a tar archive |
| `tar -czvf` | `tar -czvf archive.tar.gz folder/` | Create a gzip-compressed tar archive |
| `tar -xzvf` | `tar -xzvf archive.tar.gz` | Extract a gzip-compressed archive |
| `tar -cjvf` | `tar -cjvf archive.tar.bz2 folder/` | Create a bzip2-compressed archive |
| `tar -xjvf` | `tar -xjvf archive.tar.bz2` | Extract a bzip2 archive |
| `tar -tf` | `tar -tf archive.tar` | List contents of a tar archive |
| `gzip` | `gzip file.txt` | Compress a file to file.txt.gz |
| `gunzip` | `gunzip file.txt.gz` | Decompress a .gz file |
| `zip` | `zip archive.zip file1 file2` | Create a zip archive |
| `zip -r` | `zip -r archive.zip folder/` | Zip a folder recursively |
| `unzip` | `unzip archive.zip` | Extract a zip archive |
| `unzip -l` | `unzip -l archive.zip` | List zip contents without extracting |

```bash
# tar flags: c=create, x=extract, v=verbose, f=filename, z=gzip, j=bzip2

tar -czvf backup_$(date +%F).tar.gz project/   # timestamped backup
tar -xzvf backup.tar.gz -C /tmp/               # extract to /tmp
zip -r website.zip public/ -x "*.git*"          # zip, excluding .git
```

---

## 12. Useful Shell Features

### Redirection and Pipes

```bash
echo "Hello" > greeting.txt        # write output to file (overwrites)
echo "Again" >> greeting.txt       # append output to end of file
cat < greeting.txt                 # use file as input to cat
grep "Python" notes.txt | wc -l   # pipe grep output to wc
command 2> error.log               # redirect stderr to file
command > out.txt 2>&1             # redirect both stdout and stderr
command > /dev/null 2>&1           # suppress all output
```

| Symbol | Meaning |
|--------|---------|
| `>` | Redirect stdout to file (overwrite) |
| `>>` | Redirect stdout to file (append) |
| `<` | Use file as stdin |
| `\|` | Pipe stdout of one command to stdin of another |
| `2>` | Redirect stderr |
| `2>&1` | Merge stderr into stdout |
| `/dev/null` | Discard output completely |

### Wildcards (Globbing)

```bash
ls *.txt           # all files ending in .txt
ls file?.txt       # file1.txt, file2.txt (? = exactly one character)
ls file[123].txt   # file1.txt, file2.txt, file3.txt
ls file[a-z].txt   # filea.txt to filez.txt
rm -i temp*.log    # prompt before deleting each matching file
```

### Quoting and Variables

```bash
name="Asha"
echo "Hello, $name"    # double quotes expand variables → Hello, Asha
echo 'Hello, $name'    # single quotes are literal → Hello, $name
echo "Today: $(date)"  # command substitution inside double quotes
```

### Brace Expansion

```bash
echo file{1,2,3}.txt       # file1.txt file2.txt file3.txt
mkdir -p project/{src,tests,docs}   # create multiple dirs at once
cp file.txt{,.bak}          # copy file.txt to file.txt.bak
```

### Command History

```bash
history                      # list all previous commands
history | grep git           # search history for 'git' commands
!!                           # repeat the last command
!50                          # repeat command number 50
!git                         # repeat the most recent 'git' command
Ctrl + R                     # reverse interactive search through history
```

### Job Control

```bash
command &          # run in background
Ctrl + Z           # suspend foreground process
jobs               # list background/suspended jobs
fg %1              # bring job 1 to foreground
bg %1              # resume job 1 in background
nohup command &    # background command that survives logout
```

---

## 13. Shell Scripting Basics

### Basic Script Structure

```bash
#!/bin/bash
# Script name: example.sh
# Description: What this script does
# Author: Your Name
# Date: 2026-11-21

# Your commands go here
echo "Hello from Bash"
```

The first line `#!/bin/bash` is the **shebang** — it tells Linux to use Bash to run the file. Comments begin with `#`.

### How to Create and Run a Script

```bash
nano hello.sh           # create/edit the script file
chmod +x hello.sh       # make it executable (only needed once)
./hello.sh              # run from the current directory
bash hello.sh           # run directly with bash (no chmod needed)
```

### Variables

```bash
#!/bin/bash

name="Alice"              # assign a string
age=25                    # assign a number
result=$((5 + 3))         # arithmetic in $(( ))
greeting="Hello, $name!"  # embed variable in string
today=$(date +%F)         # command substitution

echo $name
echo $age
echo $result
echo $greeting
echo "Today is: $today"
```

### Reading User Input

```bash
#!/bin/bash

echo "What is your name?"
read name                              # read into variable 'name'

read -p "Enter your age: " age        # prompt inline
read -s -p "Enter password: " pass    # -s hides input (silent)

echo "Hello, $name. You are $age years old."
```

### Conditional Statements (if / elif / else)

```bash
#!/bin/bash

read -p "Enter a number: " num

if [ $num -gt 0 ]; then
  echo "$num is positive"
elif [ $num -lt 0 ]; then
  echo "$num is negative"
else
  echo "$num is zero"
fi
```

**Comparison Operators:**

| Numeric | String | Meaning |
|---------|--------|---------|
| `-eq` | `=` or `==` | Equal to |
| `-ne` | `!=` | Not equal |
| `-gt` | `>` | Greater than |
| `-lt` | `<` | Less than |
| `-ge` | `>=` | Greater than or equal |
| `-le` | `<=` | Less than or equal |

**File/Path Tests:**

```bash
if [ -f "$file" ]; then ...    # is it a regular file?
if [ -d "$dir" ]; then ...     # is it a directory?
if [ -e "$path" ]; then ...    # does the path exist?
if [ -r "$file" ]; then ...    # is it readable?
if [ -w "$file" ]; then ...    # is it writable?
if [ -x "$file" ]; then ...    # is it executable?
if [ -s "$file" ]; then ...    # is it non-empty?
if [ -z "$var" ]; then ...     # is the variable empty?
if [ -n "$var" ]; then ...     # is the variable non-empty?
```

### Loops

**for loop:**

```bash
#!/bin/bash

# Loop over a range
for i in {1..5}; do
  echo "Number: $i"
done

# Loop over files
for file in *.txt; do
  echo "File: $file"
done

# C-style for loop
for ((i=1; i<=5; i++)); do
  echo "i = $i"
done
```

**while loop:**

```bash
#!/bin/bash

count=1
while [ $count -le 5 ]; do
  echo "Count: $count"
  ((count++))
done
```

**until loop:**

```bash
#!/bin/bash

count=1
until [ $count -gt 5 ]; do
  echo "Count: $count"
  ((count++))
done
```

**Loop control:**

```bash
break       # exit the loop immediately
continue    # skip to the next iteration
```

### Case Statement

```bash
#!/bin/bash

read -p "Enter a day (Mon/Tue/Wed): " day

case $day in
  Mon)
    echo "Start of the work week!" ;;
  Tue|Wed|Thu)
    echo "Midweek — keep going!" ;;
  Fri)
    echo "Almost the weekend!" ;;
  Sat|Sun)
    echo "Weekend! Relax." ;;
  *)
    echo "Unknown day: $day" ;;
esac
```

### Functions

```bash
#!/bin/bash

# Define a function
greet() {
  local person=$1          # $1 is the first argument; local = scoped
  echo "Hello, $person!"
}

add() {
  local result=$(( $1 + $2 ))
  echo $result             # return value via echo
}

# Call the functions
greet "Alice"
greet "Bob"

sum=$(add 10 20)           # capture returned value
echo "10 + 20 = $sum"
```

### Arrays

```bash
#!/bin/bash

fruits=("apple" "banana" "cherry")

echo ${fruits[0]}           # apple
echo ${fruits[1]}           # banana
echo ${fruits[@]}           # all elements
echo ${#fruits[@]}          # number of elements

# Loop over array
for fruit in "${fruits[@]}"; do
  echo "Fruit: $fruit"
done

# Add element
fruits+=("mango")
echo ${fruits[@]}
```

### Exit Codes and Error Handling

```bash
#!/bin/bash

# Every command returns an exit code: 0 = success, non-zero = error
ls /tmp
echo "Exit code: $?"          # $? holds the last exit code

# Exit the script with a code
cp file.txt backup.txt || { echo "Copy failed!"; exit 1; }

# set -e: exit script immediately if any command fails
set -e

# set -u: treat unset variables as errors
set -u
```

### Script Arguments

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "All arguments: $@"
echo "Number of arguments: $#"

# Example: ./myscript.sh hello world
# $0 = ./myscript.sh
# $1 = hello
# $2 = world
```

---

## 14. Practice Scripts

> Save each script in the shown filename. Run with `bash scriptname.sh` or `chmod +x scriptname.sh && ./scriptname.sh`.

### Script 1 — Greeting with User Input (`greeting.sh`)

Introduces: shebang, `read`, variables, `echo`.

```bash
#!/bin/bash

echo "What is your name?"
read name
read -p "What city are you from? " city
echo "Hello, $name from $city! Welcome to Linux shell scripting."
```

```text
What is your name?
Riya
What city are you from? Mumbai
Hello, Riya from Mumbai! Welcome to Linux shell scripting.
```

---

### Script 2 — Even or Odd Checker (`even_odd.sh`)

Introduces: `if`, arithmetic (`%`), `elif`.

```bash
#!/bin/bash

read -p "Enter an integer: " number

if (( number % 2 == 0 )); then
  echo "$number is even."
else
  echo "$number is odd."
fi

if (( number > 0 )); then
  echo "$number is positive."
elif (( number < 0 )); then
  echo "$number is negative."
else
  echo "$number is zero."
fi
```

---

### Script 3 — Multiplication Table (`table.sh`)

Introduces: `for` loop, arithmetic.

```bash
#!/bin/bash

read -p "Enter a number: " number
read -p "Up to which multiplier? " max

echo "--- Multiplication Table for $number ---"
for (( i=1; i<=max; i++ )); do
  result=$(( number * i ))
  echo "$number x $i = $result"
done
```

---

### Script 4 — File Backup (`backup.sh`)

Introduces: file tests (`-f`), `cp`, date formatting, variables.

```bash
#!/bin/bash

read -p "Enter the filename to back up: " file

if [ -f "$file" ]; then
  mkdir -p backups
  backup_name="backups/${file}.$(date +%F_%H-%M-%S).bak"
  cp "$file" "$backup_name"
  echo "✅ Backup created: $backup_name"
else
  echo "❌ Error: '$file' does not exist or is not a regular file."
  exit 1
fi
```

```bash
# Test it:
echo "My important notes" > notes.txt
bash backup.sh           # enter: notes.txt
ls backups/
```

---

### Script 5 — Count Files in a Folder (`count_files.sh`)

Introduces: directory tests (`-d`), `find`, command substitution, default values.

```bash
#!/bin/bash

read -p "Enter a folder path (press Enter for current folder): " folder

folder=${folder:-.}      # use '.' if no input given

if [ -d "$folder" ]; then
  count=$(find "$folder" -maxdepth 1 -type f | wc -l)
  echo "There are $count file(s) directly inside '$folder'."
else
  echo "❌ Error: '$folder' is not a valid folder."
  exit 1
fi
```

---

### Script 6 — Simple Calculator (`calculator.sh`)

Introduces: `case` statement, arithmetic, functions.

```bash
#!/bin/bash

calculate() {
  local a=$1 op=$2 b=$3
  case $op in
    +) echo "$a + $b = $(( a + b ))" ;;
    -) echo "$a - $b = $(( a - b ))" ;;
    x) echo "$a × $b = $(( a * b ))" ;;
    /) 
      if [ "$b" -eq 0 ]; then
        echo "❌ Error: Cannot divide by zero."
      else
        echo "$a ÷ $b = $(( a / b ))"
      fi
      ;;
    *) echo "❌ Unknown operator: $op" ;;
  esac
}

read -p "Enter first number: " num1
read -p "Enter operator (+, -, x, /): " op
read -p "Enter second number: " num2

calculate $num1 "$op" $num2
```

---

### Script 7 — System Info Report (`sysinfo.sh`)

Introduces: combining many commands, `echo` formatting.

```bash
#!/bin/bash

echo "============================================"
echo "         SYSTEM INFORMATION REPORT         "
echo "============================================"
echo "Hostname       : $(hostname)"
echo "OS             : $(lsb_release -d | cut -f2)"
echo "Kernel         : $(uname -r)"
echo "Architecture   : $(uname -m)"
echo "CPU Cores      : $(nproc)"
echo "RAM (total)    : $(free -h | awk '/^Mem:/{print $2}')"
echo "RAM (used)     : $(free -h | awk '/^Mem:/{print $3}')"
echo "Disk (/)       : $(df -h / | awk 'NR==2{print $4}') free"
echo "Logged in as   : $(whoami)"
echo "Current Time   : $(date '+%Y-%m-%d %H:%M:%S')"
echo "Uptime         : $(uptime -p)"
echo "============================================"
```

---

### Script 8 — Log Error Counter (`log_check.sh`)

Introduces: `grep`, `wc`, function use, file arguments.

```bash
#!/bin/bash

if [ -z "$1" ]; then
  echo "Usage: $0 <logfile>"
  exit 1
fi

logfile="$1"

if [ ! -f "$logfile" ]; then
  echo "❌ File '$logfile' not found."
  exit 1
fi

errors=$(grep -ic "error" "$logfile")
warnings=$(grep -ic "warning" "$logfile")
total=$(wc -l < "$logfile")

echo "Log file: $logfile"
echo "Total lines : $total"
echo "Errors      : $errors"
echo "Warnings    : $warnings"
```

```bash
# Usage:
bash log_check.sh /var/log/syslog
```

---

## 15. Small Exercises

| # | Exercise |
|---|----------|
| 1 | Modify `greeting.sh` to also ask for the user's favourite programming language and print it. |
| 2 | Modify `even_odd.sh` to check whether a number is divisible by 3 and 5. |
| 3 | Modify `table.sh` so the user can choose both the start and end multiplier. |
| 4 | Update `backup.sh` to also print the original file size and backup file size after copying. |
| 5 | Write a script that asks for a filename and prints its lines, words, and characters using `wc`. |
| 6 | Write a script that lists all `.log` files in `/var/log` and shows the size of each. |
| 7 | Write a script that takes two filenames as arguments and merges them into a third file. |
| 8 | Write a script that checks if a website is reachable using `ping` and prints "UP" or "DOWN". |
| 9 | Write a script using a `while` loop that counts down from 10 to 1 and prints "Liftoff!" at the end. |
| 10 | Create a script that accepts a username as argument, checks if the user exists, and prints their home directory from `/etc/passwd`. |

---

## 16. Safety & Good Habits

- ✅ Use `pwd` and `ls` before moving, copying, or deleting files.
- ✅ Always quote file variables: `"$file"` so names with spaces work correctly.
- ✅ Use `rm -i` to confirm before deleting — especially with wildcards.
- ✅ Check with `ls *.log` before running `rm *.log`.
- ✅ Test scripts on sample files before using on real data.
- ✅ Start scripts with `#!/bin/bash`, clear comments, and meaningful variable names.
- ✅ Use `set -e` in scripts so they exit immediately on error.
- ✅ Use `set -u` to catch uninitialized variable mistakes.
- ✅ Avoid `sudo` unless you know exactly why it is needed.
- ✅ Use `man command` or `command --help` when unsure how a command works.
- ✅ Spaces matter: write `name="Sam"` not `name = "Sam"` for variable assignment.
- ✅ `rm -rf` is permanent — Linux has no Recycle Bin in the terminal. Think twice.

---

## Quick Reference — Command Cheat Sheet

```bash
# Navigation
pwd && ls -la && cd /path/to/dir

# File Operations
touch file.txt && mkdir -p a/b/c
cp -r src/ dst/ && mv old.txt new.txt
rm -i file.txt && rm -r folder/

# View Files
cat file.txt && less file.txt
head -n 10 file.txt && tail -f log.txt

# Search
grep -rn "keyword" ./        # recursive search
find . -name "*.py" -type f  # find files
wc -l file.txt               # count lines

# Permissions
chmod 755 script.sh && chmod +x script.sh
sudo chown user:group file.txt

# Processes
ps aux && top
kill -9 PID && killall processname

# Users
whoami && id && who
sudo useradd -m username && sudo passwd username

# Network
ip addr && ping -c 3 google.com
ss -tuln && curl https://example.com

# Disk
df -h && du -sh folder/

# System
uname -a && uptime && free -h

# Archive
tar -czvf archive.tar.gz folder/
tar -xzvf archive.tar.gz

# Packages (Ubuntu/Debian)
sudo apt update && sudo apt install package
```

---

