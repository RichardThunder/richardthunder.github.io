---
title: vim 目录树
toc: true            # 是否生成目录
indent: true         # 是否首行缩进   
archive: true        # 是否显示在归档
cover: false         # 是否显示封面
mathjax: false       # 是否渲染公式
pin: false           # 是否首页置顶
top_meta: false      # 是否显示顶部信息
bottom_meta: false   # 是否显示尾部信息
sidebar: [toc]
tag:
  - Vim
  - Linux
categories: Linux
keywords: vim
date: {{ date }}
description: 
icons: [fas fa-fire red, fas fa-star green]
---

# Nerdtree的安装

------

1. 进入.vim/bundle目录.
2. 执行git clone [https://github.com/scrooloose/nerdtree](https://link.jianshu.com/?t=https://github.com/scrooloose/nerdtree).
3. 下载完成后，在bundle下会多出一个nerdtree的文件夹，所有相关插件都在该文件夹下.
4. 在终端进入项目文件夹, 输入`vim .`,即可看到树形目录.

快捷键

------

h j k l移动光标定位
ctrl+w+w 光标在左右窗口切换
ctrl+w+r 切换当前窗口左右布局
ctrl+p 模糊搜索文件
gT 切换到前一个tab
g t 切换到后一个tab

> 可以在.vimrc里为标签页进行的配置，通过ctrl h/l切换标签等
> let mapleader = ','
> nnoremap <C-l> gt
> nnoremap <C-h> gT
> nnoremap <leader>t : tabe<CR>

o 打开关闭文件或者目录，如果是文件的话，光标出现在打开的文件中
O 打开结点下的所有目录
X 合拢当前结点的所有目录
x 合拢当前结点的父目录

i和s水平分割或纵向分割窗口打开文件
u 打开上层目录
t 在标签页中打开
T 在后台标签页中打开

p 到上层目录
P 到根目录
K 到同目录第一个节点
J 到同目录最后一个节点
m 显示文件系统菜单（添加、删除、移动操作）
? 帮助
:q 关闭