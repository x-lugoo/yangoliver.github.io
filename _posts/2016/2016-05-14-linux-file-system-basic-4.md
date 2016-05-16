---
layout: post
title: Linux File System - 4
description: Linux file system(文件系统)模块的实现和基本数据结构。关键字：Ext4，文件系统，内核，samplefs，VFS，存储。
categories: [Chinese, Software]
tags:
- [file system, driver, crash, kernel, linux, storage]
---

>本文首发于 <http://oliveryang.net>，转载时请包含原文或者作者网站链接。

>本文仍处于未完成状态，内容随时可能修改。

* content
{:toc}

## 1. 背景

本文将在 Sampleblk 块设备上创建 Ext4 文件系统，以 Ext4 文件系统为例，用 Crash 来查看 Ext4 文件系统的磁盘格式。

在 [Linux File System - 3](http://oliveryang.net/2016/02/linux-file-system-basic-3) 中，Samplefs 只有文件系统内存中的数据结构，而并未规定文件系统磁盘数据格式 (File System Disk Layout)。
而 [Linux Block Driver - 1](http://oliveryng.net/2016/04/linux-block-driver-basic-1) 则实现了一个最简的块驱动 Sampleblk。 
Sampleblk [day1 的源码](https://github.com/yangoliver/lktm/tree/master/drivers/block/sampleblk/day1)只有 200 多行，但已经可以在它上面创建各种文件系统。
由于 Sampleblk 是个 ramdisk，磁盘数据实际上都写在了驱动分配的内核内存里，因此可以很方便的使用 Linux Crash 工具来研究任意一种文件系统的磁盘格式。

## 2. 准备

### 2.1 Ram Disk 驱动

需要按照如下步骤去准备 Sampleblk 驱动，

* Linux 4.6.0 编译，安装，引导
* Sampleblk 驱动编译和加载
* 在 `/dev/sampleblk1` 上创建 Ext4 文件系统。
* mount 文件系统到 /mnt 上

以上详细过程可以参考 [Linux Block Driver - 1](http://oliveryng.net/2016/04/linux-block-driver-basic-1)。

### 2.2 调试工具

需要做的准备工作如下，

* 升级 Crash 到支持 Linux 4.6.0 内核的版本

  详细过程请参考 [Linux Crash - coding notes](http://oliveryang.net/2015/07/linux-crash-coding-notes/) 这篇文章。

* 确认 debugfs 工具是否安装

  debugfs 是 ext2, ext3, ext4 文件系统提供的文件系统调试工具，通过它我们可以不通过 mount 文件系统而直接访问文件系统的内容，它是 e2fsprogs 软件包的一部分，如果找不到请安装。
  debugfs 的详细使用说明可以通过 [debugfs man page](http://linux.die.net/man/8/debugfs) 得到。

## 3. Ext4 磁盘格式

本小节将对 Ext4 的磁盘格式做简单介绍。Ext2/Ext3 文件系统的磁盘格式与之相似，但 Ext4 在原有版本的基础上做了必要的扩展。

如下图所示，一个 Ext4 文件系统的磁盘格式是由一个引导块和很多个 block group 组成的,

![ext4 block groups](/media/images/2016/ext4_disk_layout_1.png)

其中每个 block group 又包含如下格式，

![ext4 layout in one group](/media/images/2016/ext4_disk_layout_2.png)

## 4. 实验

#### 3.2.1 Block Group

#### 3.2.2 Super Block

#### 3.2.3 Group Descriptor

#### 3.2.4 Data Block Bitmap

#### 3.2.5 Inode Bitmap

#### 3.2.6 Inode Table

#### 3.2.7 Data Blocks

## 5. 延伸阅读

* [Linux Block Driver - 1](http://oliveryng.net/2016/04/linux-block-driver-basic-1)
* [Linux File System - 1](http://oliveryang.net/2016/01/linux-file-system-basic-1)
* [Linux File System - 2](http://oliveryang.net/2016/01/linux-file-system-basic-2)
* [Linux File System - 3](http://oliveryang.net/2016/02/linux-file-system-basic-3)
* [Linux Crash - background](http://oliveryang.net/2015/06/linux-crash-background)
* [Linux Crash - coding notes](http://oliveryang.net/2015/07/linux-crash-coding-notes/)
* [Ext4 Disk Layout](https://ext4.wiki.kernel.org/index.php/Ext4_Disk_Layout)
* [在Fedora 20环境下安装系统内核源代码](http://www.cnblogs.com/kuliuheng/p/3976780.html)
* [Linux Crash White Paper (了解 crash 命令)](http://people.redhat.com/anderson/crash_whitepaper)