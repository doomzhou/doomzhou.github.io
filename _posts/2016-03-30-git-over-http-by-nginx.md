---
layout: post
title: Windows7下修改无线MAC地址 [后文有下载工具]
categories: [Git/linux]
tags: [Ops, Git]
---
<h2>{{ page.title }}</h2>

Intro
===
>需要一下简易的内部Git服务供给给发布系统，gitlab的话太重了
所以才有了以下方案

## 选型

0. centos 6
1. git http-backend
2. fcgiwrap 
3. nginx
4. 需注意使用bare repo交互
5. salt(可选，用于发布系统)

## git http-backend

假如直接yum,的话一般在/usr/lib/git-core/,或者/usr/libexec/git-core下面

## fcgiwrap

### install

get source:  

    cd /usr/local/src/
    git clone git://github.com/gnosek/fcgiwrap.git

compile:

    cd /usr/local/src/fcgiwrap
    autoreconf -i
    ./configure
    make
    mv fcgiwrap /usr/local/bin/


### Init script

* /etc/init.d/fcgiwrap

        #!/usr/bin/perl
        use strict;
        use warnings FATAL => qw( all );

        use IO::Socket::UNIX;

        my $bin_path = '/usr/local/bin/fcgiwrap';
        my $socket_path = $ARGV[0] || '/tmp/cgi.sock';
        my $num_children = $ARGV[1] || 1;

        close STDIN;

        unlink $socket_path;
        my $socket = IO::Socket::UNIX->new(
            Local => $socket_path,
            Listen => 100,
        );

        die "Cannot create socket at $socket_path: $!\n" unless $socket;

        for (1 .. $num_children) {
            my $pid = fork;
            die "Cannot fork: $!" unless defined $pid;
            next if $pid;

            exec $bin_path;
            die "Failed to exec $bin_path: $!\n";
        }

    Don’t forget chmod +x /etc/init.d/fcgiwrap

* /etc/rc.local

sudo -u www /etc/init.d/fcgiwrap to /etc/rc.local , **www** is your nginx user

### usage

    fastcgi_pass unix:/tmp/cgi.sock

## Config nginx


