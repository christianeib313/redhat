
# OVERVIEW
---
- There are three different approaches that can be used by services to write log informatoin
	1. `systemd-journald` - default solution for logging in RHEL9
	2. Direct write: some services write logging directly to the log files - this is not recommended 
	3. `rsyslog`: is the enhancement of syslog and takes care of centralized log files. `systemd-journald` may be the default but `rsyslog` provides features not offered by `systemd-journald`.

## Get information about what is happening on your system
---
- `journalctl`: to get detailed information from the journal
- `systemctl status <unit>`: short overview of the most recent significant events
- monitor files in `/var/log` that are written by `rsyslogd` 
### SYSTEM LOG FILES OVERVIEW
---
- `/var/log/messages`: Commonly used log file; it is the generic log file where most messaged are written to
- `/var/log/dmesg`: kernel log messages
- `/var/log/secure`: authentication-related messages. *check here to see which authentication errors have occurred on a server.*
- `/var/log/boot.log`: messages related to system startup.
- `var/log/audit/audit.log`: Contains audit messages. SELinux writes to this file.
- `var/log/maillog`: mail-related messages.
- `/var/log/httpd`: log files written by Apache web server. *apache writes files directly to this file and not through rsyslog*
### GENERAL COMMANDS:
---
- `tail -f <logfile>`: shows real time which lines are added to the log file
	- ctrl-c to close out
- `logger`: allows you to write messages to rsyslog
	- you could use this to have a script write any error messages to rsyslog
- `journalctl -f`: shows last lines the of messages where new log lines are automatically added
- `journalctl --no-pager`: shows contents of the journal without using a pager
- `logger`: used to write logs manually to `rsyslog`
## SYSTEMD-JOURNALD
---
- **Overview:** stores log messages into journal, a binary file that is **temporarily** stored in the file `/run/log/journal` 
- **Using journalctl to find events:** 
	- `journalctl`: show the content of the journal since last server boot
	- `journalctl -p err`: show errors only
	- `journalctl -o verbose`: if you need as much detail as possible
## Preserving Systemd Journal
---
- By default the journal is stored in the file `/run/log/journal` the entire `/run` directory is cleared when system reboots.
- To make system persistent through restarts:
	- create a directory `/var/log/journal`.
		- requires the `Storage=auto` parameter in `/etc/systemd/journald.conf` 
		- Available parameter values:
			- `Storage=auto`: The journal will be written on disk if the directory `var/log/journal` exists.
			- `Storage=volatile`: The journal will be stored only in the `/run/log/journal` directory.
			- `Storage=persistent`: The journal will be stored on disk in the directory `/var/log/journal`. This directory will be created automatically if it doesn't exsist.
			- `Storage=none`: No data will be stored, but forwarding to other targets such as the kernal log buffer or syslog will still work.
#### Make systemd journal persistent
---
1. `mkdir /var/log/journal`
2. `chown root:systemd-journal /var/log/journal`
	- *we need to change ownership to be able to write to this directory*
3. `chmod 2755 /var/log/journal`
4. `systemctl restart systemd-journal-flush`
5. systemd journal is now persistent
# RSYSLOG
---
- Two configuration files
	1. `/etc/rsyslog.conf`: Central location where rsyslogd is configured.
	2. `/etc/rsyslog.d`: drop in files
- Each logger line contains three items:
	1. **facility**: the specific facility that the log is created for
	2. **severity/priority**: the severity/priority from which the message should be logged
	3. **destination**: the file or destination the log should be written to
- Logs are locacted in /var/log
	- use `ls -lrt` to see most recent files listed last
		- `-l`: list in long format
		- `-r`: reverse the order of listing (oldest first)
		- `-t`: sort by time, with most recently modified first
# logrotate
---
- **rotating log files**:
	- system by default will rotate logs every 4 weeks
	- if `/var/log/messages` is rotated on June 8, 2023, then the rotated filename will be `/var/log/messages-20230608`
- `/etc/logrotate.conf`: default settings for log rotation
- `/etc/logrotate.d`: use if specific files need specific settings this will overwrite default settings in the `/etc/logrotate.conf` file
# LAB
---
### Objective:
1. Make sure the systemd journal is logged persistently
2. create an entry in rsyslog that writes all messages with a severity of error or higher to /var/log/error
3. Ensure that /var/log/error is rotated on a monthly basis, and the last 12 logs are kept before they are rotated out
#### Steps:
1. **First objective:
	- create a directoy /var/log/journal
	-  make sure `storage=auto` in `/etc/systemd/journald.conf`
2. **second objective:
	- add `*.err             /var/log/error` in `/etc/rsyslog.conf`
3. **third objective:
	- `vim /etc/logrotate.d/error`
		- reference `/etc/rsyslog.conf` for what needs to be added to config
#### TIP
---
- You can test to make sure error messages are going to the correct log by using `logger`
- `logger -p err hello world` will send a log with the priority of error 
- `cat /var/log/error` you will see the hello world log here if it is successful





