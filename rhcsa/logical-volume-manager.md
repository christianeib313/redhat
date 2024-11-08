mkfs# Overview
---
- **LVM**:
	-  volumes are no longer bound to the restriction of physical hard drives. 
	- If additional storage is needed, the volume group can easily be extended by adding a new physical volume.
###### LVM setup:
---

| Logical Volumes      | you can have as many Lv's as you want and these are what hold your ***filesystems***                                                                                       |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Volume Group**     | **This is where all of your logical volumes will pull storage space and this is where the flexibility comes from since logical volumes can aquire more storage if needed** |
| **Physical Volumes** | **All of your physical volumes will be added to the volume group**                                                                                                         |
## Creating an LVM Logical Volume
---
1. We first need to create a parition and set the type to **lvm**
	- use` fdisk `
2. `pvcreate /dev/dev-name` to create a physical volume
3. `vgcreate` to create the volume group
4. `lvcreate -n <lv-name> -L <size> /dev/volume-group to create a logical volume.`
	1. *it's important to remember `-L or -l` these options because it allows you to also increase the size of the filesystem at the same time*
		1. `-L`: Size
		2. `-l`:Extents
5. `mkfs` to create a filesystem
6. `/etc/fstab` to mount persistently
**fstab syntax:**

| device-name                        | mount point             | filesystem                 | options  |     |
| ---------------------------------- | ----------------------- | -------------------------- | -------- | --- |
| `/dev/volume-group/logical-volume` | /fs-mountpoint-you-made | anything but recomend ext4 | defaults | 0 0 |

###### Extents:
- extent size can be defined while defining the volume group
- `vgcreate -s <size-of-extent> <volume-group-name> /dev/dev-name` to create a volume group with an extent size of 8MB
- LVM logical volumes built from the volume group with use the same extent size
- `vgdisplay` to show properties of volume groups, including the extent size.
## Resizing Logical Volumes
---
- `vgs` to verify volume group has unused disk space
- if required, `vgextend` to add more PVs to the VG
- `lvextend -r -L +<size>` to grow the LVM logical volume, and the including filesystem it's hosting.
######  resizing example:
- Create 2 partitions with 1GiB each and set LVM partition type
- `vgcreate vgfiles /dev/sde1`
- `lvcreate -l 255 -n lvfiles /dev/vgfiles`
- `mkfs.ext4 /dev/vgfiles/lvfiles`
- `df -h`
- `vgs` shows no available extents in the volume group
- `vgextend vgfiles /dev/sde2` adds a new PV to the VG
- `lvextend -r -l +50%FREE /dev/vgfiles/lvfiles` is now possible
- `df -h` shows the newly available space in the volume