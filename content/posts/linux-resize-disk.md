---
title: 'Linux服务器分区扩容'
date: '2024-03-27T11:02:06+0800'
categories: []
tags: ["Linux"]
---

{{< notice warning >}}
请务必备份数据！！！
{{< /notice >}}

事情是这样的，服务器系统盘是块 120GB 的 SSD，当时装系统的时候只给了 50GB，还剩下 70GB 的剩余容量，那么现在由于东西越来越多，需要把剩下的 70GB 容量也用上

> _这个笔记只适用于同一块硬盘扩容_

## 过程：

可以看到，根目录只有 50GB

```markup
root@localhost:~# df -h

Filesystem Size Used Avail Use% Mounted on
udev 3.9G 0 3.9G 0% /dev
tmpfs 798M 1.1M 797M 1% /run
/dev/vda2 49G 39G 8.0G 83% /
```

还有 70GB 目前没用

```markup
root@localhost:~# fdisk -l

Disk /dev/vda: 120 GiB, 128849018880 bytes, 251658240 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: 0A82F7CF-FB16-4A55-ACF6-5D4339FA7608

Device Start End Sectors Size Type
/dev/vda1 2048 4095 2048 1M BIOS boot
/dev/vda2 4096 104855551 104851456 50G Linux filesystem
/dev/vda3 104855552 251658206 146802655 70G Linux filesystem
```

选择 /dev/vda，并且输入p（print）

```markup
root@localhost:~# parted /dev/vda

GNU Parted 3.2
Using /dev/vda
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) p  # 输入p
Model: Virtio Block Device (virtblk)
Disk /dev/vda: 129GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number Start End Size File system Name Flags
1 1049kB 2097kB 1049kB bios_grub
2 2097kB 53.7GB 53.7GB ext4
```

输入 resizepart 2，输入 yes，End 这就填写上一步看到的磁盘空间(Disk /dev/vda: 129GB) 129GB，这个应该是换算出了问题，最后再输入 q (quit) 退出

由于我是系统盘，这个 /dev/vda2 是活动中的分区，会有警告是否要继续，我这里是继续。

```markup
(parted) resizepart 2
Warning: Partition /dev/vda2 is being used. Are you sure you want to continue?
Yes/No? yes
End? [53.7GB]? 129GB
(parted) q
Information: You may need to update /etc/fstab.
```

退出之后，用 df -h 命令看发现没有任何改变，但是使用 lsblk 命令会发现已经扩容成功了，因为这只是 block device 容量变大了，还没有反映到 file system 中

需要使用 resize2fs 命令更新

```markup
root@localhost:~# resize2fs /dev/vda2
resize2fs 1.44.1 (24-Mar-2018)
Filesystem at /dev/vda2 is mounted on /; on-line resizing required
old_desc_blocks = 7, new_desc_blocks = 15
The filesystem on /dev/vda2 is now 31456763 (4k) blocks long.
```

这个时候用 df -h 查看

```markup
root@localhost:~# df -h
Filesystem Size Used Avail Use% Mounted on
udev 3.9G 0 3.9G 0% /dev
tmpfs 798M 1.1M 797M 1% /run
/dev/vda2 118G 39G 75G 35% /
```

lsblk 查看

```markup
root@localhost:~# lsblk
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
loop1 7:1 0 6.6M 1 loop /snap/libxslt/44
loop2 7:2 0 91.3M 1 loop /snap/core/8592
loop3 7:3 0 9.6M 1 loop /snap/libxslt/67
loop4 7:4 0 91.4M 1 loop /snap/core/8689
vda 252:0 0 120G 0 disk
├─vda1 252:1 0 1M 0 part
└─vda2 252:2 0 120G 0 part /
```

the end
