---
title: Mac终端中使用sublime打开文件
date: 2019-12-26 16:06:01
tags:
    - sublime
---

Mac OS 自带终端，怎么使用sublime打开文件？
<!-- more -->

####一、open命令
1.open命令可以在shell内打开文件，可以通过 -a 选项来指定使用哪个应用来打开
```
$ open -a /Applications/Sublime\ Text.app .zsh_history
```
2.将下面的命令写入到 .bash_profile 文件中，并执行 source 命令
```
$ sublime='open -a /Applications/Sublime\ Text.app'
$ sublime .zsh_history
```
3.上面的语句如果直接运行，只会在当前 shell 会话内有效
4.真正的快捷方式来啦~~~
```
$ sudo ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/subl
// 如果之前有设置过但未生效，可以先移除再执行上面的语句！
$ sudo rm /usr/local/bin/subl
// 使用方法：
$ subl .bash_profile
```