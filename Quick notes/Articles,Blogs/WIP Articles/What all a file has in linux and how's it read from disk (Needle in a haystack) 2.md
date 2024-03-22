Keeping in mind what all we discussed in last article here() let's understand how a file fits in LINUX ecosystem. Let's take a brief overview of something which hosts the file i.e. `FileSystem`.
The files are an integral part of the filesystem and cannot exist as standalone entities. Since it's the filesystem that act as a container for these files. Filesystems on their own are a separate discussion but for now we can think of them as something which is implemented/provided to user by OS kernel(there can also be filesystems implemented in userspace such as FUSE).
Below are important parts of a kernel filesystem in Linux:
- **Drives** - A (physical) block device such as HDD/SSD/NVMe etc.
- **Partitions** - Splitting drives into partitions, a set of storage sectors. e.g. `/dev/sda1, /dev/sda2`
- **Super block** - These are stores metadata about the filesystem itself, such as its type, size, block size, and other configuration parameters. Think of it this way all these partitions etc. we're doing it must be stored somewhere so that filesystem can refer to it at startup, so here it's stored.
- **Inode and Inode Cache** - Inode is the structure which stores data all data related to files like ownership, created date, size, pointers to data blocks etc. It doesn't store filename and the actual data. The Inode cache is just a bunch of recently/frequently accessed inodes stored for faster access.
- **Buffer cache** - Caches recently accessed data blocks.
- **Filesystem Drivers** - You would have heard of ext4, XFS, FAT etc. correct these are nothing but drivers/softwares to store and access data from drives(HDDs/SSDs).
- **Virtual File System (VFS)** - Now this is something very important, it's basically an abstraction layer. We can think of it this way that there are multiple types of filesystem drivers correct basically different ways to store data on HDD. They would have their different implementations but to unify them all this VFS provides an interface for common functions like `read`, `write`, `open`, `close` etc. so that kernel can easily utilise these calls.
This not it there are other parts also of kernel filesystem like File Descriptors, File Tables, Directory Entries etc. but we can skip these for now otherwise this would be an article on Filesystem itself ;-)

Let's correlate all the above on how it actually pans out using a diagram [here](obsidian://open?vault=obsidian&file=Excalidraw%2FDrawing%202024-03-23%2001.45.48.excalidraw) 
If we summarise this below are the points, which can help you understand the how a file is actually stored:
1. Drives(HDDs/SSDs) hold the file in blocks.
2. We partition these drives, one easiest reason you can think of is to keep the boot/startup files separate.
3. To interact with these partitions on drives we have filesystem drivers i.e. ext4, FAT, XFS etc.
4. Since there are multiple implementation of the drivers Kernel needs a uniform way to interact with them to get data so VFS provides that uniform layer.
In diagram if you see VFS as separate don't get confused it's still part of Kernel :) but often conceptually depicted as a separate layer for clarity and to emphasise its role as an abstraction layer between the kernel and filesystem implementations.

I know it's a heavy read but hope you liked it, we'll continue this journey in next part to delve more into how the Linux file is actually structured, what are the metadatas, permissions around it etc.



`read` system call blocked and kernel tries to figure out what command to send, what virtual file system to pick, block device mapping file system, getting page cache, send command to NVMe/HDD/SSD, after getting blocks, storing in cache in file system page cache.
Getting bytes from page cache to requested userspace memory. Blocking meanwhile doing all this work.