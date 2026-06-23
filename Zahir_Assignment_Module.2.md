 
 
# LINUX OPERATIONS
## Software and package management
### Package managers and repositories:
#### 1. Package Manager:
   A **package manager** is a tool that installs, updates, removes, and manages software  automatically.
   Common linux package manager is:
   **APT (Advanced Package Tool)** – the most common command-line package manager use for:
   Install software
   Update package list
   Upgrade installed software
   Remove packages
#### 2. Repository:
   A **repository (repo)** is an online storage location that contains software packages.
   Linux downloads software from repositories.
   Example:
   User → Package Manager → Repository → Software Installed
  So.
   **Repository** = place where software packages are stored.
   **Package Manager (APT)** = tool that gets software from repositories and manages it on your  system.
  
### Installing, updating and removing packages
#### 1. Installing Packages
   **Install a Package (VLC media player):**
   ==inara@zahir-30:~$ sudo apt install vlc==
   **Install multiple Packages:**
   ==inara@zahir-30:~$ sudo apt install git firefox curl==
#### 2. Update Package Information
   ==inara@zahir-30:~$ sudo apt update==
#### 3. Upgrade Installed Packages
   ==inara@zahir-30:~$ sudo apt upgrade==
#### 4. Remove a package
   ==inara@zahir-30:~$ sudo apt remove vlc==
   For Complete Removel of package alongside Configration files:
   ==inara@zahir-30:~$ sudo apt purge vlc==
   For unused dependencies removel:
   ==inara@zahir-30:~$ sudo apt autoremove==

### Querying installed software
#### 1. List all installed packages
   ==inara@zahir-30:~$ dpkg -l==
#### 2. Search for a specific package
   ==inara@zahir-30:~$ dpkg -l | grep git== 
#### 3. Lists all packages installed through APT
   ==inara@zahir-30:~$ apt list --installed==
#### 4. Check if a package is installed
   ==inara@zahir-30:~$ dpkg -s git==
#### 5. Search for Available Packages
   ==inara@zahir-30:~$ apt search nginx==
#### 6. Check Package Location
   ==inara@zahir-30:~$ which git==
   ==/usr/bin/git==
#### 7. To see files that got installed because of a package:
   dpkg -S rsync
   ==inara@zahir-30:~$ dpkg -S rsync==
#### 8. To see a certain file got installed because of what package
   dpkg -S /usr/share/doc/rysnc
     
   
### Managing repository sources
   **Repository sources** are the places from which your system downloads software and updates.
   Main file:  /etc/apt/sources.list
   Additional sources:  /etc/apt/sources.list.d/  
#### 1. View repository sources
   ==inara@zahir-30:~$ cat /etc/apt/sources.list== 
#### 2. Add a repository
   ==inara@zahir-30:~$ sudo add-apt-repository ppa:repository-name==
   ==inara@zahir-30:~$ sudo apt update== 
#### 3. Remove a repository
   ==inara@zahir-30:~$ sudo add-apt-reposiotry --remove ppa:repository-name==
   ==inara@zahir-30:~$ sudo apt update== 
#### 4. Edit repository sources manually
   ==inara@zahir-30:~$ sudo nano /etc/apt/sources.list==

## Services and Systemd

###  The service concept 
####    Service
   A **service** is a program that runs in the background and provides functionality to the system or other programs.
   Examples:
     SSH service  -> allows remote login
     Apache/Nginx service -> serves websites
     MySQL service -> manage databases
####   Systemd
   Systemd act as manager that manages the system services. It is responsible for:
   - Starting services during boot.
   -  Stopping, restarting, and monitoring services.
   -  Managing system startup and dependencies.
### Unit files and their structure
####  Unit File
  A **unit file** is a configuration file used by **systemd** to define how a service, timer, mount point, or other system resource should behave. 
  It is an instructions sheet that tell systemd:
- What to start
- How to start it
- When to start it
 Unit Files are usually stored in"
  /etc/systemd/system/myservice.service
  /lib/systemd/system/myservice.service
  To View:
  ==inara@zahir-30:~$ cat /lib/systemd/system/ssh.service==
#### Structure of a Service Unit File
Example:
[Unit]  
Description=My Sample Service  
After=network.target  
  
[Service]  
ExecStart=/usr/bin/python3 /home/user/app.py  
Restart=always  
  
[Install]  
WantedBy=multi-user.target

##### Sections Explain:
[Unit]
General information and dependencies.
[Service]
Defines how the service runs
[Install]
Controls when the service is enabled.
#### Common Unit File Types:
.service -> Background service
.socket -> Socket activation
.timer -> Scheduled task (like corn)
.target -> Group of units / system state

 Examples:
 - ssh.service
- cron.service
- backup.timer
### Starting, stopping and enabling services
####  Start a service:
  sudo systemctl start nginx
  ==inara@zahir-30:~$ sudo systemctl start nginx==
####  Stop a service:
  sudo systemctl stop nginx
  ==inara@zahir-30:~$ sudo systemctl stop nginx==
####   Restart a service:
   sudo systemctl restart nginx
   ==inara@zahir-30:~$ sudo systemctl restart nginx==
####   Enable service at boot:
   sudo systemctl enable nginx
   ==inara@zahir-30:~$ sudo systemctl enable nginx==
####   Disable service at boot:
   sudo systemctl disable nginx
   ==inara@zahir-30:~$ sudo systemctl disable nginx==
### Reading service status
####  Check service status:
   systemctl status ssh
   ==inara@zahir-30:~$ sudo systemctl status ssh==
####  Check only whether a service is running:
   systemctl is-active ssh
   ==inara@zahir-30:~$ sudo systemctl is-active ssh==
####  Check whether it starts at boot:
   systemctl is-enabled ssh
   ==inara@zahir-30:~$ sudo systemctl is-enabled ssh==
## Process management
   To Monitor, Control and manage running programs(processes) is called process management.
###   Viewing running processes
   ps aux
   ==inara@zahir-30:~$ sudo ps aux==
   Shows all running processes.
####      Monitor processes in real time
   top 
   ==inara@zahir-30:~$ top==
   or
     htop
   ==inara@zahir-30:~$ htop==
   Displays CPU and memory usage of running processes
