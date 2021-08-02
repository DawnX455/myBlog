---
layout: post
title:  "Swap Space"
date:   2021-04-19 07:28:30 -0500
categories: operating systems
---
persistent storage is used to keep data intact even if there is a power loss, eg. hard disk drive and solid-state storage device.

Two key abstractions in the virtualization of storage: file and directory.

File:

    constructed by a linear array of bytes

    each file has low-level name as inode number

    filesystem stores data persistently on disk

Directory:

    contains a list of (user-readable name, low-level name) pairs

    each entry in a directory refers to file or directory

    eg: ("foo", "10") means a file "foo" with the low-level name "10"

Creating files:

    int fd = open("foo", O_CREAT|O_WRONLY|O_TRUNC);

    flags:

        O_CREAT: create file

        O_WRONLY: only write to that file while it is opened.

        O_TRUNC: if the file already exists, remove any existing content to make the file size zero.
    
    return value:
        fd: file descriptor

File Descriptor is an integer to access files. Each process has its own file descriptors, meaning that file descriptors are private per process. 


