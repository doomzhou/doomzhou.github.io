---
layout: post
title: FVWM下实现KDE的runner(带autocomplete效果)
categories: [linux]
tags: [FVWM, Python]
---
<h2>{{ page.title }}</h2>


最早的时候是使用KDE的, ALT-<F2>的runner功能非常好用,上个图怀念一下

![](https://67.media.tumblr.com/b32b9e64a3616421fc969add01e24180/tumblr_o7pl80Gfpq1r68ev5o2_500.png)

自从切换到了FVWM后一直没有好用的runner, 每次都通过terimate 然后run  
感觉没有geek的感觉,所以决定自己实现一把通过python 的tkinter,


代码非常简单如下

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    # File Name : runner.py
    '''Purpose : 模拟kde runner ALT+F2 
    requirement:
        tkinter
        tk
    '''
    # Creation Date : 1462173892
    # Last Modified :
    # Release By : Doom.zhou
    ###############################################################################


    import os
    from tkinter import  Text, Button, Tk, INSERT

    root = Tk()

    def call():
        tv = text.get("1.0", 'end-1c')
        os.system("%s &" % tv)
        root.quit()

    text = Text(root , height = 2, width = 30)
    text.focus_force()


    button = Button(root, height=2,command=call, width=20,text="Submit")
    text.bind('<Return>', lambda e: call())
    root.bind('<Escape>', lambda e: root.quit())

    text.pack()
    button.pack()

    root.mainloop()


    if __name__ == '__main__':
        pass

最实用的autocomplete功能当时没有实现,使用一段时间后增加了此功能代码如下

[参考了](http://code.activestate.com/recipes/578253-an-entry-with-autocompletion-for-the-tkinter-gui/)


绑定Esc,Return,Right几个键了最终效果图如下

![](https://67.media.tumblr.com/8d475445d739c18347e0f5d81bd4444a/tumblr_o7pl80Gfpq1r68ev5o1_250.png)


代码如下

    #!/usr/bin/env python3
    # -*- coding: utf-8 -*-
    # File Name : runner.py
    '''Purpose : 模拟kde runner ALT+F2 
    requirement:
        tkinter
        tk
    '''
    # Creation Date : 1462173892
    # Last Modified :
    # Release By : Doom.zhou
    ###############################################################################


    import os
    #from tkinter import  Text, Button, Tk, INSERT
    from tkinter import *


    class AutocompleteEntry(Entry):
        def __init__(self, lista, *args, **kwargs):
            
            Entry.__init__(self, *args, **kwargs)
            self.lista = lista        
            self.var = self["textvariable"]
            if self.var == '':
                self.var = self["textvariable"] = StringVar()

            self.var.trace('w', self.changed)
            self.bind("<Right>", self.selection)
            self.bind("<Up>", self.up)
            self.bind("<Down>", self.down)
            
            self.lb_up = False

        def changed(self, name, index, mode):  
            
            print('changed')

            if self.var.get() == '':
                self.lb.destroy()
                self.lb_up = False
            else:
                words = self.comparison()
                if words:            
                    if not self.lb_up:
                        self.lb = Listbox()
                        self.lb.bind("<Double-Button-1>", self.selection)
                        self.lb.bind("<Right>", self.selection)
                        self.lb.place(x=self.winfo_x(), y=self.winfo_y()+self.winfo_height())
                        self.lb_up = True
                    
                    self.lb.delete(0, END)
                    for w in words:
                        self.lb.insert(END,w)
                else:
                    if self.lb_up:
                        self.lb.destroy()
                        self.lb_up = False
            
        def selection(self, event):

            print('selection')
            if self.lb_up:
                self.var.set(self.lb.get(ACTIVE))
                self.lb.destroy()
                self.lb_up = False
                self.icursor(END)

        def up(self, event):

            print('up')
            if self.lb_up:
                if self.lb.curselection() == ():
                    index = '0'
                else:
                    index = self.lb.curselection()[0]
                if index != '0':                
                    self.lb.selection_clear(first=index)
                    index = str(int(index)-1)                
                    self.lb.selection_set(first=index)
                    self.lb.activate(index) 

        def down(self, event):

            print('down')
            if self.lb_up:
                if self.lb.curselection() == ():
                    index = '0'
                else:
                    index = self.lb.curselection()[0]
                if index != END:                        
                    self.lb.selection_clear(first=index)
                    index = str(int(index)+1)        
                    self.lb.selection_set(first=index)
                    self.lb.activate(index) 

        def comparison(self):

            print('comparison')
            pattern = re.compile('.*' + self.var.get() + '.*')
            return [w for w in self.lista if re.match(pattern, w)]




    if __name__ == '__main__':
        lista = ['feh', 'google-chrome-stable', 'google-chrome-unstable', 'gimp', 'firefox',
                'burpsuite', 'wireshark', 'jmeter', 'freemind', 'XMind', 'okular', 'termite']
        def call():
            tv = entry.get()
            os.system("%s &" % tv)
            root.quit()
        root = Tk()

        entry = AutocompleteEntry(lista, root)
        entry.grid(row=0, column=0)
        Button(root, height=2,command=call, width=20,text="Submit").grid(row=3, column=0)

        entry.focus_force()
        entry.bind('<Return>', lambda e: call())
        root.bind('<Escape>', lambda e: root.quit())
        

        root.mainloop()

需要的程序添加到其中的lista中，可以通过`compgen -ac`来取到命令列表
compgen 使用参考[URL](http://stackoverflow.com/questions/948008/linux-command-to-list-all-available-commands-and-aliases?answertab=active#tab-top)
