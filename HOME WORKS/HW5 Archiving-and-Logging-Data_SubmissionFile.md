## Week 5 Homework Submission File: Archiving and Logging Data

Please edit this file by adding the solution commands on the line below the prompt.

Save and submit the completed file for your homework submission.

---

### Step 1: Create, Extract, Compress, and Manage tar Backup Archives

1. Command to **extract** the `TarDocs.tar` archive to the current directory:
tar xvvf TarDocs.tar -C Projects

2. Command to **create** the `Javaless_Doc.tar` archive from the `TarDocs/` directory, while excluding the `TarDocs/Documents/Java` directory:

tar cvf Javaless_Docs.tar --exclude="*Java*" /home/sysadmin/Projects/TarDocs


3. Command to ensure `Java/` is not in the new `Javaless_Docs.tar` archive:

tar tvvf Javaless_Docs.tar | grep 'Java'


**Bonus** 
- Command to create an incremental archive called `logs_backup_tar.gz` with only changed files to `snapshot.file` for the `/var/log` directory:

sudo tar --listed-incremental=snapshot.file -cvzf logs_backup.tar.gz /var/log
#### Critical Analysis Question

- Why wouldn't you use the options `-x` and `-c` at the same with `tar`?

-- -x will extract the file that -C will create

### Step 2: Create, Manage, and Automate Cron Jobs

1. Cron job for backing up the `/var/log/auth.log` file:

0 6 * * 3 tar cvf logs_backups.tar /var/log/auth.log /auth_backup.tgz  >/dev/null 2>&1



authlog_backup_script" Contains:

#!/bin/bash

tar cvf logs_backups.tar /var/log/auth.log
()end


### Step 3: Write Basic Bash Scripts

1. Brace expansion command to create the four subdirectories:

first
mkdir backups
then 
sudo mkdir ~/backups/{freemem,diskuse,openlist,freedisk}

2. Paste your `system.sh` script edits below:


    #!/bin/bash
    
	free -h > ~/backups/freemem/free_mem.txt

	du -h > ~/backups/diskuse/disk_use.txt

	lsof > ~/backups/openlist/open_list.txt

	df -h > ~/backups/freedisk/free_disk.txt
]
    ```

3. Command to make the `system.sh` script executable:

chmod +x system.sh

**Optional**
- Commands to test the script and confirm its execution:

sudo ./system.sh

**Bonus**
- Command to copy `system` to system-wide cron directory:

--- sudo cp system.sh /etc/cron.weekly/

### Step 4. Manage Log File Sizes
 
1. Run `sudo nano /etc/logrotate.conf` to edit the `logrotate` configuration file. 

    Configure a log rotation scheme that backs up authentication messages to the `/var/log/auth.log`.

    - Add your config file edits below:

    ```bash
    $!/bin/bash

    /var/log/auth.log {
    # Rotate weekly
    rotate 7
    # Rotates only the seven most recent logs
    create
    # Do not rotate empty logs
    notifempty
    # Delay compression
    delaycompress
    # Skips error messages for missing logs and continues to next log
    missingok

    }

    ```
---

### Bonus: Check for Policy and File Violations

1. Command to verify `auditd` is active:

systemctl status auditd

2. Command to set number of retained logs and maximum log file size:

    local_events = yes
write_logs = yes
log_file = /var/log/audit/audit.log
log_group = adm
log_format = RAW
flush = INCREMENTAL_ASYNC
freq = 50
max_log_file = 35
num_logs = 7
priority_boost = 4

    ```

3. Command using `auditd` to set rules for `/etc/shadow`, `/etc/passwd` and `/var/log/auth.log`:


    - Add the edits made to the `rules` file below:

-w /etc/shadow -p wra -k hashpass_audit
-w /etc/passwd -p wra -k userpass_audit
-w /var/log/auth.log -p wra -k authlog_audit

    ```bash
    [Your solution edits here]
    ```

4. Command to restart `auditd`:

sudo systemctl restart auditd

5. Command to list all `auditd` rules:

sudo auditctl -l

6. Command to produce an audit report:

sudo aureport -au

7. Create a user with `sudo useradd attacker` and produce an audit report that lists account modifications:

8. Command to use `auditd` to watch `/var/log/cron`:

 sudo auditctl -w /var/log/cron

9. Command to verify `auditd` rules:

--- sudo auditctl -l

### Bonus (Research Activity): Perform Various Log Filtering Techniques

1. Command to return `journalctl` messages with priorities from emergency to error:
sudo journalctl -p err -b

1. Command to check the disk usage of the system journal unit since the most recent boot:

journalctl --disk-usage | less

1. Comand to remove all archived journal files except the most recent two:
sudo journalctl --rotate
sudo journalctl --vacuum-time=1m

1. Command to filter all log messages with priority levels between zero and two, and save output to `/home/student/Priority_High.txt`:

- journalctl -p crit -b > /home/student/Priority_High.txt
- more Priority_High.txt

1. Command to automate the last command in a daily cronjob. Add the edits made to the crontab file below:
- sudo nano High_Priority.sh
   #!/bin/bash
  journalctl -p crit -b >> /home/student/Priority_High.txt

- sudo cp High_Priority.sh /etc/cron.daily/

OR /??
    sudo nano /etc/systemd/journald.conf
    0 0* * * 
    ```

---
© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.
