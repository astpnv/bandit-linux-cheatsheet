# Linux CLI Cheat Sheet

**OverTheWire: Bandit — levels 0 → 15**

My personal command reference from the first 15 levels of [OverTheWire Bandit](https://overthewire.org/wargames/bandit/).  
This is a cheat sheet of commands I actually used, not a walkthrough. No flags, no passwords, no copy-paste solutions.

| | |
|---|---|
| Lab | [bandit.labs.overthewire.org](https://overthewire.org/wargames/bandit/) · port `2220` |
| Progress | Level 0 → 15 |
| Goal | A quick reference for log analysis and server navigation |

Connect:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Password for level 0 is public (`bandit0`). Everything else stays off this repo.

## Commands I used

Examples below show syntax, not the actual level solutions.

| Command | What it does | Example |
|---|---|---|
| `pwd` | Print current directory | `pwd` |
| `cd` | Change directory | `cd inhere` |
| `ls` | List files and folders | `ls -la` |
| `cat` | Print file content | `cat /etc/bandit_pass/bandit14` |
| `file` | Identify file type by headers | `file data.txt` |
| `cp` | Copy files | `cp sshkey.private /tmp/mykey` |
| `mv` | Move or rename files | `mv file1.txt file2.txt` |
| `mkdir` | Create a new directory | `mkdir /tmp/my_folder` |
| `chmod` | Change file permissions | `chmod 600 /tmp/mykey` |
| `find` | Search by owner, size, or name | `find / -user banditX -size 33c 2>/dev/null` |
| `grep` | Search text inside a file | `grep "needle" data.txt` |
| `sort` | Sort lines alphabetically/numerically | `sort data.txt` |
| `uniq` | Filter duplicate adjacent lines | `sort data.txt \| uniq -u` |
| `strings` | Extract readable text from binaries | `strings data.txt` |
| `base64` | Decode / encode base64 | `cat data.txt \| base64 -d` |
| `tr` | Translate or delete characters | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| `xxd` | Create or reverse a hex dump | `xxd -r hexdump > data` |
| `tar` / `gzip` / `bzip2` | Extract / compress archives | `tar -xvf archive.tar` |
| `ssh` | Connect to a remote host | `ssh -i key.priv user@host -p 2220` |
| `nc` | Netcat: raw TCP/UDP connection | `nc localhost 30000` |
| `openssl` | TLS/SSL connection | `openssl s_client -connect localhost:30001` |
| `tee` | Write to a file and standard output | `echo "..." \| sudo tee /etc/resolv.conf` |
| `man` | Open the manual for a command | `man find` |

## What I learned at each stage

No spoilers — just the core skills.

| Levels | Skill | Commands |
|---|---|---|
| 0 | SSH into a host on a non-default port | `ssh -p 2220` |
| 0 → 3 | Read files with awkward names (dashes, spaces) | `ls`, `cat`, `cd` |
| 3 → 4 | View hidden files | `ls -la` |
| 4 → 5 | Identify files by magic bytes, not extensions | `file` |
| 5 → 7 | Find files knowing only owner, group, or exact size | `find`, `2>/dev/null` |
| 7 → 8 | Locate a specific string in a massive text file | `grep` |
| 8 → 9 | Find the only unique line in a file | `sort \| uniq -u` |
| 9 → 10 | Extract readable text from binary junk | `strings` |
| 10 → 11 | Decode base64 | `base64 -d` |
| 11 → 12 | Shift characters (ROT13 cipher) | `tr` |
| 12 → 13 | Reverse a hex dump and extract nested archives | `xxd`, `gzip`, `bzip2`, `tar` |
| 13 → 14 | Authenticate via SSH using a private key | `chmod 600`, `ssh -i` |
| 14 → 15 | Send data to a local port | `nc`, pipe `\|` |
| 15 → 16 | Send data to a local port over TLS | `openssl s_client` |

## Combinations I now type without thinking

```bash
ls -la                          # all files, permissions, hidden
file ./something                # what is this really?
find / -type f -size 33c 2>/dev/null
grep -n "text" file
sort file | uniq -c | sort -nr  # count and rank
strings file | grep -i http
base64 -d encoded.txt
chmod 600 ./id_rsa              # always do this before ssh -i
ssh -i ./id_rsa user@host -p 2220
nc localhost 30000
openssl s_client -connect localhost:30001
```

## Local Toolkit

Commands I used on my own Linux VM while doing the lab.

| Command | What it does | Example |
|---|---|---|
| `ip a` / `ip link` | Check interface status and IPs | `sudo ip link set ens33 up` |
| `dhclient` | Request an IP via DHCP | `sudo dhclient ens33` |
| `ping` | Check if a host is reachable | `ping -c 3 8.8.8.8` |
| `systemctl` | Manage system services | `sudo systemctl restart NetworkManager` |

## My Rules

- No passwords or flags are stored here (per [OverTheWire rules](https://overthewire.org/rules/)).
- Always read `man <command>` before googling for a write-up.
- Appending `2>/dev/null` only hides permission errors, it doesn't magically bypass them.

Anna Stepanova · [astpnv](https://github.com/astpnv) · SOC & GRC trainee
