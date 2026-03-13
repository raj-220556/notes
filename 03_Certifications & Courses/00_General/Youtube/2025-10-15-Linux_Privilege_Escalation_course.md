---
title: Linux Privilege Escalation Course
description: A to Z Course
date: 2025-10-15
categories: [03_Certifications, 00_General, Youtube]
tag: [Linux, Privilege Escalation, courses, Learnings, Youtube, NotCompleted!!!!]
---


 Linux Privilege Escalation Complete Learning
============================================

---


Table of Contents
: 
 01 – Introduction to the Linux Shell <br>
 02 – File System Permissions <br>
 03 – PATH Hijacking <br>
 04 – SUID Exploitation <br>
 05 – SUDO Exploitation <br>
 06 – Wildcard Expansion Exploitation <br>
 07 – Reverse Shells in Linux <br>
 08 – Unshadow Attack <br>
 09 – System Enumeration <br>
 10 – Cronjob Enumeration <br>
 11 – Capabilities Enumeration <br>
 12 – Local Service Exploitation <br>
 13 – Linux Binary Exploitation <br>
 14 – Linux Kernel Exploitation <br>


--- 

---

## 01 – Introduction to the Linux Shell

Table of Contents
: 

1. Example: SSH Connection
2. Terminal, TTY and Bash
3. Basic Information
4. Relative vs Absolute Paths
5. File System Commands
6. Resource Management
.. 1. Example: fdisk output
7. User Management
8. Packages Management
9. Refs

---

### 1.1 Example: SSH Connection

Creating Docker Enviroment for Working.

```text
Dockerfile
  ,----
  | FROM ubuntu:latest
  | 
  | RUN apt-get update && apt-get install -y openssh-server sudo
  | RUN useradd -rm -d /home/sshuser -s /bin/bash -g root -G sudo sshuser
  | RUN echo "sshuser:password" | chpasswd
  | RUN mkdir /var/run/sshd
  | 
  | EXPOSE 22
  | 
  | CMD ["/usr/sbin/sshd", "-D"]
  `----

  Manage docker
  ,----
  | docker build -t ssh-lab .
  | docker run --name ssh-lab --rm -p 22:22 -d ssh-lab
  | docker exec -u root -t -i ssh-lab /bin/bash
  `----

  Run `sftp' docker image
  ,----
  | docker run -p 22:22 -d ssh-lab
  `----

  Suppose we have to connect to an `sftp' server. We can execute the
  following command
  ,----
  | ssh -o "UserKnownHostsFile=/dev/null" sshuser@127.0.0.1
  `----
```

  In order to do this I have implicitly answered the following
  questions:

  1. What **program** do I need to access the server?
  2. What is the **IP address** of the server?
  3. What is the **username**?
  4. What is the **password**?

---

### 1.2 Terminal, TTY and Bash

```text
                 (1)         (2)       (3)
     user <---> xterm <---> tty <---> bash
```

- User input is **converted into GUI events that are captured by xterm**.
- Terminals such as `xterm` visualize output of commands and **pass user input to command-line tools**.
- The `tty` is an **abstraction** that handles the **communication between a terminal and an interpreter**.
- `Bash` is an **implementation** of a command-line interpreter that **executes commands** on the operating system.

---

### 1.3 Basic Information
When using a **terminal**, the *first step* is to understand how to **extract basic information** from the system.

The following command will help in a linux-based system.

- *username*

```bash
$ whoami # username
kali

$ id # userID, groupId, permissions that you have
uid=1000(kali) gid=1000(kali) groups=1000(kali),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),101(netdev),103(scanner),107(bluetooth),124(lpadmin),132(wireshark),134(kaboxer)

```

- *hostname*

```bash
$ hostname   # computer's name on a network 
kali

$ hostname -i   # shows localhost IP.
127.0.1.1

$ hostnmae -I   # shows actual IP of System.
10.106.159.52 2409:40f0:319a:8f13:315:5bf:4b97:7a3a 

```
A `hostname` is a unique name assigned to a device on a network.


- *working directory*

```bash
$ pwd
/home/kali
```

- *environment variables*

```bash
$ env   # prints all current enivronment variables
$ env -i command_to_run # or --ignore-environment it clears entire environment 

$ echo $SHELL # SHELL is an environment variable
/usr/bin/zsh

$ echo $HOME  # HOME is an environment variable
/home/kali

$ echo $USER  # USER is an environment variable
kali

```

