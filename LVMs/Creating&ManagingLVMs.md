#NOT MINE -- GRABBED HERE: https://www.cyberciti.biz/faq/linux-mount-an-lvm-volume-partition-command/

Linux mount an LVM volume
If lvm2 not installed on your system, install it as per your Linux distro.
```
  $ sudo yum install lvm2
```

How to mount LVM partition in Linux
The procedure to mount LVM partition in Linux as follows:

1. Run vgscan command scans all supported LVM block devices in the system for VGs
2. Execute vgchange command to activate volume
3. Type lvs command to get information about logical volumes
4. Create a mount point using the mkdir command
5. Mount an LVM volume using sudo mount /dev/mapper/DEVICE /path/to/mount

How to mount an LVM volume
Type the following command to find info about LVM devices:
```
$ sudo vgscan
```
OR
```
$ sudo vgscan --mknodes
```

To activate it run:
```
$ sudo vgchange -ay
```
OR
```
$ sudo vgchange -ay LVM_GORUP_NAME
```

Activate the LVM volume using the vgchange command
You can run the following command to list it:
```
$ sudo lvdisplay
```
OR
```
$ sudo lvs
```

You can get good look at using the ls command:
```
$ ls -l /dev/LVM_GROUP_NAME/
        sample outputs:
        total 0
        lrwxrwxrwx 1 root root 7 Aug 17 15:47 home -> ../dm-1
        lrwxrwxrwx 1 root root 7 Aug 17 15:47 root -> ../dm-2
        lrwxrwxrwx 1 root root 7 Aug 17 15:47 swap -> ../dm-0
```

Mount an LVM partition
Create a mount point using the mkdir command:
```
$ sudo mkdir -vp /path/to/mountpoint
```

Mount both home and root logical volume from LV path using the following syntax:
```
$ sudo mount {LV_PATH} /path/to/mount/point/
  LVM Path as seen in lvdisplay. Typically /dev/LVM_GORUP_NAME/LVM
```

Verify it with the help of df command or grep command:
```
$ df -T
```

How to mount an LVM volume and verify it on Linux
Click to enlarge image
Update /etc/fstab
Update /etc/fstab file if you want a logical volume to be mounted automatically on boot:
```
/dev/mapper/LVM /path/to/mounpoint xfs defaults 0 0
```
      See the /dev/mapper/path with dh -h or df -T
