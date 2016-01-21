---
layout: post
title: X11把菜单键改成鼠标中键(针对各种弱化中键场景)
categories: [linux]
tags: [X11, xkbset, fvwm, xmodmap]
---

>首先看看我们的鼠标中键功用:粘贴,关闭,拖动画布,滚动条....
能看到这里说明对此键的需求十分的关切，来重点(加入Fvwm启动脚本)。

###解决方案###
    
    大体需要xkbset 和Xmodmap
    你的wm脚本启动。

####step1:install xkbset

`sudo yaourt -S aur/xkbset`

####step2:define keyboard layout

    查看你当前的keyboard layout

`setxkbmap -query`
    
    大概显示如下(确认使用us)
    rules:      evdev
    model:      pc104
    layout:     us

####step3:定义你当前要映射鼠标键

`xkbset m`
`xkbset exp m`     #引用文档里使用的估计版本有差异 `xkbset exp =m`

####step3:使用xev查找你要替换键的keycode(xmodmap需要使用)

    你不会用xev?没事你可以查看引用，也可以到

[wiki](https://wiki.archlinux.org/index.php/extra_keyboard_keys)

    里找到答案.我们也用菜单键(keycode 135)
    

####step4:加入xmodmap 

`xmodmap -e "keycode 135 = Pointer_Button2"`

    or 写到xmodmap配置文件里，说到这里我也要去更新自己的配置文件了

[config](/linux/2014/11/25/Dvorak-fcitx-X11-autostart)
 
    加入之，下次就可以自启动了
    
    keycode 135 = Pointer_Button2


####fvwm 自动启动

fvwm初始化代码

    + I Exec exec xkbset m
    + I Exec exec xkbset exp m

Done

###reference
[askubuntu](http://askubuntu.com/questions/45203/how-can-i-assign-a-middle-button-press-to-a-specific-key-in-my-laptop)    