- *which (and $PATH)*

```bash
$ which which
/usr/bin/which

$ which burpsuite
/usr/bin/burpsuite

$ which ls
/usr/bin/ls
```
`which` says that where that **program of command stored**.

- *alias*

```bash
$ alias ls
ls='ls --color=auto'
```
`alias` says that **default Execution of command**.

---

### 1.4 Relative vs Absolute Paths

- *absolute path*

```bash
/home/linux/projects/FOUNDATIONS/yt-en/linux-privesc/01-introduction-shell/content/notes.org
```
Path that begins from the root `/`.

- *relative path*

```bash
../../../certs-oscp/full/video/
```
Path that begins from the **current directory** `.`

---

### 1.5 File System Commands

Commands to move in the *File System*.

- *working directory*

```bash
$ pwd   # print working directory
/home/kali
```

- *Change directory*

```bash
$ cd <Abosolute or Relative Path>

$ $ cd Desktop && pwd            
/home/kali/Desktop
```

- *List Files*

```bash
$ ls  # list content of pwd
$ ls -alh # list all files of pwd with all-long-humanReadable format

$ ls -l /etc/passwd  # check the permission of file /etc/passwd
-rw-r--r-- 1 root root 2975 Jul 16 14:50 /etc/passwd
```
It list the content of a Directory.

- *Move Files* or *Rename Files*

```bash
# Moving File To another Directory
mv file.txt /destination/path

# Renaming File old to New
mv old.txt new.txt
```

- *Copy Files*

```bash
# Copying File without removing original file
cp file.txt /destination/path
```

- *Remove Files*

```bash
rm file.txt  
# Removing directory 
rm -dr directory
```
  - `-d` or `--dir` : remove empty directories
  - `-r` or `-R` or `--recursive` : remove directories and their content recursively

---

### 1.6 Resource Management

- *disk devices*

```bash
sudo fdisk -l  # detailed information about all disk partitions
```
It is used Linux like systems for **managing disk partitions**. It allows users to creste, delete, resize and modify partitions on hard drive.

Example: fdisk output
```text
$ sudo fdisk -l backup.img

Disk backup.img: 31.9 GB, 31914983424 bytes, 62333952 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x00009590

 Device Boot          Start         End      Blocks   Id  System
backup.img1            8192     2496093     1243951    e  W95 FAT16 (LBA)
backup.img2         2496094    62333951    29918929    5  Extended
backup.img5         2498560     2564093       32767   83  Linux
backup.img6         2564096     2699263       67584    c  W95 FAT32 (LBA)
backup.img7         2703360    62333951    29815296   83  Linux
```

- *disk usage*

```bash
$ df -h /path/to/directory  # df : disk free
# total, used, and avaliable disk space of the file system

$ du -h 
# disk space used by files/directories
```
`-h` : human readable

- *processes*

processes bounds by controlling terminal
```bash
$ ps
```

view sistem processes
```bash
$ ps aux
```

show hierarchy for every process
```bash
$ ps -axjf
```
read `man ps` for more information.

- *network interfaces*

```bash
# both same shows ip address with netmask of all network intefaces
$ ip address 
$ ip a
```

- *open ports*

display all TCP listening ports, displaying PID/program names and resolve names with IP address

```bash
$ netstat -ltp
```
we can use this for observing services within `localhost`

---

### 1.7 User Management

- Create new user with defaul settings

```bash
$ sudo useradd -m <USERNAME>
```

- Change user password

```bash
$ sudo passwd <USERNAME>
```

- Delete user

```bash
$ sudo userdel -r <USERNAME>
```

- List groups of a given user

```bash
$ groups <USERNAME>
```

- Create new group

```bash
$ groupadd <GROUPNAME>
```

- Add user to group

```bash 
$ usermod -a -G <GROUPNAME> <USERNAME>
```

Two foundamental files related to user management are

- `/etc/passwd', contains useful metadata for users.

```bash
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
ubuntu:x:1000:1000:Ubuntu:/home/ubuntu:/bin/bash
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
systemd-timesync:x:997:997:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:100:101::/nonexistent:/usr/sbin/nologin
systemd-resolve:x:996:996:systemd Resolver:/:/usr/sbin/nologin
sshd:x:101:65534::/run/sshd:/usr/sbin/nologin
sshuser:x:999:0::/home/sshuser:/bin/bash
```

- `/etc/shadow', contains hashed passwords of users.

