---
layout: post
title: Linux制作只有ssh的chroot
categories: [Linux]
tags: [chroot, ssh, linux]
---
<h2>{{ page.title }}</h2>

Intro
===

>安全需求需要制作一个只有SSH命令的chroot, 最初需求是要制作一个只能执行SSH命令的前置机。

###step1 配置ssh和 pam

编辑/etc/pam.d/system-auth-ac 加入下面两行

{% highlight python %}
session     required      pam_chroot.so debug
session     required      pam_mkhomedir.so skel=/etc/skel/ umask=0022
{% endhighlight python %}

###step2 配置chroot.conf

编辑/etc/security/chroot.conf ，语法见原文档

{% highlight python %}
# /etc/security/chroot.conf
# format:
# username_regexchroot_dir
#matthew/home
{% endhighlight python %}

比如我要所有mysql用户进入 /home/mysql则加入

{% highlight python %}
mysql   /home/mysql
{% endhighlight python %}


###step3 让ssh 调chroot的

修改/etc/pam.d/sshd 最后加入
{% highlight python %}
session   required      pam_chroot.so
{% endhighlight python %}

###修改脚本以适应自己环境

{% highlight bash %}
#!/bin/bash
#
# Author: Pravin Rane
#
# This script creates chroot env. Change CHROOT variable as per your requirement
# Tested on RHEL5, CentOS5, Fedora5

CHROOT="/home/jailops"
echo "chroot is $CHROOT"
echo "Creating directory sturcture"
mkdir $CHROOT
cd $CHROOT
mkdir home
mkdir etc
mkdir etc/security
mkdir bin
mkdir lib
mkdir usr
mkdir usr/bin
mkdir usr/share
mkdir usr/share/locale
mkdir var
mkdir var/log
mkdir proc
mkdir dev
mkdir dev/pts
mkdir -p usr/lib/locale/
mknod dev/null c 1 3
mknod dev/zero c 1 5
mknod dev/random c 1 8
mknod -m 0444 dev/urandom c 1 9
mknod dev/tty c 5 0
chown root.tty dev/tty
chmod 666 dev/tty
mknod dev/ptmx c 5 2

# Copy basic files
echo "Copying config files"
cp -pr /etc/skel /etc/environment /etc/passwd /etc/group /etc/localtime $CHROOT/etc/
cp -p /etc/security/console.handlers /etc/security/pam_env.conf $CHROOT/etc/security/
cp -p /var/log/lastlog $CHROOT/var/log/
cp -pr /usr/share/locale/en /usr/share/locale/en_US /usr/share/locale/locale.alias $CHROOT/usr/share/locale
cp -pr /usr/share/locale/zh_CN /usr/share/locale/zh /usr/share/locale/zh_CN.GB2312 $CHROOT/usr/share/locale
cp -pr /usr/share/i18n $CHROOT/usr/share
cp -pr /usr/lib/locale/locale-archive $CHROOT/usr/lib/locale


#COMMANDS="/bin/bash /usr/bin/mysql /usr/bin/ssh"
COMMANDS="/bin/bash /bin/ls /bin/mkdir /bin/mv /bin/pwd /bin/rm /usr/bin/id /usr/bin/ssh /bin/ping /usr/bin/mysql"
for prog in $COMMANDS;  do
        cp $prog ./$prog

        # obtain a list of related libraries
        ldd $prog > /dev/null
        if [ "$?" = 0 ] ; then
                LIBS=`ldd $prog | awk '{ print $3 }'|grep -v 0x`
                for l in $LIBS; do
                        mkdir -p ./`dirname $l` > /dev/null 2>&1
                        cp -p $l ./$l
                done
        fi
done

# For ssh You don't exist, go away
cp -pr /lib64/libnss_* $CHROOT/lib64/


if [ $? -eq 0 ]; then
        echo ".."
        echo "Chroot is successfully created at $CHROOT"
        echo "1. Mount proc and devpts now using following commands"
        echo "mount proc $CHROOT/proc -t proc"
        echo "mount devpts $CHROOT/dev/pts -t devpts -o gid=5,mode=620"
        echo ""
        echo "2. Do the changes in syslogd as mentioned in script and restart it."
        echo "Your syslogd's extra socket should be at $CHROOT/dev/log"
        echo ""
        echo "As a root run command \"chroot $CHROOT\" to test your setup"
fi
{% endhighlight bash %}


###挂载 相关proc dev

{% highlight python %}
mount proc /home/chroot/proc -t proc
mount devpts /home/chroot/dev/pts -t devpts -o gid=5,mode=620""
{% endhighlight python %}

###Troubleshooting

####Q1 

    Unkown Terminal type when doing SSH

`cp /usr/share/terminfo to /home/chroot/usr/share`


####Q2 You don't exist, go away!

    You don't exist, go away!


copy 相关的 (/etcp/passwd, /etc/group, /etc/shadow,/etc/gshadow)
还有/lib/libnss_* libraries /etc/hosts, /etc/nsswitch.conf, /etc/resolv.conf


####Q2 ssh  chroot error session_pty_req session 0 alloc failed  

    ssh  chroot error session_pty_req session 0 alloc failed  


如果你在Centos 的话记得关掉Selinux

###好了就这样，有问题找我


###更新加入中文支持

{% highlight bash %}}
mkdir -p usr/lib/locale/
cp -pr /usr/lib/locale/locale-archive $CHROOT/usr/lib/locale
{% endhighlight bash %}}

###Reference: 
[url1](http://www.systemonix.com/2012/06/chroot-linux.html+&cd=4&hl=en&ct=clnk&gl=us)

[url2](http://lccnetvip.pixnet.net/blog/post/32937893--%E7%8E%A9%E7%8E%A9-ssh-%2B-chroot-%E7%B0%A1%E6%98%93%E7%92%B0%E5%A2%83%E6%9E%B6%E8%A8%AD)

[url3](http://allanfeid.com/content/creating-chroot-jail-ssh-access)
