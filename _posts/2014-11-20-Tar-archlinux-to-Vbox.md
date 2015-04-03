---
layout: post
title: 把Archlinux转移到虚拟机(VBox)上.
categories: [Gnu/linux]
tags: [linux, tar]
---

##需求起因

    公司需要所有办公PC入域，所有linux系统就是Vbox的形式滚进标装机器
    虽然有提供导数据的缓冲时间，但是重新安装Arch，真是有点慢,选用
    dd + tar的方式。 


##操作步骤

###实体机上执行tar,dd的备份操作

1. `[root@doomarch ~]tar cvpzf /data/backup.tgz / --exclude=/proc --exclude=/lost+found --exclude=/data/backup.tgz --exclude=/mnt --exclude=/sys`

        命令解释: cvpzf c创建v有细节z压缩p保留权限的文档
                    / 所有文件
                    --exclude  排除掉一些目录包括/proc /lost+found  /mnt /sys /media 和一些自己外挂的目录(为了可行和备份速度)

2. `dd if=/dev/sda of=backup.mbr bs=466 count=1`

        命令解释: 备份磁盘开始的466个字节大小的MBR信息到指定文件,因为我们分区表信息已经改变
                  不用备份.

###虚拟机Vbox上操作。

1. <span style="background-color: #f9f2f4; color: #c7254e">创建一个Archlinux的虚拟机,需要增加两个硬盘。</span>

        磁盘大小依据你现有资料的大小做一定的 收减,因为Vbox比较灵活可以自由增删硬盘,扩容方便
        两个磁盘的理由是，有一个小盘是用来当做U盘把上面的backup.tgz 和 backup.mbr 通过scp的方式拷贝到liveCD中
        方便还原。
2. <span style="background-color: #f9f2f4; color: #c7254e">用liveCD引导进入新虚拟机</span>

        选择很多，可以直接archlinux的iso 或者puppy之类的

3. <span style="background-color: #f9f2f4; color: #c7254e">挂载当U盘的磁盘,直接通过SCP 把备份取到这个挂载上</span>

4. <span style="background-color: #f9f2f4; color: #c7254e">为目标磁盘分区开挂载，了解原系统的目录结构</span>

        比如是否单独/boot /usr 数据盘可以先不创建 比如/data 

5. `tar xvfpz backup.tgz -C /mnt`
    
        假设目标磁盘挂载在 /mnt下面

6. `dd if=backup.mbr of=/dev/sdb bs=466 count=1`
        
        对应的是466哦，因为分区表已经改变

7. <span style="background-color: #f9f2f4; color: #c7254e">基本完成，一些小的调整,比如 sys proc ...目录创建。/etc/fstab修改</span>

8. <span style="background-color: #f9f2f4; color: #c7254e">目标磁盘引导，完成</span>

###Virtualbox+archlinux上操作

1. <span style="background-color: #f9f2f4; color: #c7254e">分配网络</span>

2. `pacman -S virtualbox-guest-utils`

        除此之外，不做其它操作，不要用自带的iso来安装Vbox增强功能

PS:话说真心没觉得VBox 会比Vmware差，开源是种状态。