```bash
root:*:19842:0:99999:7:::
daemon:*:19842:0:99999:7:::
bin:*:19842:0:99999:7:::
sys:*:19842:0:99999:7:::
syntextc:*:19842:0:99999:7:::
games:*:19842:0:99999:7:::
man:*:19842:0:99999:7:::
lp:*:19842:0:99999:7:::
mail:*:19842:0:99999:7:::
news:*:19842:0:99999:7:::
uucp:*:19842:0:99999:7:::
proxy:*:19842:0:99999:7:::
www-data:*:19842:0:99999:7:::
backup:*:19842:0:99999:7:::
list:*:19842:0:99999:7:::
irc:*:19842:0:99999:7:::
_apt:*:19842:0:99999:7:::
nobody:*:19842:0:99999:7:::
ubuntu:!:19842:0:99999:7:::
systemd-network:!*:19869::::::
systemd-timesync:!*:19869::::::
messagebus:!:19869::::::
systemd-resolve:!*:19869::::::
sshd:!:19869::::::
sshuser:$y$j9T$OeC1gyHTe5zm5WKfFyzIN/$Ka2yBHIvDV6km05stxfMM.51OTzJdcu0NLIW5QxCQ43:19869::::::
```

---

### 1.8 Packages Management

In order to manage sytem packages we can use `apt` or `apt-get`.

- Install

```bash
$ sudo apt-get install fdisk
$ sudo apt install fdisk
```

- Search

```bash
$ apt search disk
```

- Remove

```bash
$ apt-get purge fdisk
```

- Update

Download package lists from upstream repositories and updates metadata.

```bash
$ apt-get update
```

- Upgrade

Fetch new versions of packages.

```bash
$ apt-get upgrade
```

---

### 1.9 Refs

  - <https://kevroletin.github.io/terminal/2021/12/11/how-terminal-works-in.html>
  - <https://www.linusakesson.net/programming/tty/>
  - <https://overthewire.org/wargames/>


---



---




## 02 – File System Permissions

Table of Contents
: 
1. MAN pages
2. File System Permissions
3. Reading File Permissions
4. Updating File Permissions
5. SUID and GUID
6. Sudo

---

### 2.1 MAN pages

The command `man` (short for "manual page") is used for accessing local documentation of common linux commands.

```bash
$ man ls
$ man ps
$ man df
$ man du
```

You can either download it locally or you can browser the online version at the following URL
-  <https://man7.org/linux/man-pages/>


### 2.2 File System Permissions

Suppose we list out the files in a given directory with `ls`

```bash
$ ls -lha
drwxr-xr-x  3 linux  users 100 28 mag 20.03 .
drwxrwxrwt 18 root root  440 28 mag 20.03 ..
drwxr-xr-x  2 linux  users  40 28 mag 20.02 dir
-rwxr-xr-x  1 linux  users   0 28 mag 20.03 exec.sh
-rw-r--r--  1 linux  users   6 28 mag 20.02 test.txt
```

For each file we have the following information regarding permissions

- File permissions `(-rwxr-xr-x)`
- User that owns the file `(linux)`
- Group that owns the file `(users)`

Within each string of the form

```bash
drwxrwxrwt
drwxr-xr-x
-rw-r--r--
```

we find 10 letters, which can be either set or not set:

```
  • The first specifies the `type of the file`.
    • Regular files -> "-"
    • Directories -> "d"
    • Links -> "l"
```

  • Then we find three groups of three letters each.

    These specify the permissions for three types of users:

    • The first group specifies the permissions for the user who is also
      the owner of the file.

    • The second group specifies the permissions for the user who
      belongs to the group that owns the file.

    • The third group specifies the permissions for all other
      users. That is, for users who do not belong in the group that owns
      the file and are not the owner of the file themselves.

  • Each group works in the same way.

    We have three distinct types of permissions:

    • Read permission (r)
    • Write permission (w)
    • Execute permission (x)

    There are minor variants in this. For example, when setting the SUID
    bit on an executable, the execute permission will be written as `s'
    instead of `x'.

    ┌────
    │ -rwsr-sr-x  1 linux  users   0 28 mag 20.03 exec.sh
    └────

---

### 2.3 Reading File Permissions


File permissions can be read with the `ls` command.

  Consider the following output

