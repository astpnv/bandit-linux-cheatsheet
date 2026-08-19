# Linux CLI Cheat Sheet

**OverTheWire: Bandit — levels 0 → 15**

Personal command reference from the first 15 levels of [OverTheWire Bandit](https://overthewire.org/wargames/bandit/).  
This is a **cheat sheet of commands I actually used**, not a walkthrough. No flags, no passwords, no copy-paste solutions.


|---|---|
| Lab | [bandit.labs.overthewire.org](https://overthewire.org/wargames/bandit/) · port `2220` |
| Progress | Level 0 → 15 |
| Goal | Keep the commands in one place so I can reuse them on real logs and hosts |

Connect:

```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Password for level 0 is public (`bandit0`). Everything after that stays off this repo.

---

## Commands I used

The table I kept while playing. Examples are generic — they show *how the tool is typed*, not how a level is solved.

| Command | What it does | How I typed it |
|---|---|---|
| `ls` | List files and folders | `ls -la` |
| `cat` | Print a file | `cat /etc/bandit_pass/bandit14` |
| `file` | Identify the real type of a file | `file data.txt` |
| `cp` | Copy | `cp sshkey.private /tmp/mykey` |
| `mv` | Move or rename | `mv file1.txt file2.txt` |
| `mkdir` | Create a folder | `mkdir /tmp/my_folder` |
| `chmod` | Change permissions | `chmod 600 /tmp/mykey` |
| `tar` / `gzip` / `bzip2` | Pack / unpack archives | `tar -xvf archive.tar` |
| `ssh` | Remote shell | `ssh -i key.priv user@host -p 2220` |
| `nc` (netcat) | Raw TCP connection | `nc localhost 30000` |
| `openssl s_client` | TLS / SSL connection | `openssl s_client -connect localhost:30001` |
| `tee` | Write to a file and still see the output | `echo "..." \| sudo tee /etc/resolv.conf` |
| `\|` (pipe) | Send output of one command into the next | `cat pass.txt \| nc localhost 30000` |

These showed up on the same levels even if they were not in my first notes:

| Command | What it does | How I typed it |
|---|---|---|
| `pwd` | Where am I | `pwd` |
| `cd` | Change directory | `cd inhere` |
| `find` | Search the filesystem by owner, size, name | `find / -user banditX -group banditY -size 33c 2>/dev/null` |
| `grep` | Search inside a file | `grep "needle" data.txt` |
| `sort` | Sort lines | `sort data.txt` |
| `uniq` | Drop duplicate **adjacent** lines (`sort` first) | `sort data.txt \| uniq -u` |
| `strings` | Pull readable text out of a binary | `strings data.txt` |
| `base64 -d` | Decode base64 | `cat data.txt \| base64 -d` |
| `tr` | Translate characters (e.g. ROT13) | `tr 'A-Za-z' 'N-ZA-Mn-za-m'` |
| `xxd` | Hex dump / reverse hex dump | `xxd -r hexdump > data` |
| `man` | Manual for any command | `man find` |

---

## What each block of levels trained

No solutions — only the *skill* and the *tool*. Official pages: [Bandit](https://overthewire.org/wargames/bandit/).

| Levels | Skill | Commands |
|---|---|---|
| 0 | Log into a remote host over SSH on a non-default port | `ssh -p 2220` |
| 0 → 3 | Read files, including names that are awkward (`-`, spaces) | `ls`, `cat`, `cd` |
| 3 → 4 | See hidden (dot) files | `ls -la` |
| 4 → 5 | Trust magic bytes, not the filename | `file` |
| 5 → 7 | Find a file when you only know owner / group / size | `find` , `2>/dev/null` |
| 7 → 8 | Find one line inside a large file | `grep` |
| 8 → 9 | The line that appears only once | `sort \| uniq -u` |
| 9 → 10 | Readable strings inside binary junk | `strings` |
| 10 → 11 | Decode, don't guess | `base64 -d` |
| 11 → 12 | Transform a cipher one character at a time | `tr` |
| 12 → 13 | Hex file + several layers of compression | `xxd`, `file`, `gzip`, `bzip2`, `tar` |
| 13 → 14 | SSH with a private key | `chmod 600`, `ssh -i` |
| 14 → 15 | Talk to a service on localhost | `nc` , pipe |
| 15 → 16 | Same idea, but the port speaks TLS | `openssl s_client -connect` |

---

## Flags I now type without thinking

```bash
ls -la                          # all files, permissions, hidden
file ./something                # what is this really?
find / -type f -size 33c 2>/dev/null
grep -n "text" file
sort file | uniq -c | sort -nr  # count + rank (uniq needs sort first)
strings file | grep -i http
base64 -d encoded.txt
chmod 600 ./id_rsa              # always, before ssh -i
ssh -i ./id_rsa user@host -p 2220
nc localhost 30000
openssl s_client -connect localhost:30001
```

---

## Also in my toolkit (not from Bandit)

Used on my own Linux / WSL box while I was doing the lab. Kept here so the sheet matches what I actually type.

| Command | What it does | How I typed it |
|---|---|---|
| `ip a` / `ip link` | Addresses and interface state | `sudo ip link set ens33 up` |
| `dhclient` | Ask DHCP for an IP | `sudo dhclient ens33` |
| `ping` | Is the host alive | `ping -c 3 8.8.8.8` |
| `systemctl` | Start / stop / status of a service | `sudo systemctl restart NetworkManager` |

---

## Rules I kept

- Passwords and flags are **not** in this repo. That is the [OverTheWire honor code](https://overthewire.org/rules/).
- I read `man <command>` before I googled a write-up.
- `2>/dev/null` only hides errors. It does not make a failed `find` succeed.

Lab: [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)

*Anna Stepanova · [astpnv](https://github.com/astpnv) · SOC & GRC trainee*