### Foreground and background jobs
#### Start a process in the background
   command &
   ex: firefox &
   ==inara@zahir-30:~$ sleep 60 &==
    ==inara@zahir-30:~$ sleep 90 &==
#### List background jobs(processes)
   jobs
   ==inara@zahir-30:~$ jobs==
#### Bring a background job(process) to the foreground
  fg
  ==inara@zahir-30:~$ fg==  -> Brings back the most recent job
  ==inara@zahir-30:~$ fg %2== -> Brings back Job ID number 1
#### Send a job(process) to background
   CTRL+Z  -> to pause it then
   bg
   ==inara@zahir-30:~$ bg==
### Signals and terminating processes
 A **signal** is a notification sent to a process to tell it to perform a specific action, such as stopping, pausing, or restarting.
#### Find a process
   pgrep nginx
   ==inara@zahir-30:~$ pgrep nginx==
   Shows the process ID (PID) of nginx.
   or 
   pgrep -l nginx
To Find Master PID: 
==inara@zahir-30:~$ ps -ef | grep nginx==
#### Stop a process
   kill PID -> Master PID
   ex: kill 1234
   or ctl+c
   ==inara@zahir-30:~$ sudo kill 23567==
#### Force stop a process
   kill -9 PID
   ==inara@zahir-30:~$ sudo kill -9 23567==
#### Terminating by Process Name
   pkill process_name
   ==inara@zahir-30:~$ pkill nginx==
#### Pause a Process
   kill -STOP PID
   ==inara@zahir-30:~$ sudo kill -STOP 23567== 
   or
   ==inara@zahir-30:~$ sudo kill -19 23567==
  or ctrl+z
#### Resume a Process
   kill -CONT PID
   ==inara@zahir-30:~$ sudo kill -CONT 23567==
   or
   ==inara@zahir-30:~$ sudo kill -18 23567==
#### Common Signals
| Signal  | Number | Description                       |
| ------- | ------ | --------------------------------- |
| SIGTERM | 15     | Gracefully terminates a process   |
| SIGKILL | 9      | Forcefully terminates a process   |
| SIGINT  | 2      | Interrupts a process (`Ctrl + C`) |
| SIGSTOP | 19     | Pauses a process                  |
| SIGCONT | 18     | Resumes a paused process          |
### Priorities and resource usage
####   Process Priority
   Process priority determines how much **CPU time** a process gets compared to other processes.
   - Linux uses a **nice value** to set priority.
- Range: **-20 to 19**
    - **-20** = Highest priority
    - **19** = Lowest priority
- Default nice value is **0**
####  Viewing Process Priority
   ps -el  
   ==inara@zahir-30:~$ ps -el==
   or
   top
   These commands show the **NI (nice value)** and process details.
#### Starting a Process with a Specific Priority
  **Syntax:**
     nice -n {priority} {command}
  ex: nice -n 10 firefox   -> if process is not running
  Starts Firefox with a lower priority.
#### Changing Priority of a Running Process
  renice -n 5 -p PID
  ex:
    renice -n 5 -p 1234
   Changes the nice value of process 1234 to 5.
==inara@zahir-30:~$ sudo renice -n 2 -p 23567==     -> if process is running
#### Resource Usage
  Linux provides tools to monitor CPU, memory, and disk usage.
#####   CPU and Memory Usage
   top
   ==inara@zahir-30:~$ top==
   Shows real-time CPU and memory consumption of processes.
#####   Detailed Process Monitoring
   htop
   ==inara@zahir-30:~$ htop==
   An interactive version of `top` (may need installation).
#####   Memory Usage
   free -h
   ==inara@zahir-30:~$ free -h==
   Displays available and used RAM in a human-readable format.
#####   Disk Usage
   df -h
   ==inara@zahir-30:~$ df -h==
   Shows disk space usage of file systems.
#####   Directory Size
   du -sh folder_name
   ==inara@zahir-30:~$ du -sh Desktop==
   Shows the size of a specific directory.

## Logs with journalctl and the log directory
  Logs are files that record what happens in the system.
  Errors
  Login Successful/Unsuccessful(attempts)
  Monitor Services(start/Stop)
  
Following are the reasons why logs are important:
- You can't find errors without them.
- You can't debug without them.
- You can't check security.
### The system journal
  System journal is a collection of system logs stored by `systemd-journald`.
  Stored in:
#####    Persistent logs (kept after reboot):
      /var/log/journal/
#####    Volatile logs (lost after reboot):
    /run/log/journal/
#### To View all System Logs:
  journalctl
  ==inara@zahir-30:~$ journalctl==
#### To View Latest Logs:
   journalctl -n 20
   ==inara@zahir-30:~$ journalctl -n 20==
   shows last 20 lines of log
### Filtering and following log output

### Filtering
  **Filtering logs** means showing only the log entries you need based on(service, time, priority, etc).
#### Show logs for a specific service:
 journalctl -u nginx 
 ==inara@zahir-30:~$ journalctl -u nginx==
 or
 journalctl -u ssh
#### Show logs since today:
journalctl --since today
==inara@zahir-30:~$ journalctl --since today==
#### Show log  with time filtering
journalctl --since "1 hr ago"
==inara@zahir-30:~$ journalctl --since "1 hr ago"==
#### Show logs from the current boot:
journalctl -b
==inara@zahir-30:~$ journalctl -b==
#### Show only error messages:
journalctl -p err
==inara@zahir-30:~$ journalctl -p err==

### Following Logs
 Following logs means watching new log entries appear in real time (similar to tail, -f).
 continuously watch new log messages as they are generated.
#### Follow all system logs:
journalctl -f
==inara@zahir-30:~$ journalctl -f==
Shows new log entries from the entire system as they happen
#### Follow logs for a specific service:
 journalctl -u ssh -f
 ==inara@zahir-30:~$ journalctl -u ssh -f==
 Shows new logs only from the SSH service
 or 
 journalctl -u nginx -f
 ==inara@zahir-30:~$ journalctl -u nginx -f==
 Useful for monitoring incoming requests and errors.