```bash
$ ls -lha
drwxr-xr-x  3 linux  users 100 28 mag 20.03 .
drwxrwxrwx 18 root root  440 28 mag 20.03 ..
drwxr-xr-x  2 linux  users  40 28 mag 20.02 dir
-rwxr-xr-x  1 linux  users   0 28 mag 20.03 exec.sh
-rw-r--r--  1 linux  users   6 28 mag 20.02 test.txt
```

  We can analyze the output line by line:

  • The first line is referring to the current directory ( . ).

    It is saying that:

    • It is a directory (d)
    • The user `linux' can read (r), write (w) and execute (x) on the
      directory.
    • Users beloging to the group `users' can read (r) and execute (x)
      but not write.
    • Any other user can read (r) and execute (x) but not write.

  • The second line is referring to the previous directory ( .. ).

    It is saying that:

    • It is a directory (d)
    • The user `root' can read (r), write (w) and execute (x) on the
      directory.
    • Users beloging to the group `root' can read (r), write (w) and
      execute (x) on the directory.
    • Any other users can read (r), write (x) and execute (x) on the
      directory.

  • The last line is referring to the resource test.txt.

    It is saying that:

    • It is a regular file (-)
    • The user `linux' can read (r), write(r), but not execute (x) the
      file.
    • Users belonging to the group `users' can only read (r) the file.
    • Any other users can also read (r) the file.

--- 

#### *Special Cases*

  Sometimes it can happen to see for the last position the value of `t`
  instead of the value of `x`. This is called the "**sticky bit**". It
  implies that users in the group `root` who technically have the
  *permissions to delete a file* (write), cannot do it as long as the
  sticky bit is set.

  Other times instead we see the value of `s` instead of the value of
  `x`. This instead has to do with the `SUID` and `GUID` permissions,
  which will be discussed later.

---

### 2.4 Updating File Permissions


The command that can be used for updating the permission on a given file is the `chmod` command.

Let's assume to start with the following permission

```bash
$ ls -lh
-rw-r--r-- 1 linux users   0 28 mag 20.26 exec.sh
```

  It can be used as follows:

• Add *execute* `x` *for all*
: 
```bash
$ chmod +x exec.sh
$ ls -lh
-rwxr-xr-x 1 linux users   0 28 mag 20.27 exec.sh
```

• Remove *read* `r` and *execute* `x` for *other users*
: 
```bash
$ chmod o-xr exec.sh 
$ ls -lh
-rwxr-x--- 1 linux users   0 28 mag 20.27 exec.sh
```

• Add *write* `w` for *group* that owns the file
: 
```bash
$ chmod g+w exec.sh
$ ls -lh
-rwxrwx--- 1 linux users   0 28 mag 20.27 exec.sh
```

• If we want to make a change in an *entire subfolder*, we have to use the *recursive* `-R` option
: 
```bash
$ chmod -R o-rwx directory
```

--- 

To **change the owner** of the file we can use the `chown` command.

```bash
$ chown root test.sh
```

If you want to **change the group** you can use the syntax with the `:`.

```bash
$ chown <USER>:<GROUP> test.sh
$ chown root:root test.sh
$ ls -lh
$ -rwxrwxr-- 1 root root    0  2 giu 12.34 test.sh
```

---

  Instead of using the identifiers `ugoa` with `rwx`, it is also
  possible to set permissions using a **sequence of three numbers**. This is
  because `read `(r), `write` (w) and `execute` (x) permissions can be
  `represented using three bits`.

  Given that with three bits we can also represent the numbers from `0`
  to `7`, this means that each number can represent a specific set of
  permissions.

  Specifically, we have the following association between bits,
  permissions and numbers.

 ```
    0  ->  000  ->  ---
    1  ->  001  ->  --x
    2  ->  010  ->  -w-
    3  ->  011  ->  -wx
    4  ->  100  ->  r--
    5  ->  101  ->  r-x
    6  ->  110  ->  rw-
    7  ->  111  ->  rwx
```

  This means for example that `5` represents the read (r) and execute
  (x) permission bits, while `7` represent the entire read (r), write
  (w) and execute (x) bits.

  Remember that we have three groups of users. This means that to fully
  specify the basic permissions for a file we will **need three numbers**,
  each going from `0` to `7`.

  • Set `rwx` for owner, `rw-` for group and `r--` for others

```bash
$ chmod 764 exec.sh
$ ls -lh
-rwxrw-r-- 1 linux users   0 28 mag 20.27 exec.sh
```

  • Set `rw-` for owner, `r-`- for group and `-` nothing for others

```bash
$ chmod 640 exec.sh
```


