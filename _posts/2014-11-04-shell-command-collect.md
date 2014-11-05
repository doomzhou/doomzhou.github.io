---
layout: post
title: Shell command collect
categories: [shell]
tags: [shell, bash, linux]
---
<h2>{{ page.title }}</h2>

Intro
===
Shell command collect

##loop a to z
`for w in {a..z}`  

##awk array
`netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'`

##generate password
`date +%s | sha256sum | base64 | head -c 32 ; echo`

##format number
`echo 18888888888888888888 | rev | sed 's/.../&,/g' | rev `

    18,888,888,888,888,888,888

##get double 双色球
`echo -e '\e[0;31m' $(shuf -i 1-33 | head -n6 | sort -n) '\e[0;34m' $(shuf -i 1-16 | head -n1) '\e[0m'`

##DDos bad boy

    for i in `netstat -an | grep -i ':80 '|grep 'EST' | awk '{print $5}' | cut -d : -f 1 | sort | uniq -c | awk '{if($1 > 50) {print $2}}'`
    echo $i
    echo $i >> /tmp/banip
    /sbin/iptables -A INPUT -p tcp -j DROP -s $i
    done


##bash ssh command 
`alias ssh='strace   -o   /tmp/sshpwd-``date    '+%d%h%m%s'``.log -e read,write,connect  -s2048 ssh'`

##pass function Usage who-to e.g:doom-gmail
`pass () { echo $1-$2 | md5sum | sha1sum | awk -F '[1a2b]' '{for (i=1;i<=NF;i++) if ( length($i)>= 6) print$i}' | sed -n '1p' | sed -n 's/[3c]/\%/gp'}`

##dd for progress
`while killall -USR1 dd; do sleep 5; done`

##Gnu/linux user security
`chage -d 0 -m 0 -M 90 -W 15 root`

##awk half an hour ago 
`awk '{split($1,d,"-");split($2,t,":");epoch = mktime(d[1]" "d[2]" "d[3]" "t[1]" "t[2]" "t[3])}(systime()-epoch)<1800{print} ' log`