#### Follow kernel messages:
 journalctl -k -f
 ==inara@zahir-30:~$ journalctl -k -f==
#### Follow logs from the current boot:
 jornalctl -b -f
 ==inara@zahir-30:~$ journalctl -b -f==
 show only log generated  since the system was last started.

**Example:**  Suppose you restart a service and want to see what happens.
       sudo systemctl restart nginx  
       journalctl -u nginx -f
    You will immediately see new messages such as:
       nginx: Starting nginx...  
       nginx: Started nginx.

`-f` = **follow** = keep the log open and display new entries as they arrive.

### Traditional log files
  **Traditional log files** are plain text files stored in the `/var/log/` directory that record system and application events.
  Common Example:
- `/var/log/syslog` – General system messages (on many Linux distributions)
- `/var/log/messages` – General system logs (on some distributions)
- `/var/log/auth.log` – Login and authentication events
- `/var/log/kern.log` – Kernel messages
- `/var/log/cron` – Scheduled task (cron) logs
#### View log directory:
   ls /var/log
   ==inara@zahir-30:~$ ls /var/log==
#### View traditional log files:
  cat /var/log/syslog
  ==inara@zahir-30:~$ cat /var/log/syslog==
Quit with: q
#### View page by page:
  less /var/log/syslog
  ==inara@zahir-30:~$ less /var/log/syslog==
#### View the latest entries:
  tail /var/log/syslog
  ==inara@zahir-30:~$ tail /var/log/syslog==
#### Follow new entries in real time:
  tail -f /var/log/syslog
  ==inara@zahir-30:~$ tail -f /var/log/syslog==
  
Read with tools like `cat`, `less`, `grep`, and `tail`.
Unlike the **System Journal**, they are not stored in a binary journal database.
### Log rotation basics
   **Log rotation** is the process of managing log files so they do not grow indefinitely and consume too much disk space.
- Renames or archives the old log.
- Creates a new empty log file.
- Optionally compresses old logs to save space.
- Deletes very old logs after a certain period.

Linux systems use **logrotate** service for log rotation. Logs are automatically handled by **log rotate service**.
**Configuration files are in:**
  /etc/logrotate.conf
  ==inara@zahir-30:~$ cat /etc/logrotate.conf==

**Log Rutation Rules:**
  /etc/logrotate.d/
#### To test manually(Force Log Rotation)
   sudo logrotate -f /etc/logrotate.conf
   ==inara@zahir-30:~$ sudo logrotate -f /etc/logrotate.conf==

## Cron and timers
 Linux use Cron and Systemd Timers to run commands/scripts automatically at set time.
###   Scheduling work with cron
   Cron is a Linux tool that **runs commands/Scripts (Tasks) automatically at set times**. Like a built-in alarm clock for your system. It works in background using **crond** service. These tasks are called **cron jobs**.
####   Cron Schedule Format
   A cron schedule has 5 time fields.
   
     * * * * *  command
     │ │ │ │ │
     │ │ │ │ └── Day of week  (0-7, Sun=0 or 7)
     │ │ │ └──── Month        (1-12)
     │ │ └────── Day of month (1-31)
     │ └──────── Hour         (0-23)
     └────────── Minute       (0-59)
     
  Common Examples:
    Schedule                                            Meaning
    `* * * * *`                                       Every minute
    `0 * * * *`                                       Every hour
    `0 9 * * *`                                       Every day at 9 AM
    `0 9 * * 1`                                       Every Monday at 9 AM
    `0 0 1 * *`                                       1st of every month at midnight
    `*/15 * * * *`                                  Every 15 minutes
    
   Schedule a cron to work on every hr:
              0 * * * * echo "Hello"
    Schedule a corn to work every monday at 10 AM:
          0 10 * * 1 echo "Welcome"
### Crontab syntax
   A cron entry has 5 time fields and then a command.
   ex:
     zahir@zahir:~$ sudo nano /etc/crontab  
     then put this file inside.
     * * * * * /home/user/script.sh    -> Every minute
     0 * * * * /home/user/backup.sh  -> Every hour
     0 2 * * * command  ->Every day at 2 AM
     0 17 1 * * command  -> 1st of every month at 5 PM
 **Some Shortcuts:**
     @daily command
     @weekly command
     @monthly command
#### View cron logs
     grep CRON /var/log/syslog
  ==inara@zahir-30:~$ sudo grep CRON /var/log/syslog==
#### Check Cron Service Status
     systemctl status cron
  ==inara@zahir-30:~$ systemctl status cron==
    or
      systemctl status crond
####    Edit Cron Jobs
    corntab -e
   ==inara@zahir-30:~$ crontab -e==
####    List Cron Jobs
     corntab -l
   ==inara@zahir-30:~$ crontab -l==
####    Remove all Cron Jobs
      crontab -r
### Systemd timers
  Systemd timers are modern alternative to cron. It is reliable and easier to manage.
  **How It Works**
    Systemd timer need two files.
    Service File (.service) -> defines what to run
    Timer File (.timer) -> defines when to run
    Example:
   **The .service File**
      [Unit] 
      Description=My Backup Job
       
     [Service] 
     ExecStart=/usr/bin/bash /scripts/backup.sh

   **The .timer File**
       [Unit] 
       Description=Run backup daily 
       
       [Timer] 
       OnCalendar=daily    # WHEN to run 
       Persistent=true       # catch up if system was off 
       
       [Install] 
       WantedBy=timers.target
       
   Step 1: Create the Service File
      ==zahir@zahir:~$ sudo nano /etc/systemd/system/myjob.service==
      and put Configuration in it
      
   Step 2: Create the Timer File
      ==zahir@zahir:~$ sudo nano /etc/systemd/system/myjob.timer==
      and put configuration in it
### Verifying scheduled jobs
#### List all Timers
     systemctl list-timers  -> active timers
  ==inara@zahir-30:~$ sudo systemctl list-timers==
  
     systemctl list-timers --all  ->all including inactive
   ==inara@zahir-30:~$ sudo systemctl list-timers --all==
#### To find timer unit files installed on the system
   ==inara@zahir-30:~$ sudo systemctl list-unit-files --type=timer==