### 2.5 SUID and GUID

Executable files can have special permissions called `SUID` and `GUID`.

`SUID` stands for `Set User ID`.

Binaries that have this bit *allow the binary to change user permission during its execution*.

  Specifically, it **allows the program to be executed by anyone**. Then,
  during its execution, it will **change its permission and take the permissions of the owner of the file**.

  For example, consider the `passwd` program

```bash
$ ls -lh /usr/bin/passwd
-rwsr-xr-x 1 root root 80K  1 apr 12.19 /usr/bin/passwd
```

  The executable flag is not `x` but rather `s`. This is because the
  program has the `SUID` bit set. This means that durings its execution,
  at specific times, the program will change its roles in order **to become root**.

  This change of permissions is done by using the function `setuid`
  offered by the `standard C library`. It is necessary since the `shadow`
  file is owned by `root` and only `root` can edit it.

  <github.com/shadow-maint/passwd.c>

```c
if (setuid (0) != 0) { // if changes can`t possibile
  (void) fputs (_("Cannot change ID to root.\n"), stderr);
  SYSLOG ((LOG_ERR, "can't setuid(0)"));
  closelog ();
  exit (E_NOPERM);
 }
if (spw_file_present ()) {
  update_shadow ();
 } else {
  update_noshadow ();
 }
```

```bash
$ ls -lh /etc/shadow
-rw------- 1 root root 1,3K 26 mag 17.19 /etc/shadow
```

  To set a SUID bit we can use `chmod`

```bash
$ chmod u+s exec.sh
$ ls -lh
-rwsr-x--x 1 linux users   0 28 mag 20.27 exec.sh
```                                 

  ――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――――

  The same idea of `SUID' also applies to `GUID'.

  This time however instead of using the file owner permission, the
  program during its execution is allowed to obtain the permission of
  the group that owns the file.

  To set a GUID bit we can use `chmod'

  ┌────
  │ $ chmod g+s exec.sh
  │ $ ls -lh
  │ -rwxr-s--x 1 linux users 0 28 mag 20.27 exec.sh
  └────


[github.com/shadow-maint/passwd.c](https://github.com/shadow-maint/shadow/blob/58b6e97a9eef866e9e479fb781aaaf59fb11ef36/src/passwd.c)


6 Sudo
══════

  Finally, an important subystem that relates to permissions is the
  `sudo' functionality.

  ┌────
  │ sudo -> SuperUserDO
  └────

  The sudo utility allows to change user for the execution of specific
  commands. Once it is installed, we can check the way we can use it
  with `sudo -l'

  ┌────
  │ Matching Defaults entries for homer on canape:
  │ env_reset, mail_badpass,
  │ secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
  │ 
  │ User homer may run the following commands on canape:
  │ (root) /usr/bin/pip install *
  └────

  ┌────
  │ env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin
  │ 
  │ User richard may run the following commands on stratosphere:                                           
  │ (ALL) NOPASSWD: /usr/bin/python* /home/richard/test.py
  └────

  ┌────
  │ Matching Defaults entries for genevieve on dab:
  │ env_reset, mail_badpass,
  │ secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
  │ 
  │ User genevieve may run the following commands on dab:
  │ (root) /usr/bin/try_harder
  └────

  ┌────
  │ User dorthi may run the following commands on Oz:
  │ (ALL) NOPASSWD: /usr/bin/docker network inspect *
  │ (ALL) NOPASSWD: /usr/bin/docker network ls
  └────

  ┌────
  │ Matching Defaults entries for www-data on bashed:
  │ env_reset, mail_badpass,
  │ secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
  │ 
  │ User www-data may run the following commands on bashed:
  │ (scriptmanager : scriptmanager) NOPASSWD: ALL
  └────

  To use sudo we can do

  ┌────
  │ sudo -u scriptmanager python3 -c 'import pty; pty.spawn("/bin/bash")'
  └────

  The configuration file for sudo is found in `/etc/sudoers'.


## 03 – PATH Hijacking



## 04 – SUID Exploitation



## 05 – SUDO Exploitation



## 06 – Wildcard Expansion Exploitation


## 07 – Reverse Shells in Linux



## 08 – Unshadow Attack



## 09 – System Enumeration



##  10 – Cronjob Enumeration



##  11 – Capabilities Enumeration



##  12 – Local Service Exploitation



##  13 – Linux Binary Exploitation



##  14 – Linux Kernel Exploitation


