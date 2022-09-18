---
title: Merge 2 Partitions Into 1
description: A short guide about merging your two mistakes into one larger mistake.
slug: unify-partitions
date: 2021-03-16 21:21:12+0000
---

# First and Foremost...
I am not responsible over whatever shit that might occur during your migration
process. What I am doing in this guide is documenting the process of the
migration for my two previous partitions into one unified partition for the
sake of seeking this guide when required.

# Getting Started
/Have you ever gotten into a situation where you have to clean your /root
partitions cache constantly because it keeps getting full? And by any chance,
did you create a separate /home and /root partition which you now regret doing?
If so then this guide is for you!/

> **Note**
> Before proceeding with anything you need to know this: *do not remove more
> than the /home partition after backing it up in another partition and it's
> also advised that you back up both the partitions in another partition than
> /root!*

> **WARNING**
> Before we start off with the guide I need to clarify this that you should
> apply the commands based on your partition scheme and that this guide is
> written based on my partition scheme! So please, be cautious and follow this
> guide with the help of an experienced person if you do not understand what
> you should do!**

## Current Partition Scheme
```bash
[arch@archlinux ~]$ lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
loop0    7:0    0  87.9M  1 loop /var/lib/snapd/snap/core/5548
sda      8:0    0 465.8G  0 disk
├─sda1   8:1    0   300M  0 part
├─sda2   8:2    0   100M  0 part
├─sda3   8:3    0   128M  0 part
├─sda4   8:4    0 232.7G  0 part
├─sda5   8:5    0   550M  0 part /boot
├─sda6   8:6    0    12G  0 part [SWAP]
├─sda7   8:7    0    20G  0 part /
└─sda8   8:8    0   200G  0 part /home
sr0     11:0    1  1024M  0 rom
```

# The surgery
## IMPORTANT
- Make sure to remove cached files which you won't be needing for this
  procedure to reduce the amount of time required to backup your files!

- Backup `/home`: will later be used to restore all the files after home
  partition (`/dev/sdaX`) has been removed and turned into anllocated space.
  + `sudo tar czpvf /PATH/TO/BACKUP_PARTITION/home.tar.gz -C /home .`

- Backup `/root`: if something wrong happens throughout the process, this
  backup would be used to ensure that the process is still replicatable.
  + *Reboot into a LiveUSB* to prevent file loss during the backup process.

## Before Backup
- *If you want to backup your files in your dual-boot OS* (Windows): you need
  to install ntfs-3g which will allow you to mount the Windows partition
  without problems.
  + `pacman -S ntfs-3g`.

- `mkdir /x; mount /dev/sdaX /x` (Replace: /X/ = partition, *x* = folder-name).
- `mount /dev/sdaX /mnt` (/X/ = root-partition), do not mount `/home` otherwise
  it will backup alongside the `/root` backup of the partition.

- `tar zcvpf /PATH/TO/BACKUP_PARTITION/root.tar.gz -C /mnt .`
- *When the backup has finished without returning errors* => reboot and later
  check the files in the new archived folder to confirm that the files had
  indeed finished the backup process without trouble.

- Reboot to LiveUSB or just ignore this and finish the rest of this guide.

## After Backup
1. Boot into your LiveUSB.
2. Confirm that you are either using GPT or MBR through `gdisk /dev/sda`.
3. Type `p` -> hit enter and you should be greeted with the current partition
   scheme.

4. Type `d` to remove partition and then `X` (replace /X/ with the number for
   your `/home` partition)

5. Repeat step 3, but replace *X* with the number for the `root` partition.
   a. If the process was successful, there should be no file loss from your
      `root` partition due to the creation of a merged partition rather than the
      deletion of the partition, which would transfer the files to the new
      partition instead of removing them.

6. *During the writing of this guide, **I had used two EXT4 partitions** for both
   partitions*, therefore you should note that the method is different for other
   partition types. (Hexcode number: 8300)

7. Type ~n` to create a new partition and later choose the number for your
   partition with the block size set from the `start sector` to the `end sector`.
   a. Double check your `start sector` and your `end sector`.

8. Type `p` to check the partition scheme and ensure that the new partition =
   `/home + /root` size and make sure to check the Hexcode number for the
   partition is the same as it previously was. (Hexcode number = 8300).

9. If you are satisfied with the new partition, type `w` to write the new
   partition.

10. Expand the filesystem using `resize2fs`:
    a. `resize2fs /dev/sdaX`, where *X* = number of the new created partition.
    b. If it returns `e2fck -f /dev/sdaX``, then execute that command + the
       `resize2fs` command afterwards.

11. Mount the newly expanded partition to `/mnt`:
    a. `mount /dev/sdaX /mnt`
    b. Do: `ls /mnt` to check if your old `/root` files are still present.

12. `mkdir /x; mount /dev/sdaX /x` and locate your saved `/home.tar.gz` file
    using `cd`.

13. `tar -zxpvf /PATH/TO/home.tar.gz -C /mnt/home` to unpack your files to
    `/home` directory.

14. Remove old `/home` partition from `fstab` from another window,
    `CTRL+ALT+F3`, through `nano /mnt/etc/fstab` + remove old lines for `/home`.

# Lastly
 1. To ensure that the process is working as expected do `ls -l /mnt/home/`.
    a. It should **not** be `0` because the old files in `/home` have
       transferred to the new `/root` partition!

 2. Return to the window where you ran the tar command through `CTRL+ALT+F3`,
    when the process has finished run `sync`.

 3. Check your files through `ls` and if you are satisfied.... `reboot`.

# Good Job!
Now you have one unified partition rather than two :P
