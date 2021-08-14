---
layout: post
title:  "Solution to VirtualBox Crash Because of 100% Usage of /dev/sda5"
date:   2021-08-14 10:22:02 -0500
categories: Linux
---
# Solution to VirtualBox Crash Because of 100 % Usage of /dev/sda5

Theses days, my Virtual machine crashes because /dev/sda5 has used 99%. The virtual machine just shows nothing but black when I launched it. Now I have fixed it and write this article to provide the thought of how to solve this problem.

- Use `GRUB` to check the memory
- Mount `/home` to other hard disk.

## How to Find the Problem ##

We can use `GRUB` -- hold down `Shift` when you launch the VM. Then you will see what is shown below.

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition1.png" alt="image-20210813184045826" style="zoom:20%;" />

Then choose `Advanced options for Ubuntu` and `recovery mode`.

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition2.png" alt="image-20210813184216442" style="zoom:20%;" />

Now we come to the `Recovery Menu`, and choose `root`.

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition3.png" alt="image-20210813204630614" style="zoom:20%;" />

Input password

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition4.png" alt="image-20210813204726618" style="zoom: 25%;" />

Use `df -h` to check the usage of memory

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition5.png" alt="image-20210813204823671" style="zoom: 25%;" />



## Linux Partition ##

Partition is a tench to protect the security of data. Dividing the hard disk in partition, data can be grouped and separated. If the accident happens, one partition will be affected but others won't be. But, if a disk contains only one big partition, the system will crash if this partition is full.

### Linux Hard Disk

Linux hard disk can be divided into IDE and SCSI, the current primary one is SCSI.

IDE hadr disks are indicated by `hdx~`, where `hd` means this partition uses IDE hard disk, `x` represents the order of devices from `a` to `z`, `~` is the number signifies the partition on the device.

It is same with SCSI device which is identified by `sdx~`.

For example, `/dev/sda5` means the 5th partition on the first drive.

### Partition Layout and Types ###

Data partition: normal Linux system data, including the root partion containing all the data to start up and run the system.

Swap partition: expansion of computer's physical memory, extra memory or hard disk.

### Mount Point ###

Mount point defines the place of a particular data set in the file system. All partitions are connected through the root partition.

Partition is a concept in hard disk, while directory is an abstract one in Linux file system. So how can these two things are connected? The answer is mount point. In Linux, we use mount point to make a hard disk partition point to one directory.

### View the Status of Partition ###

`lsblk`: list block devices. It will list information about all available or the specified block devices:

- `-a`: list all devices include empty ones.
- `-b`: print the SIZE column in bytes rather than in a human-readable format.
- `-f`: Output info about filesystems.

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition6.png" alt="image-20210813222942724" style="zoom:20%;" />

we can see that there are sda and sr0(cd-rom), and sda are divided into `sda1`, a vfat type, UUID AA24-31BB, 511MB available, 0% used and mounted at `/boot/efi`; `sda2` is not mounted, so there are not UUID, available size, usage, and mount point; `sda5`, an ext4 type, UUID 30c2da...., 665.1M available, 88% used, and mounted at `/`.

If we want to check the SIZE of each block, we can use `lsblk` directly. 

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/inuxpartition7.png" alt="image-20210813223715042" style="zoom:33%;" />

## How to Mount a Directory to Disk ##

There we will mount `[swap]` and mount `/home` to a new partition.

**Mount `[swap]`** (mount swap space is not a direct method for fixing the problem, it is menthioned here because of my interest, if you want to fix your problem, you can jump to mount `/home`):

**What is Swap space?** Swap space in Linux is used when the amount of pyhiscal memory(RAM) is full. If the system needs more memory resources and the RAM is full, inactive pages in memory are moved to the swap space. While swap space can help machines with a small amount of RAM, it should not be considered a replacement for more RAM. Swap space is located on hard drives, which have a slower access time than physical memory.

**Steps for Mounting Swap Space in VirtualBox**

1. Shutdown the running VM and add a disk for swap space. Choose the VM you want to assign the disk and click `Settings`.

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition8.png" alt="image-20210813225321726" style="zoom: 40%;" />

   

2. choose `Storage`, and click `Adds hard disk`.

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition9.png" alt="image-20210813225626168" style="zoom:40%;" />

   

