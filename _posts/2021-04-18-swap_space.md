---
layout: post
title:  "Swap Space"
date:   2021-04-18 22:12:21 -0500
categories: operating systems
---
why supports a single large address space for a process?

Because it is convenient that we won't worry about whether the space is enough or not.

Swap Space: space reserved on the disk for moving pages back and forth.

How do the hardware determine the page is not present in physical memory?

Present bit: 

    if the present bit = 1, it means that the page is present in physical memory

    if the present bit = 0, it means that the page is not in memory but rather on distk somewhere(causing a page fault).

Page Fault: accessing page that is not in physical memory

Page-fault handler: 

    OS swaps the page into memory in order to service the page fault.
    
    How? OS looks in the PTE to find the address, and issues the request to disk to fetch the page into memory.


