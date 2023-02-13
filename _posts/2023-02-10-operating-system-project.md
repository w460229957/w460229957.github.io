---
title: "Operating System Project"
date: 2023-02-10 00:00:00 +0800
categories: [project, operating-system]
tags: [operating-system]
---


# A Linux Kernel Sub-System for a software-implemented Remote Address Mapping and Synchronization

## Student Information and Project Proposal:

**Student Name**: ZhengYuan Zhang

**Proposal:** The project aims to develop a sub-system for the Linux kernel that provides a remote address mapping and synchronization mechanism. This sub-system will enable user-space programs, both locally and remotely, to share memory between different processes efficiently. It will also facilitate the synchronization of memory access between the processes, ensuring data consistency and avoiding race conditions.

**Purpose**: To create a more user-friendly and centralized memory access model for inter-process communication.

## Objectives:

**Objectives**: 

- Develop a Linux kernel sub-system for remote address mapping and synchronization.

- Design and implement a user-space API for using the sub-system.

- Test and evaluate the sub-system for performance, scalability, and performance.

## Tentative Approach:

**Tentative approaches:** The proposed sub-system will be implemented as a loadable kernel module that provides a set of device files for memory access from user spaces. Ideally, those files should be presented under Following the Unix tradition of "everything is a file," it should support everyday open(), read(), and write() operations**. The task should be similar to how we implemented the page fault handler last semester in CSE 422S.**  the sub-system will try to mimic RDMA (Remote Direct Memory Access) protocol. Finally, the sub-system will also provide a synchronization mechanism to ensure data consistency and avoid race conditions**. This part should be the minimum requirement of the project, considering its allowed time and expected complexity and difficulty.**

The user-space API for the sub-system will be designed and implemented with a user-space C++ Daemon process listing to a given port on the Linux system. The process should act like HTTP protocol and any protocol at the application layer, providing a specific communication end-to-end mechanism between the remote and the local end system. For the project's simplicity, the underlying transport layer protocol is TCP. In addition to the remote addressing mapping, the C++ Daemon process should create log files in the /etc/RDMA_test location. Why not C? C++ provides an STL library for a better start-up. Furthermore, although TCP has congestion control, sometimes the entire system may be impeded by the slowest communication link; **the protocol is mainly designed for private network communication in which there is no external network traffic on the link.** 

The sub-system will be tested and evaluated for performance and scalability using a set of benchmark applications that simulate real-world use cases. **The tests will host on Raspberrypi-4B for local inter-process communication and windows 10 x86_64 architecture for remote inter-process communication.** 


## Tentative Tasks:
**Test Cases**:

A possible test case includes using two processes to share a memory sequence and then do matrix multiplication with the allocated memory. One process can first try to call **open()** on the device file, which returns an integer indicating the entry of shared memory segments the process owns. Then, for any following **write ()** with the same entry number of shared memory segments, they can both try to write and read the shared memory. 

Process1:

​	int buf[100];

​	....Initialize buffer

​	int entry_num = open(device_file_of_the_module,O_CREATE...)->Most flags will be ignored

​	write(entry_num,buf,nbytes)->should have no data inconsistency

Process2:

​	int buf[100];

​	int result[200];

   ....Initialize buffer

​	write(entry_num,buf,nbytes)->return -EFAULT if possible data race may happen.

​	ioctl(device_file,LOCK,....)->Arguments to specify the locked memory size, and times of write.

​	write(entry_num,buf,nbytes)->after times of write, the lock is automatically released.

   read(entry_num,buf,200)->should have no data inconsistency



 However, considering Raspberrypi-4B only have four cores, the maximum number of processes involved in the computation should not exceed 20. it may also be worth using **TSC or Linux performance_event series of system calls** to measure the execution and put testing software in a container to measure their overall performance.

**Expected Outcomes**: 

1. A Linux kernel sub-system for remote address mapping and synchronization. (Required)
2. A user-space API for using the sub-system. (Required, but there may be a more straightforward approach)
3. Performance and scalability test results and analysis. (Required)



## Tentative Schedule;

**Week1~Week3**:Kernel Module implementation

**Week3~Week6**:User Space implementation

**Week7~Final**: Final testing and integration 