3. Choose `Create` and create a VDI hard disk.

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition10.png" alt="image-20210813232249403" style="zoom:50%;" />

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition11.png" alt="image-20210813232325578" style="zoom:50%;" />

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition12.png" alt="image-20210813232349174" style="zoom:50%;" />

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition13.png" alt="image-20210813232425427" style="zoom:50%;" />

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition14.png" alt="image-20210813232506244" style="zoom:50%;" />

   and now we can see the sdb1.vdi is under the `Controller.SATA`

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition15.png" alt="image-20210813232551516" style="zoom:50%;" />

   

4. Launch the VM.

   Click the `Disks` in the menu

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition16.png" alt="image-20210813233020983" style="zoom: 33%;" />

   we can see the `sdb` disk here

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition17.png" alt="image-20210813233114191" style="zoom: 40%;" />

   we can also use `lsblk` to list the information of filesystem, especially we use the `GRUB`.

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition18.png" alt="image-20210813233227211" style="zoom:50%;" />

   

5. Use `fdisk` for partition.

   **fdisk** also known as format disk is a dialog-driven command in Linux used for creating and manipulating disk partition table. It is used for the view, create, delete, change, resize, copy and move partitions on a hard drive using the dialog-driven interface. 

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition19.png" alt="image-20210813234142922" style="zoom:50%;" />

   There we use `n` to add a new partition, and `w` to write table to disk and exit.

   Now we check the filesystem information.

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition20.png" alt="image-20210813234319878" style="zoom:50%;" />

   We can see there is `sdb1` without `FSTYPE`, `LABEL`, ...

6. Use `mkswap` to set the swap space.

   `mkswap /dev/sdb1`

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition21.png" alt="image-20210813235809957" style="zoom:50%;" />

7. Make swap space effective.

   `weapon /dev/sdb1`

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition22.png" alt="image-20210813235854954" style="zoom:50%;" />

8. Set auto mounting.

   Now it seems we have mounted the swap space, but if we shut down the VM, we need to mount again. So we should modify the `/etc/fstab` to set auto mounting.

   `vim /etc/fstab`

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition23.png" alt="image-20210814003048022" style="zoom:40%;" />

**Mount `/home`** :

**Although there I only provide the way for remounting /home to fix this problem, you can check the size of directory in `/` by `sudo du -sh /*` to find the largest directory and mount it as we mount `/home`, or you can just remount `/` on a larger disk**.

**Why should we mount /home? ** In Linux, the system accounts except `root` user directories are in `/home` like the figure shown below (There are 4 accounts-- `rick`, `anna`, `emmy` and `bob`), so this `/home` will grow in big size. When `/home` is full, it can cause critical problems on the root file system which may make the boot failure.

**Steps for Mounting /home in VirtualBox**

1. Create a new disk like mentioned in mounting swap space.

2. Use `fdisk` for partition.

3. Now we should create a new directory `/srv/home` to backup `/home`.

   ```shell
   mkdir -p /srv/home #create /srv/home directory
   mount /dev/sdc1 /srv/home #mount /srv/home for backup
   rsync -av /home/* /srv/home #move the content of /home into /srv/home
   diff -r /home /srv/home #check the difference between /home and /srv/home
   rm -rf /home/*
   ```

4. The content has been moved to `/srv/home` which is mounted on our new partition `/dev/sdc1`, it means that we moved the content of `/home` from `/dev/sda5` to `/dev/sdc1`. So we should remount the `/dev/sdc1` on `/home`. 

   ```shell
   umount /srv/home
   mount /dev/sdc1 /home
   ```

5. Modify the `/etc/fstab` and make it effective.

6. Check the information of filesystem. 

   We can see the `/` changes to 73% usage. 

   <img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition24.png" alt="image-20210814095606560" style="zoom:50%;" />

### Linux File System Layout ###

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition25.png" alt="image-20210813212711214" style="zoom: 33%;" />

This figure comes from `Introduction to Linux`, showing the file system layout. The `/` is called `root directory`. 

The table below also comes from `Introduction to Linux`, explaining the subdirectories of the root directory.

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition26.png" alt="image-20210813212932195" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/DawnX455/myBlog/main/assets/images/linuxpartition27.png" alt="image-20210813212953034" style="zoom:50%;" />

