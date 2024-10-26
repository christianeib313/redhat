___
# DISK LAYOUT
---
- `lsblk` shows disk layout
## COMMON BLOCK DEVICES
---
- `/dev/sdx` is commong for SCSI and SATA disks 
- `/dev/vdx` is used in KVM virtual machines
- `/dev/nvmexny` is used for NVME devices
- *the x is what ever device you have*
## GPT AND MBR PARTITIONS
---
**MBR**: Master Boot Record
- 512 bytes to store information
- 64 bytes to store partitions
- place for 4 partitions only with a max size of 2 TiB
- to use more partitions, extended and logical partitions must be used
**GUID**: GPT Partition Table
- More space to store partitions
- Used to overcome MBR limitations
- 128 partitions max
## CREATE PARTITIONS WITH `FDISK`
---
`fdisk /dev/sda` to use the fdisk utilist on the device
use `m` to see a list of useful commands in fdisk
`n` to add a new partition
# MOUNTING FILESYSTEMS
---
#### Filesystem Types:
- **XFS**: is the default filesystem
	- fast and scalable
	- uses CoW (Copy on Write) to guarantee data integrity
	- size can be increased, not decreased
- **Ext4**: was default in RHEL6 and is still used
	- backward compatible to Ext2
	- Uses jounrla to guarentee data integrity
	- size  can be increased and decreased
- **vfat**: offers multi=OS support
	- used for shared devices
- **Btrfs**: is a new and advanced filesystem
	- not installed by default on RHEL
#### Creating Filesystems
- `mkfs.xfs`: creates an XFS filesystem
- `mkfs.ext3`: creates an Ext4 filesystem
- `mkfs.vfat`: creates a vfat filesystem
- **TIP**: use `mkfs.[Tab][Tab]` to show a list of available filesystems
- ![[mkfs-tab-tab.png]]
#### Mounting
---
`mount` command to mount 
`findmnt`: shows filesystem hierarchy
- ex.
	- `mount /dev/sda1 /mnt`
		- sets the mountpoint for sda1 to /mnt directory
- 
**TIP**: if you see an error "wrong fs type" this is because you are trying to mount before creating a filesystem

###### Persistently mount partitions:
- `/etc/fstab` is the main configuration file to persistently mount partitions

| device you want to mount | mount point | filesystem | filesystem defaults |
| ------------------------ | ----------- | ---------- | ------------------- |
| `/dev/sdb1`              | `/data`     | ext4       | 0 0                 |
- this is the format you'll use when you vim into `/etc/fstab`
##### Troubleshooting fstab:
- if you make an error in the fstab and don't know what to do
- reboot - jump into grub bootloader menu by holding shift
- remove `rhgb` and `quiet` at the end of the line that starts with linux
	- you will now see what is happening and see any errors
- once the root shell is available you'll be able to vim into `/etc/fstab` and make any changes that you need
- ctrl-D or type exit to continue boot process
- 
