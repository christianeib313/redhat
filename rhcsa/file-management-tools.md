# FINDING FILES
---
- `find` command can help with finding a file in a directory
	- `find / -name "hosts > 2>/dev/null"` 
		- this will search within the `/` directory for the file name "hosts" and if there are any errors send them to the stderr 
	- if you want to find a file that includes a text use wildcards for ex.
		- `find / -name "*hosts*" > 2>/dev/null` 
			- this search for files that have the name hosts included where as the top will look specifically for names that only contain "hosts"
	- `find / -type -f -size +100M`
		- search for a specific type. In this case `-f` = files that contain a `-size` bigger than `+100M`
	- `find /etc -exec grep -l student {} \;`
		- find within the `/etc` directory and execute `grep` with `-l student` the `{}` are needed because it's a placeholder for where the find commands results will be going in to and the `\;` is needed to close out a `-exec` command
# MOUNTING FILESYSTEMS
- a mount is a connection between a device and a directory
- 
