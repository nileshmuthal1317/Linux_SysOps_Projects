# Standard, SWAP, LVM Storage

**Creating Standard Volume**

**`# fdisk -l`**

**`# df -PTh`**

**`# lsblk`**

**`# blkid`**

**`# fdisk /dev/xvdb n (e,p) default (sector 2048) default +2G t l 8e(lvm) 82(swap) p w`**

**`# partprobe`**

**`# fdisk -l`**

**Creating Physical Volume**

**`# pvcreate /dev/xvdb1`**

**Creating Volume Group**

**`# vgcreate vgone /dev/xvdb1`**

**`# pvs`**

**`# vgs`**

**Creating Logical Volume**

**`# lvcreate -L +1G -n lvone vgone`**

**`# lvs`**

**`# mkfs.ext4 /dev/vgone/lvone`**

**Mounting LVM**

**`# mount /dev/vgone/lvone /lvone`**

**`# df -kh`**

**Extending Volume Group**

**`# create lvm partition /dev/xvdb2`**

**`# pvcreate /dev/xvdb2`**

**`# vgextend vgone /dev/xvdb2`**

**Extending Logical Volume**

**`# umount /dev/vgone/lvone /lvmone`**

**`# lvextend -L +1G /dev/vgone/lvone >> how much space you want to add`**

**For EXT4 Filesystem**

**`# e2fsck -f /dev/vgone/lvone >> check for error`**

**`# resize2fs /dev/vgone/lvone >> increase the fs`**

**For XFS Filesystem**

**`# xfs_repair /dev/vgone/lvone >> check for error`**

**`# xfs_growfs /dev/vgone/lvone >> increase the fs`**

**`# lvs`**

**`# mount /dev/vgone/lvone /lvmone`**

**Reducing Logical Volume**

**`# umount /dev/vgone/lvone /lvmone`**

**For EXT4 Filesystem**

**`# resize2fs /dev/vgone/lvone 500M >> actual required size`**

**`# lvreduce -L -500M /dev/vgone/lvone`**

**`# e2fsck -f /dev/vgone/lvone >> check for error`**

***** For XFS Filesystem**

**`# xfs_growfs /dev/vgone/lvone -D 500M >> actual required size`**

**`# xfs_repair /dev/vgone/lvone >> check for error`**

**`# mount /dev/vgone/lvone /lvmone`**

**SWAP Volume**

**`# For standard partition simply select the 83 format it and mount the same`**

**`# For swap partition select 82 format`**

**`# mkswap /dev/xvdb3`**

**`# swapon /dev/xvdb3`**

**`# free -m`**

**Configuration File**

**`# cat /etc/fstab`**

**`UID=2805e7c1-ff1a-439c-8fc0-cf37ea535d6b none swap defaults 0 0`**

**`UUID=088a2335-8767-492a-9e57-b74482db7d19 /lvmone ext4 defaults 0 0`**