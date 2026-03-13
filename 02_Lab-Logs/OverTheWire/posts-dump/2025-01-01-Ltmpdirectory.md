---
title: /tmp directory in Linux
description: The /tmp directory in Linux is one of those places every process uses.
date: 2025-01-01
categories: 
tag: [/tmp, Linux, theory, Learnings]
---


## 📌 What is /tmp in Linux?

- `/tmp` = **temporary directory** in Unix/Linux.
- It is used to store **temporary files** created by:
  - Programs
  - System processes
  - Users
- Files in `/tmp` are **not meant to be permanent**.
- Typically, they are **deleted automatically**:
  - On **system reboot** (most distros)
  - Or after a **timeout** using `systemd-tmpfiles`, `tmpwatch`, or `tmpreaper`

## 📌 Key Characteristics

1. World-writable
: 
Anyone can write files in `/tmp`.
- Permissions are usually `drwxrwxrwt`
- The `t` (**sticky bit**) means:
  - Users can only delete their **own files**, not others’ (even if world-writable).
```bash
ls -ld /tmp
drwxrwxrwt  20 root root  4096 Sep 29 10:00 /tmp
```

1. Volatile storage
: 
- Files are temporary.
- Don’t store important data here.

1. Accessible by all processes
: 
- Programs use `/tmp` for sockets, locks, caches, and temp storage.

## 📌 Examples of /tmp usage

- When you **unpack an archive** in a GUI, temp files are written to `/tmp`.
- Applications like browsers store **session data / downloads-in-progress** in `/tmp`.
- Editors like **vim** create swap files in `/tmp`.

## 📌 Cleaning of /tmp

- On **system reboot**, many Linux distros clean `/tmp`.
- On **systemd-based systems**:
  - `/tmp` is managed by `systemd-tmpfiles`.
  - Config: `/usr/lib/tmpfiles.d/tmp.conf`
  - Example: delete files not accessed for 10 days.

Check cleanup policy:
```bash
systemctl status systemd-tmpfiles-clean.timer
```

## 📌 Security Considerations

- Since `/tmp` is world-writable, it is a common target for **symlink attacks** or **privilege escalation**.
- Mitigations:
  - Use **sticky bit** (default).
  - Mount `/tmp` with `noexec`, `nosuid`, `nodev` options (in `/etc/fstab`) for extra safety.

Example:
```bash
tmpfs /tmp tmpfs defaults,noexec,nosuid,nodev 0 0
```

## 📌 Difference between /tmp and /var/tmp

- `/tmp`: 
  - For **short-lived** files.
  - Usually cleared on reboot.

- `/var/tmp`: 
  - For **longer-lived temporary files**.
  - Survives across reboots.

Example: 
```bash
echo "hello" > /tmp/myfile.txt
echo "hello" > /var/tmp/myfile.txt
```
After reboot: `/tmp/myfile.txt` → **gone**, `/var/tmp/myfile.txt` → **still there**.



## 📌 Commands To explore /tmp

🔍 1. Check /tmp permissions
: 
```bash
ls -ld /tmp
```
You should see something like:
```
drwxrwxrwt 20 root root 4096 Sep 29 14:00 /tmp
```
- `rwxrwxrwt` → world-writable + sticky bit (`t`).

🔍 2. See what’s inside `/tmp`
: 
```bash
ls -lh /tmp
```
This shows current temporary files (may belong to apps, browsers, editors, etc.).

🔍 3. Check who owns what in `/tmp`
: 
```bash
ls -lh /tmp | grep $USER
```
This lists only your files inside `/tmp`.

🔍 4. See cleanup policy (systemd systems)
: 
```bash
systemctl status systemd-tmpfiles-clean.timer
```
It shows how `/tmp` cleanup is scheduled (e.g., every 1 day).<br>
Check detailed rules: 
```bash
cat /usr/lib/tmpfiles.d/tmp.conf
```
Example content: 
```bash
# Clear /tmp files not accessed for 10 days
d /tmp 1777 root root 10d
```

🔍 5. Check which processes are using `/tmp`
: 
```bash
lsof +D /tmp
```
Shows open files under `/tmp`.

🔍 6. Compare with `/var/tmp`
: 
```bash
ls -ld /var/tmp
```
Notice it also has `1777` perms, but its files **survive reboots**.


## 📌 Summary

- `/tmp` is **temporary storage**, auto-cleaned, world-writable with sticky bit.
- Use it for **non-critical temporary files** only.
- Use `/var/tmp` for temp files that must survive reboots.
- System admins often mount `/tmp` with restrictions (`noexec`, `nosuid`) for security.
