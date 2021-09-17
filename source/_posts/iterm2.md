---
title: iterm2
date: 2021-06-09 19:31:40
tags: mac
categories: daily
---
关于自己Mac电脑的iterm2终端美化配置方案
<!--more-->

![2](iterm/2.png)

# oh-my-zsh 安装
一个美化要用的东西，iterm2就直接App store下载就行了

先切换命令 
```
        chsh -s /bin/zsh
```

再安装
```
        sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

安装失败就先git clone
```
        git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
```

再替换
```
        cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```
这样就好啦

# 插件
命令高量 代码补全 和一个jump 文件夹
前两个不咋用 最后一个好用 
先安装brew 然后

`brew install autojump`

然后编辑 vim ~/.zshrc 文件, 找到 plugins 配置, 增加 autojump 插件.

# 窗口外观
就是下拉式嘛 看下面这些就好咯
[链接](https://www.bilibili.com/read/cv8638110/)
配色 rgb(133,73,71)
字体 Andale Moto

