
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
- Two configuration files
	1. `/etc/rsyslog.conf`: Central location where rsyslogd is configured.
	2. `/etc/rsyslog.d`: drop in files
- 


 

