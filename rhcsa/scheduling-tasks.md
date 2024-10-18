---

---
# Scheduling Tasks
--- 


For the exam focus on `systemd` timers

`systemctl list-units -t timer`
- this will list all the timers

Logrotate example.

`systemctl list-unit-files logrotate.*`
 - list all the files on logrotate
## Commands
---

`systemctl status <service you want status of>`
`systemctl list-units -t timer`
`systemctl list-unit-files <file you want to list>`


"/user/lib/systemd/system" `ls logro*` you will see the "logrotate.timer"

### Crontab Syntax
--- 
![[assets/crontab-syntax.png]]

## Lab 
---
**Objectives: Running Scheduled Jobs
1. Ensure the systemd timer that cleans up tmp files is enabled
2. run a cron job that will issue the command `touch /tmp/cronfile` 5 minutes from now
3. use `at` to schedule a job to power off your system at a convenient time later today

**Steps**:
1. **Step 1**: Verify tmp file cleanup timer is running
	- `systelctl list-units -t timer` 
		- `systemd-tmpfiles-clean.timer` - daily cleanup of temporary directories
		- `systemctl status systemd-tmpfiles-clean.timer` - shows the services is enabled
2. **Step 2:** Run a cron job that will issue the command `touch /tmp/cronfile` 5 minutes from now.
	- **Commands Used:** 
		- `crontab -e`
			- ![[crontab-job-touch-tmp.png]]
		- run `systemctl restart crond` this will make sure the job is synced immediately
3. **Step 3:** Use `at` to schedule a job to power off system
	- `at 14:45`
	- `at> poweroff`
	- ctrl-D to confirm it 
	- use `atq` to check status of job
	
	- ! MAKE SURE YOU DO THIS AS ROOT
- 
test