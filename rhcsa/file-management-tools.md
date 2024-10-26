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
---
- a mount is a connection between a device and a directory
- `mount /dev/devicename /directory` to mount a device
- `findmnt` commands shows all currently mounted devices and their place in the filesystem
- `ls blk` to list devices that are not mounted
- `umount` to unmount if you need to remove a device
# USING LINKS
---
- compare links to shortcuts
- linux uses *hard links* and *symbolic links*
- `ln` **hard link**
- `ln -s` **soft link**

| inode -->                    | blocks -->                       | hard link -->                    | symbolic link                                              |
| ---------------------------- | -------------------------------- | -------------------------------- | ---------------------------------------------------------- |
| administration of your files | actual location files are stored | name attached to inode           | attaches to the hard link                                  |
| everyfile has a unqiue inode |                                  | you can have multiple hard links | if you remove the hardlink the symbolic link will not work |
|                              |                                  |                                  | This is a depency on the hard link                         |
|                              |                                  |                                  | symbolic links will obtain a unique inode                  |

# ARCHIVING FILES
---
- **tar** basic uses:
	- `tar -cvf my_archive.tar /home/etc` will create an archive
	- `tar -tvf` will show contents of an archive
	- `tar -xvf my_archive` extracts to the current directory
		- by default it will extract to the current directory. 
		- `-C` to switch extraction to a different path
	- to add compression:
		- `-z`: gzip
		- `-j`: bzip
		- `-J`: xz
- **tar example**:
	- `tar -cvf /tmp/archive.tar/home /etc`
		- creates a archive that will contain the `/home`and `/etc` directories
	- `tar -czvf /tmp/archive.tgz /home /etc`
		- compresses that arhive with gzip
# WORKING WITH COMPRESSED FILES
---
- gzip `-z` most common compression
- bzip2 `-j` an alternative compression
- xz `-J` is showing up more often
- zip is also available and has windows avaible syntax