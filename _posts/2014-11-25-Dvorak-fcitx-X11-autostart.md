---
layout: post
title: X11自动启动dvorak 同时解决和fctix冲突问题
categories: [linux]
tags: [X11, dvorak, xmodmap]
---

>一入Dvorak深似海，从此qwert是路人,每次dvo(alias xmodmap ~/scripts/dvorak.pke) 
都比较麻烦，就好比每次`startx` 都要开fcitx &一样麻烦,所以这一次我把xmodmap ~/scripts/dvorak.pke 放进了 。但这一次fcitx和xmodmap
开始搞基了。fcitx会覆盖xmodmap设置。

###解决方案###
    
    把自己的Dvorak keymap cp到.Xmodmap里面。fcitx就可以自动加载了。
    但是要注意的是文件名是：.Xmodmap。

###我的Dvorak xmodmap###

    !
    ! ANSI Dvorak keyboard
    !
    ! Author: Jeff Bigler (jcb_AT_mit.edu)
    ! Last modified: 1996/02/06 14:30:59 by jcb_AT_mit.edu
    ! 
    ! This file should be given as an argument to xmodmap to
    ! switch from a qwerty to a Dvorak keyboard.
    !
    ! Note that Null translations have been included as
    ! comments.
    ! 
    ! If you want to be easily able to switch the keyboard
    ! back to its previous state, first run xmodmap -pke and
    ! redirect the output to a file.  Then you can switch back
    ! by giving that file as an argument to xmodmap.  For
    ! example, if you type:
    !
    !     xmodmap -pke > /tmp/keyboard.default
    !
    ! before switching the keyboard, you can then type:
    !
    !     xmodmap /tmp/keyboard.default
    !
    ! to reset the keyboard to its previous state.
    !
    ! Note that if you pass this file again as an argument to
    ! xmodmap, you will apply these translations a second time
    ! and come out with gibberish!
    !
    ! keysym 1 = 1 exclam
    ! keysym 2 = 2 at
    ! keysym 3 = 3 numbersign
    ! keysym 4 = 4 dollar
    ! keysym 5 = 5 percent
    ! keysym 6 = 6 asciicircum
    ! keysym 7 = 7 ampersand
    ! keysym 8 = 8 asterisk
    ! keysym 9 = 9 parenleft
    ! keysym 0 = 0 parenright
    keysym minus = bracketleft braceleft
    keysym equal = bracketright braceright
    keysym q = quoteright quotedbl
    keysym w = comma less
    keysym e = period greater
    keysym r = p P
    keysym t = y Y
    keysym y = f F
    keysym u = g G
    keysym i = c C
    keysym o = r R
    keysym p = l L
    keysym bracketleft = slash question
    keysym bracketright = equal plus
    ! keysym a = a A
    keysym s = o O
    keysym d = e E
    keysym f = u U
    keysym g = i I
    keysym h = d D
    keysym j = h H
    keysym k = t T
    keysym l = n N
    keysym semicolon = s S
    keysym quoteright = minus underscore
    keysym z = semicolon colon
    keysym x = q Q
    keysym c = j J
    keysym v = k K
    keysym b = x X
    keysym n = b B
    ! keysym m = m M
    keysym comma = w W
    keysym period = v V
    keysym slash = z Z
    keycode 66 = Control_L
    clear Lock
    add Control = Control_L
    keycode 135 = Pointer_Button2

###我的.xinitrc###
    
    export GTK_IM_MODULE=xim
    export QT_IM_MODULE=xim
    export XMODIFIERS="@im=fcitx"
    xscreensaver -no-splash &
    fcitx &
    exec fvwm2
    xmodmap ~/scripts/dvorak.pke
    xset -b

