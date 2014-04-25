# linux系统学习记录

## linux学习资源

- [Keruspe's blag](http://www.imagination-land.org/tags/knowingyoursystem.html) 详细介绍linux系统组成及原理
- [LFS-Linux from scratch](http://www.linuxfromscratch.org/) 从源代码开始建立linux系统。这里主要看LFS和CLFS部分。

## 什么是multilib system

若系统中的cpu支持不止一种架构的程序运行，此时该系统称为multilib system。
比如现在的intel i系列处理器，同时支持32位和64位计算，因此装有该cpu的系统称为multilib system。

## 系统启动

### 原始的UNIX System V init (SysVInit)

0 关机
1 single user模式。该模式下系统的基本组件已启动
2 multi user模式。所有不需要联网的处理在这里已完成启动
3 multi user with network模式，所有需要网络的处理在这里已完成
4 不使用，用户可以在这里制定自定义的母的
5 通常情况下启动用户界面，但很多发行版linux在runlevel3时已经启动用户界面了
6 重启系统

### 新一代init system

下面是4个替代SysVInit的 init system。

- OpenRC
- Runit
- Upstart

Ubuntu使用Upstart

- systemd

Red Hat为Fedora开发了systemd，吸纳了Upstart、Upstart的TODO列表以及OS X的init system：launchd

## LFS (Linux from scratch)建设记录

### 标准

#### Linux Standard Base(LSB)

定义了5个独立标准：Core,C++,Desktop,Runtime Languages,Printing.

### 基础Packages

- Autoconf

包括可以根据开发者编辑的模版文件，自动产生配置源代码的shell脚本的程序。

- Automake

包括可以从模版文件生成makefile的程序。

- Bash

**LSB Core** 

外壳程序(Shell interface)。

- Bc

提供任意精度数值处理语言。编译Linux内核时用到。

- Binutils
 
包括linker、assembler和处理object文件的工具。编译LFS时需要本包中的程序。

- Bison

包括GNU版本的yacc(Yet Another Compiler Compiler)。一些LFS程序编译时会用到。

- Bzip2

包括压缩和解压程序。很多LFS包在解压时会用到它。

- Check

包括对其他程序的测试工具。只安装在临时工具链中。

- Coreutils

包括查看和操作文件及文件夹的基本程序。命令行下的文件操作需要这些工具，同时所有LFS的包的安装过程都需要它。

- DejaGNU

包括对其他程序的测试程序。只安装在临时工具链中。

- Diffutils

比较文件和文件夹区别的工具。可用来创建补丁，在很多包的build过程中也使用到它。

- E2fsprogs

包括处理ext2，ext3和ext4文件系统的工具。

- Expect

为其他需要交互的程序提供脚本化的对话框。只安装在临时工具链中。

- File

用来判断文件类型的工具。一些包需要它来build。

- Findutils

查找文件和文件夹的工具。很多包的build脚本用到它。

- Flex

用来生成可以识别文字中特征的程序。是GNU版本的lex。一些LFS包的build过程需要它。

- Gawk

操作文本文件的工具。时GNU版本的awk。很多包的build脚本用到它。

- Gcc

GNU Compiler Collection.包括C和C++编译器。

- GDBM

GNU Database Manager library。只有Man-DB使用它。

- Gettext

包括很多包的国际化和本地化的实用工具和库文件。

- Glibc

包括main C library。Linux程序离了他没法跑。

- GMP

数学库，提供一些函数可进行任意精度数值计算。Gcc的build过程需要它。

- Grep

在文件中进行查找。很多包的build脚本用它。

- Groff

处理和格式化文本。格式化man pages是他的重要功能。

- GRUB

Grand　Unified Boot Loader。最具弹性的boot loader。

- Gzip

压缩和解压工具。

- Iana-etc

为网络服务和网络协议提供数据。

- Inetutils

基础网络管理工具

- IProute2

包括基础和高级IPv4和IPv6网络程序。

- Kbd

键盘实用工具。包括几个控制台字体。

- Kmod

管理Linux Kernel模块的工具。

- Less

好看的文本查看工具，可以上下滚动。Man-DB的查看也用它。

- Libpipline

包括一个库，可弹性和方便的操作子进行间的管道。Man-DB需要它。

- Libtool

包括GNU通用库支持脚本。将复杂的共享库封装到一个持久的便携的接口中。LFS中其他测试工具需要它。

- Linux Kernel

这是真正的操作系统。it is the Linux in the GNU/Linux environment。

- M4

包括一个通用的文本宏处理工具，在build其他程序时用到。

- Make

指导build其他包的工具。几乎所有包的build都用到它。

- Man-DB

查找和查看man pages的工具。用它而不是man package因为它在国际化兼容方面更优。

- Man-pages

包括基础Linux man pages。

- MPC

包括复杂数值计算算法。Gcc用到它。

- MPFR

多精度算法。Gcc用到它。

- Ncurses

包括与终端无关的字符屏幕处理库。经常被用来在菜单系统中提供光标控制。一些LFS包用到它。

- Patch

包括一个程序，可以通过应用补丁文件来修改或创建文件。补丁文件时通过diff程序生成的。很多LFS的build过程需要它。

- Perl

包括运行时语言PERL的解释器。一些LFS包的安装和测试需要它。

- Pkg-config

获取安装了的库和包的元数据的程序。

- Procps-NG

监视进程的程序。

- Psmisc

显示当前正在运行的进程信息。

- Readline

提供一组库，提供命令行编辑和历史查看。Bash用到它。

- Sed

允许不用文本编辑器打开文件的情况下编辑一个文件。大部分LFS包的配置脚本用到它。

- Shadow

包括一个以安全方法处理密码的程序。

- Sysklogd

包括用来记录系统信息的程序，比如kernel或服务程序在有非常规事件发生时输出的信息。

- Sysvinit

提供了init程序。init程序是其他所有进程的父进程。

- Tar

压缩和解压程序。

- Tcl

包括Tool Command Language，被很多测试组件用到。只安装在临时工具链中。

- Texinfo

用来读写和转换info page的工具。很多LFS包的安装过程需要它。

- Udev

动态创建设备节点的程序。如果不用这个程序，则可以预先在/dev中创建上千个静态设备。

- Util-linux

包含很多实用工具。包括处理文件系统的工具、控制台、分区工具和消息工具。

- Vim

一个文本编辑器。

- XZ Utils

压缩解压程序。提供了最高压缩率，用来解压XZ和LZMA格式文件。

- Zlib

一些程序用到的压缩解压工具。


