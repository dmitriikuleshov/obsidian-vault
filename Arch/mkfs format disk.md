**pacman -S dosfstools** gets you _mkfs.vfat_ and _mkfs.msdos_  
**pacman -S ntfsprogs** gets you _mkfs.ntfs_

To see all disks:
```
lablk
```

To format a disk:
```sh
mkfs.ext4 /dev/{disk_name}
```