#### To search for a timer by name
   ==inara@zahir-30:~$ sudo systemctl list-unit-files --type=timer | grep backup==
#### Check a specific timer(status)
      systemctl status backup.timer  -> timer status
   ==inara@zahir-30:~$ sudo systemctl status apt-daily.timer==
#### To see where a specific timer file is located
   ==inara@zahir-30:~$ sudo systemctl cat apt-daily.timer==
   
      systemctl status backup.service -> sevice status
   ==inara@zahir-30:~$ sudo systemctl status backup.service==
#### Dry-run your script manually first 
       /scripts/backup.sh
#### To find service unit files installed on the system
   ==inara@zahir-30:~$ sudo systemctl list-unit-files --type=service==
#### To Enable Service
   ==inara@zahir-30:~$ sudo systemctl enable debug-shell.service==
#### To Start Service
       sudo systemctl start backup.service
   ==inara@zahir-30:~$ sudo systemctl start backup.service==
#### To Start Timer
       sudo systemctl start backup.timer
   ==inara@zahir-30:~$ sudo systemctl start backup.timer==
#### To Stop Timer
       sudo systemctl stop backup.timer
   ==inara@zahir-30:~$ sudo systemctl stop backup.timer==
#### To Enable Timer on boot
       sudo systemctl enable backup.timer
   ==inara@zahir-30:~$ sudo systemctl enable backup.timer==
#### Validate timer syntax 
    systemd-analyze verify backup.timer

## Storage basics
###   Disks and partitions
   Disk is a physical storage device. ex: SSD,HDD,USB etc.
   In linux disk are name as:
  1. /dev/sda
   2. /dev.sdb
   3.  /dev/nvme
   Partitions are divide a Disk into multiple section.
   example:
   Disk: /dev/sda
   Partitions:
      /dev/sda1
      /dev/sda2
      /dev/sda3
#### View Disk & Partitions
  List Disk:
    lsblk
 ==zahir@zahir:~$ lsblk==
 
  Detail Partitions Info:
     sudo fdisk -l
  ==zahir@zahir:~$ sudo fdisk -l==
     
 View Storage devices:
      blkid
  ==zahir@zahir:~$ blkid==

### File systems and mounting
####   File System
   A file system is how Linux **organizes and stores data** on a storage device (HDD, SSD, USB, etc.).
   **Common Linux file systems:**
- **ext4** — default for Ubuntu Linux, reliable and fast
- **NTFS** — Windows file system (Linux can read/write it)
- **FAT32/exFAT** — used on USB drives, cross-platform
- **tmpfs** — temporary file system stored in RAM
##### Check file system type:
  lsblk -f
  ==zahir@zahir:~$ lsblk -f==
##### Check Disk Space and File system type:
 df -T
 ==zahir@zahir:~$ df -T==
#### Mounting
   Mounting is a process of attaching a storage device(or partition) to a specific folder location so Linux can access it. This folder location is called Mount Point.
##### Creating Mount Point Folder
 sudo mkdir 
 ==inara@zahir-30:/$ sudo mkdir /mnt/usb==
##### Mount a device partition
 sudo mount /dev/sda1 /mnt/usb
 ==inara@zahir-30:/$ sudo mount /dev/sda3 /mnt/usb==
#### View all Mounted File System
  ==inara@zahir-30:/$ df -h==
##### Unmount a device
 ==inara@zahir-30:/$ sudo umount /mnt/usb

### Checking disk and inode usage
####   Disk Usage
   Disk usage means how much storage space is being used and how much is available on your drives.
#####    Check Disk usage or space
   ==inara@zahir-30:/$ df -h==
#####    Check Disk Usage and file system type
   ==inara@zahir-30:/$ df -T==
#####    Check Size of a specific folder
   ==inara@zahir-30:/$ du -sh Desktop==
##### Check Size of each folder under root
   ==inara@zahir-30:/$ du -sh /*==
#### Inode Usage
   An inode stores information(permissions, owner, timestamps, location) about a file.
   Every file uses one inode.
   If inodes are full:
You cannot create new files Even if disk space is available.
####   Check Inode Usage
   ==inara@zahir-30:/$ df -i==
####   View Inode number of a file
   ==inara@zahir-30:/$ ls -i file.txt==


### Persistent mounts
A **persistent mount** is a way to permanently attach a storage drive to a computer's operating system so it stays connected even after a reboot.
Without a persistent mount, any manually connected drive disappears from your file system when you restart.
#### How it Works on Linux
- **The Problem**: Regular mounts using the `mount` command are temporary.
- **The Solution**: You add the drive's information to a special configuration file called `/etc/fstab`.
- **The Result**: The system automatically reads this file during boot and mounts the drive every time.
#### View fstb
==inara@zahir-30:/$ cat /etc/fstab==
#### Edit fstb
==inara@zahir-30:/$ sudo nano /etc/fstab==
#### Test fstab without Reboot
==inara@zahir-30:/$ sudo mount -a==
 if no error appears then configuration is correct.

## SSH and key based authentication
###   The SSH client and server
   **SSH (Secure Shell)** is a protocol for securely connecting to and controlling remote computers/servers over a network.
   Used to remotely manage servers.
   ex:
     Remotely login to a server.
     Run commands to another computer.
   That's the core of SSH — **client connects, server listens, everything is encrypted**.
####    SSH Client
   The **client** is the machine _initiating_ the connection — the one you're sitting at.
   Install SSH Client:
   ==inara@zahir-30:~$ sudo apt install openssh-client==
#####    Connect to a remote server:
    sudo username@server_ip
   ==zahir@zahir:~$ ssh inara@10.11.119.30==
   inara -> user name(account on server)
   10.11.119.30  -> server_ip
####   SSH Server
   The **server** is the machine _being connected to_ — it listens for incoming SSH connections.
   Install SSH Server:
   ==inara@zahir-30:~$ sudo apt install openssh-server==
####   Start SSH
   ==inara@zahir-30:~$ sudo systemctl start ssh==
####   Status SSH
   ==inara@zahir-30:~$ sudo systemctl status ssh==
####   Enable SSH
   ==inara@zahir-30:~$ sudo systemctl enable ssh==

### Generating and distributing keys
  Instead of typing a password every time, SSH keys use a **key pair** — like a lock and key:
- 🔑 **Private key** → stays on YOUR machine (never share)
- 🔓 **Public key** → placed on the remote server (safe to share)
####   Generate Key Pair (on Client)
   ==zahir@zahir:~$ sudo ssh-keygen==
   This will create:
  ~/.ssh/id_rsa ← Private key (KEEP SECRET)    ->rsa can ba num like ed25519
 ~/.ssh/id_rsa.pub ← Public key (share this to server   -> rsa can ba num like ed25519
 Verify keys were created:
 zahir@zahir:~$ ls -la ~/.ssh/
To View:
 cat ~/.ssh/id_rsa  ->private key
 cat ~/.ssh/id_rsa.pub ->private key
#### Distribute Public Key to Server
ssh-copy-id username@server_ip -> this will copy public key to server
`ssh-copy-id` is designed to copy **only your public key** to the remote server.

==zahir@zahir:~$ ssh-copy-id zahir@10.0.2.15==  
Automatically appends your public key to file (`~/.ssh/authorized_keys`) on the server.

Now login using(without password):
==zahir@zahir:~$ ssh zahir@10.0.2.15==

### The SSH client configuration file
####  What & Where
   The SSH client config file lets you **save connection settings** so you don't type long commands every time.
    ~/.ssh/config ← per-user config (most used)
    /etc/ssh/ssh_config ← system-wide config (all users)
#### Create / Edit the File
   ==inara@zahir-30:~$ nano ~/.ssh/config==
#### Basic Structure

```
Host <nickname>
    HostName <server_ip_or_domain>
    User <username>
    Port <port_number>
    IdentityFile <path_to_private_key>
```

### Copying files with scp and rsync
#### Copy Files
To copy files we use
1. CP -> copy files on same machine
     cp [options] source destination
     
2. SCP -> copy files to another machine over ssh
     **Copy a file to a remote server:**
                `scp file.txt user@server:/destination/path/`
                ==zahir@zahir:~$ scp file.txt zahir@10.0.2.15:/home/zahir==
         **Copy file from a remote server:**
                `scp user@server:/path/to/file.txt ./local/folder/`
                ==zahir@zahir:~$ scp zahir@10.0.2.15:/home/zahir/file.txt /home/zahir/Desktop==
          **Copy a whole folder:**
                 `scp -r myfolder/user@server:/destination`
     
3. RSYNC -> smart/sync files to another machine
    - Really smart.
    - It checks whether a file is there and only sends what changed, it basically syncs.
    - Great for backups & larger folders.
    **Sync folder to a remote server:**
                 `rsync -avz myfolder /user@server:/destination/`
     **Sync folder from a remote server:**
                 `rsync -avz user@server:/path/to/folder/ ./local/folder`

## Shell scripting intro
### Declaring & using variables:
 ==inara@zahir-30:~$ name="Zahir"==  ->Declaration of variable(No space around = )
 inara@zahir-30:~$ echo $name   ->Always To Use variable with $
 Zahir
#### Common variable types:
 **Number:**
 ==inara@zahir-30:~$ age=25==   
 ==inara@zahir-30:~$ echo $age==
 ==25==
Don't use quotes around numbers

**String:**
 ==inara@zahir-30:~$ greeting="Hello"==
 ==inara@zahir-30:~$ echo $greeting==
 ==Hello==
 Use always quotes around String Value
 
**Command Output**
==inara@zahir-30:~$ files=$(ls)==
==inara@zahir-30:~$ echo $files==
==Desktop Documents Downloads==

#### Quoting
  There are three types of quotes, each behaves differently:
  Double Quotes (" ")   -> allow variable expansion
Example:
    ==inara@zahir-30:~$ name="zahir"==
    ==inara@zahir-30:~$ echo "Hello $name"==
    ==Hello zahir==
    
Single Quotes (' ')  -> Everything is literal
   No variable expansion.
Example:
     ==inara@zahir-30:~$ name="zahir"==
     ==inara@zahir-30:~$ echo 'Hello $name'==
     ==Hello $name==
     
 file="my file.txt" 
 cat "$file" # ✅ works 
 cat $file # ❌ tries to open "my" and "file.txt" separately

  Backticks / Command sub (`...`) or  $(. . .)  -> Runs a Commond
     ==inara@zahir-30:~$ echo "Today: $(date)"==
     ==Today: Wed Jun 17 11:37:39 UTC 2026==
     
##### Concatenation
 For Concatenation we use: { }
 ==inara@zahir-30:~$ name="Zahir"==
 ==inara@zahir-30:~$ echo "${name}s"==
 ==Zahirs==
**Always quote your variables** with "$var"

### Conditionals and loops

#### Conditions

 **if/elif/else**
 ==zahir@zahir:~$ age=20==
 ==zahir@zahir:~$ if [ $age -gt 18 ]; then==
> ==echo "Adult"==
> ==elif [ $age -eq 18 ]; then==
> ==echo "Just turned 18"==
> ==else==
> ==echo "Minor"==
> ==fi==
==Adult==

**Case Statement**
==zahir@zahir:~$ day="Mon"==
==zahir@zahir:~$ case $day in==
> ==Mon) echo "Monday";;==
> ==Tue) echo "Tuesday";;==
> ==Sat | Sun) echo "Weekend";;==
> ==*) echo "Other day";;==
> ==esac==
==Monday==

#### Loops

**for loop**
Runs fixed number of time. You know the iterations.
==zahir@zahir:~$ for ((i=0; i<5; i++)); do==
> ==echo $i==
> ==done==
==0==
==1==
==2==
==3==
==4==

**While loop**
 Runs as log as condition is true. unknown iterations.
 ==zahir@zahir:~$ while [ $count -le 5 ]; do== 
 ==echo "count: $count"==
  ==((count++))==
  ==done==
==count: 1==
==count: 2==
==count: 3==
==count: 4==
==count: 5==

### Exit codes
  Every command in Linux returns an **exit code** (also called return status) when it finishes — a number that tells you if it succeeded or failed.
  **`$?`** always holds the exit code of the **last run command**.
  
  ==zahir@zahir:~$ echo "Hello"==
  ==Hello==
  ==zahir@zahir:~$ echo $?==
  ==0==         -> 0 (success)

==zahir@zahir:~$ ls /fakepath==
==ls: cannot access '/fakepath': No such file or directory==
==zahir@zahir:~$ echo $?==
==2==         -> 2 (error- No such file)    

**Standard Exit Code Values**
Code                                     Meaning
  0                                      ✅ Success
  1                                      ❌ General error
  2                                       Misuse of shell/command
  126                                  Command found but not executable
  127                                  Command not found
  128+n                              Fatal signal `n` (e.g. `130` = Ctrl+C)
  255                                   Exit status out of range

### Writing a simple reusable script
 ==zahir@zahir:~$ nano myscript.sh==  -> create and edit(write commands/code) file(myscript.sh)
 ==zahir@zahir:~$ chmod +x myscript.sh==  -> add  execution permission(x) to file (make it executable)
 ==zahir@zahir:~$ ./myscript.sh==    -> run from current directory
 ==zahir@zahir:~$ bash -x myscript.sh==  -> debug mode

## Install and configure Apache or Nginx
### Installing Nginx
==inara@zahir-30:~$ sudo apt install nginx==
### Working with the main configuration files
Main File:
  /etc/nginx/nginx.conf
  To View:
   ==inara@zahir-30:~$ sudo cat /etc/nginx/nginx.conf==

  Site Configuration:
    /etc/nginx/sites-available/default

### Serving a test site
1. Create a simple folder.
    ==inara@zahir-30:~$ sudo mkdir -p /var/www/testsite==
2. Create and Edit(write) a web page (index.html) in this folder
   ==inara@zahir-30:~$ sudo nano /var/www/testsite/index.html==
     Write  html code inside:
         
  3. Fix Permission to the Folder so Nginx can read it
    ==inara@zahir-30:~$ sudo chown -R www-data:www-data /var/www/testsite==
    
       chown  -> change ownership of folder/file
     -R -> **Recursive** — apply to folder + everything inside it
      www-data:www:data -> `owner:group` → set both to `www-data`
      /var/www/tsextsite -> The target fiolder
      
 4. Create the site Config
     ==inara@zahir-30:~$ sudo nano /etc/nginx/sites-available/testsite==
     Write Code Inside:
     server { 
        listen 80; 
        server_name testsite.local; # or your IP / domain 
        
        root /var/www/testsite; 
        index index.html;
        
		 location / { 
        try_files $uri $uri/ =404;
           }
         }
         
   1. Enable the Site
     ==inara@zahir-30:~$ sudo ln -s /etc/nginx/sites-avaiable/testsite /etc/nginx/sites-enabled/==
     
   2. Test & Reload
     ==inara@zahir-30:~$ sudo nginx -t==
     ==inara@zahir-30:~$ sudo systemctl reload ngin==x
     
   3. Visit Test site(in browser)
     http://localhost
     
## Starting and verifying the service
   1. Start Nginx
        ==inara@zahir-30:~$ sudo systemctl start nginx==
        
   2. Verify Nginx is Running
         ==inara@zahir-30:~$ sudo systemctl status nginx==
         
   3. Enable Nginx (at boot)
         ==inara@zahir-30:~$ sudo systemctl enable nginx==
         
   4. Verify Nginx is Listening
         ==inara@zahir-30:~$ curl http://localhost==

## Harden SSH
  Hardening SSH means securing your SSH server to reduce the risk of **unauthorized access** or **attacks**. It involves changing default settings to make it harder for attackers to break in.
###   Disabling root login
   Prevent the root user from logging in directly through SSH. This reduces the risk of attackers targeting the root account.
   How to do it?
    1. open and Edit ssh config file
     ==inara@zahir-30:~$ sudo nano /etc/ssh/sshd_config==
    2. Find This
     PermitRootLogin  yes
     3.  change it to
      PermitRootLogin  no or prohibit-password
     4. Save and Exit
        CTRL +O Enter CTRL + X
     5. Restart ssh Service
        ==inara@zahir-30:~$ sudo systemctl restart ssh==

### Enforcing key based login
  Enforcing **key-based SSH login** means you completely disable passwords and allow only SSH keys for authentication.
- Stronger security (no password guessing/brute force)
- Only devices with your private key can log in
How do it?
  Steps to enforce key-based login
  1. Add ssh key First
    ssh-copy-id user@server_ip
    ==inara@zahir-30:~$ sudo ssh-copy-id inara@10.11.119.30==
  2. Edit SSH config 
     ==inara@zahir-30:~$ sudo nano /etc/ssh/sshd_config==
     Set or Change these lines
      PubkeyAuthentication yes  
      PasswordAuthentication no  
      ChallengeResponseAuthentication no  
      PermitRootLogin prohibit-password
      Save and Exit.
    3. Restart SSH Service
         ==inaa@zahir-30:~$ sudo systemctl restart ssh==
#### What this does
- ❌ Password login is disabled
- ❌ Root password login is blocked
- ✅ Only SSH key login works

### Restricting which users may connect
 Instead of “any valid system user can SSH in,” you say: “Only these specific users are allowed.
#### How to configure it?
1. Edit SSH config 
     ==inara@zahir-30:~$ sudo nano /etc/ssh/sshd_config==
     Add directives in the bottom of the file or in relevent section
2. Add Allow User
     AllowUsers ali admin
    This means:
- ✅ `ali` can SSH in
- ✅ `admin` can SSH in
- ❌ all other users are blocked from SSH

Allow a whole group:
    AllowGroups group_name
Then add users to the Group
     sudo groupadd group_name  
     sudo usermod -aG group_name ali
     
3. Restart SSH
   ==inara@zahir-30:~$ sudo systemctl restart ssh==

- `AllowUsers` → whitelist specific users
- `AllowGroups` → whitelist a group of users
- Everything else is automatically denied SSH access
  4. Deny Users
     DenyUsers root baduser temp
  5. Deny Groups
     DenyGroups group_name
  6. **Combined example** — allow a group but explicitly block one user:
     DenyUsers contractor1
     AllowGroups group_name
  
### Limiting authentication attempts and idle sessions
   **Edit SSH config** 
     ==inara@zahir-30:~$ sudo nano /etc/ssh/sshd_config==
     then set
        MaxAuthTries 3  ->- Limits wrong login attempts per SSH session After 3 failures                                                connection is dropped
        LoginGraceTime 30  -> - User has **30 seconds** to successfully authenticate(login) 
                             Otherwise connection is closed.
        ClientAliveInterval 300  -> Every **300 seconds (5 min)** server checks if client is active
        ClientAliveCountMax 2 -> If no response for **2 checks → session is closed**
        
- **MaxAuthTries** → stops brute-force attempts
- **LoginGraceTime** → limits login time window
- **ClientAliveInterval + CountMax** → disconnects idle sessions

  **Restart SSH**  
      ==inara@zahir-30:~$ sudo systemctl restart ssh==

## Schedule a backup script
###   Writing a backup script
   A backup script automatically copies important files from source to Destination (backup location).
   Example:
     
   ==zahir@zahir-30:~$ sudo nano backup.sh==
   then write
    #1/bin/bash
    SOURCE_DIR="/home/zahir/Documents"    ->Source directory to back up
    BACKUP_DIR="/home/zahir/backups" -> Destination directory for backups
    TIMESTAMP=$(date +"%y-%m-%d_%H-%M-%S")  -> Create timestamp
    BACKUP_PATH="$BACKUP_DIR/backup_$TIMESTAMP" -> Backup folder name
    mkdir -p "$BACKUP_PATH" ->create backup folder
    cp -r "$SOURCE_DIR"/* "$BACKUP_PATH"  -> copy
    echo "backup completed: $BACKUP_PATH"  ->confirmation
      then Save and Exit.
   **To make executable:**
     ==zahir@zahir-30:~$ sudo chmod +x backup.sh==
   **Run it**
     ==zahir@zahir-30:~$ ./backup.sh==
### Scheduling it with cron or a timer
###   Schedule Backup with Systemd Timer
   Systemd timers are more powerful than cron. Systemd timer has **2 files**: a service + a timer
1. Create Service File
   ==zahir@zahir-30:~$ sudo nano /etc/systemd/system/backup.service==
     then write
    [Unit]
    Description=Run backup script

    [Service]
    Type=oneshot
    ExecStart=/home/zahir/backups/backup.sh
     
  2. Create Timer File
     ==zahir@zahir-30:~$ sudo nano /etc/systemd/system/backup.timer==
      then Write
      [Unit]
      Description=Daily backup timer

      [Timer]
       OnCalendar=*-*-* 01:00:00
       Persistent=true

       [install]
       WantedBy=timers.target
       
 3. Enable and Start Timer 
     ==zahir@zahir-30:~$ sudo systemctl enable backup.timer==

     ==zahir@zahir-30:~$ sudo systemctl start backup.timer==
     
4. Verify its Running
     ==zahir@zahir-30:~$ sudo systemctl status backup.timer==
#### Common Timer Schedules

```ini
# Every day at 2 AM
OnCalendar=*-*-* 02:00:00

# Every hour
OnCalendar=hourly

# Every Monday at 6 AM
OnCalendar=Mon *-*-* 06:00:00

# Every 30 minutes
OnCalendar=*:0/30

# Daily shortcut
OnCalendar=daily
```
### Verifying output and logs

  1. **Check Backup File Exist**
     ==inara@zahir-30:~$ ls -lh /backup/==

  2.  **Verify Backup Contents**
      ==inara@zahir-30:~$ tar -tzf /backup/backup_*.tar.gz==

 3. **Check Systemd Timer Logs**
      View timer logs:
      ==inara@zahir-30:~$ journalctl -u backup.timer==

       View Service logs:
        ==inara@zahir-30:~$ journalctl -u backup.service==
        
       View Logs(watch in Real Time)
        ==inara@zahir-30:~$ journalctl -u backup.service -f==

       View Last 20 Lines
        ==inara@zahir-30:~$ journalctl -u backup.service -n 20==

   4. **Check Timer Status**
      ==inara@zahir-30:~$ sudo systemctl status backup.timer==

   5. **Check Service Status**
      ==inara@zahir-30:~$ sudo systemctl status backup.service==

   6. **Add Logging to Backup Script**
    Modify your script to **save logs**
    
     #!/bin/bash
SOURCE="/home/inara/Documents"
DEST="/backup"
DATE=$(date +%Y-%m-%d_%H-%M-%S)
BACKUP_NAME="backup_$DATE.tar.gz"
LOG="/var/log/backup.log"          # ← Log file
mkdir -p "$DEST"

#Run backup and log output
tar -czf "$DEST/$BACKUP_NAME" "$SOURCE" >> "$LOG" 2>&1

#Log result
if [ $? -eq 0 ]; then
    echo "$DATE - SUCCESS: $BACKUP_NAME" >> "$LOG"
else
    echo "$DATE - FAILED!" >> "$LOG"
fi

7. **View Backup Log File**
   ==inara@zahir-30:~$ cat /var/log/backup.log==  -> view full log
   ==inara@zahir-30:~$ tail -10 /var/log/backup.log==  -> view last 10 entries

## RAID
###   The RAID concept and its purpose
   **RAID** = **R**edundant **A**rray of **I**ndependent **D**isks.
   RAID is Combining multiple hard drives** to work as one — for speed, safety, or both.
   Used For:
    **Performance** (faster read/write speed)
    **Redundancy** (protect data if a disk fails)
    **Storage capacity** (combine disk space)
 Example:
     Single Disk                RAID
     ─────────           ─────────────────
     One drive     →     Multiple drives
    No backup     →     Data protected
     Slow          →     Faster read/write

### Common RAID levels
  **RAID 0** — Striping (Speed)
  Splits Data accross Disks
  Disk 1: A C E
  Disk 2: B D F
  - ✅ **Fastest** — data split across disks
- ❌ **No protection** — one disk fails = all data lost
- 📌 Use for: video editing, gaming

**RAID 1** — Mirroring (Safety)
Disk 1: A B C
Disk 2: A B C  ← exact copy
- ✅ **Best protection** — one disk fails, other takes over
- ❌ **50% storage wasted** — 2×1TB = only 1TB usable
- 📌 Use for: OS drives, critical data

**RAID 5** — Striping + Parity (Balance)
Disk 1: A B P
Disk 2: B C P
Disk 3: C A P  ← P = parity (recovery data)
- ✅ **Good balance** of speed + safety
- ✅ **1 disk can fail** safely
- ❌ Needs minimum **3 disks**
- 📌 Use for: servers, NAS

**RAID 6** — Double Parity (Extra Safety)
   Same as RAID 5 but with 2 parity blocks.
- ✅ **2 disks can fail** safely
- ❌ Needs minimum **4 disks**
- 📌 Use for: critical servers

**RAID 10** — Mirror + Stripe (Best of Both)
    Disk 1+2: Mirrored pair
    Disk 3+4: Mirrored pair
    Both pairs: Striped
- ✅ **Fast + Safe**
- ❌ Needs minimum **4 disks**, 50% storage used
- 📌 Use for: databases, high-traffic servers

### An overview of software RAID
Software RAID is managed by the operating system instead of a hardware RAID controller.
In Linux, software RAID is managed using **mdadm**.

Hardware RAID                  Software RAID
─────────────                  ─────────────
Dedicated controller  →    OS manages it
Expensive             →          Free
Independent of OS     →    OS dependent
Fast                  →               Slightly slower (CPU used)
#### How Software RAID Works on Linux
   ┌─────────────────────────────┐
   │         Applications        │
   ├─────────────────────────────┤
   │        File System          │
   ├─────────────────────────────┤
   │    md (Linux RAID layer)    │  ← Software RAID lives here
   ├──────────┬──────────────────┤
   │  Disk 1  │  Disk 2  │Disk 3 │  ← Physical drives
   └──────────┴──────────────────┘

#### Install mdadm
==inara@zahir-30:~$ sudo apt install mdadm==
#### Create RAID Arrays
##### RAID 0 — Striping
   sudo mdadm --create /dev/md0 \
  --level=0 \
  --raid-devices=2 \
  /dev/sdb /dev/sdc
##### RAID 1 — Mirroring
  sudo mdadm --create /dev/md0 \
  --level=1 \
  --raid-devices=2 \
  /dev/sdb /dev/sdc
##### RAID 5 — Striping + Parity
   sudo mdadm --create /dev/md0 \
  --level=5 \
  --raid-devices=3 \
  /dev/sdb /dev/sdc /dev/sdd

#### Format & Mount RAID
#Format the RAID device
sudo mkfs.ext4 /dev/md0

#Create mount point
sudo mkdir /mnt/raid

#Mount it
sudo mount /dev/md0 /mnt/raid

#Verify
df -h /mnt/raid

#### Auto Mount on Boot
#Add to /etc/fstab
echo '/dev/md0 /mnt/raid ext4 defaults 0 0' | sudo tee -a /etc/fstab

#### Monitor & Manage RAID
#Check RAID status
cat /proc/mdstat

#Detailed info
sudo mdadm --detail /dev/md0

#Watch live status
watch cat /proc/mdstat

##### Common Management Commands
#Add a spare disk
sudo mdadm /dev/md0 --add /dev/sdd

#Remove a disk
sudo mdadm /dev/md0 --remove /dev/sdb

#Stop RAID array
sudo mdadm --stop /dev/md0

#Scan and assemble existing arrays
sudo mdadm --assemble --scan

#Save RAID config
sudo mdadm --detail --scan | sudo tee /etc/mdadm/mdadm.conf

##### Check Disk Health
#Check for errors
sudo mdadm --detail /dev/md0 | grep -i state

#Run consistency check
echo check | sudo tee /sys/block/md0/md/sync_action

#View check progress
cat /proc/mdstat

##### RAID Status Indicators
| Symbol     | Meaning                     |
| ---------- | --------------------------- |
| `UU`       | ✅ Both disks healthy        |
| `U_`       | ⚠️ One disk failed          |
| `resync`   | 🔄 Rebuilding array         |
| `degraded` | ❌ Running with missing disk |
##### Pros & Cons of Software RAID
| Pros                       | Cons                        |
| -------------------------- | --------------------------- |
| ✅ Free — no extra hardware | ❌ Uses CPU resources        |
| ✅ Flexible & configurable  | ❌ OS must boot to manage    |
| ✅ Works on any hardware    | ❌ Slower than hardware RAID |
| ✅ Easy to monitor & manage | ❌ RAID lost if OS corrupted |
##### Summary
Software RAID on Ubuntu
━━━━━━━━━━━━━━━━━━━━━━
Tool     → mdadm
Device   → /dev/md0
Monitor  → /proc/mdstat
Config   → /etc/mdadm/mdadm.conf

### Why RAID is not a backup
RAID protects against **disk failure**, but it does **not** protect against:
- Accidental file deletion
- Virus or ransomware attacks
- File corruption
- System damage
- Fire or theft
#### The Big Difference
   RAID                                       Backup
────────────────              ────────────────
Same location          vs       Different location
Real-time mirror       vs      Point-in-time copy
Protects hardware      vs     Protects everything
No history             vs           Multiple versions


#### RAID vs Backup — What Each Protects

| Threat                | RAID | Backup |
| --------------------- | ---- | ------ |
| Disk hardware failure | ✅    | ✅      |
| Accidental deletion   | ❌    | ✅      |
| Ransomware/Virus      | ❌    | ✅      |
| File corruption       | ❌    | ✅      |
| Natural disaster      | ❌    | ✅      |
| Human error           | ❌    | ✅      |
| Multiple disk failure | ❌    | ✅      |
| Old file versions     | ❌    | ✅      |
#### Conclusion:
   RAID  = Keeps you RUNNING when a disk dies
  Backup = Keeps your DATA when anything goes wrong
  Use BOTH together for real protection! ✅

### Practical Task
1. Create two virtual disks.
2. Create a RAID 1 array using `mdadm`.
3. Format it with `ext4`.
4. Mount it on `/raid`.
5. Create a test file:




##             MODULE-2 LINUX OPERATIONS COMPLETED


##                                      THANKS...
       


       




