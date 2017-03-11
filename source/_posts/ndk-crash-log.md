---
title: 利用NDK崩溃日志查找BUG
date: 2016-12-05 22:00:41
tags:
categories: Android
---
利用Android的crash log来对C++开发的Android应用进行错误定位。
通用操作系统都有自己的日志系统，Android也不例外。但是Android的系统日志，打印的都是内存的地址和寄存器信息。
如：
        - ::35.331 23889 23889 I DEBUG : *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** *** 
        - ::35.331 23889 23889 I DEBUG : Build fingerprint: google/razorg/deb:4.4.2/KOT49H/937116:user/release-keys 
        - ::35.331 23889 23889 I DEBUG : Revision: 
        - ::35.331 23889 23889 I DEBUG : pid: , tid: , name: Thread- >>> com.guangyou.ddgame <<< 
        - ::35.331 23889 23889 I DEBUG : signal (SIGSEGV), code (SEGV_MAPERR), fault addr 00000028 
        - ::35.431 D audio_hw_primary: out_set_parameters: enter: usecase(: deep-buffer-playback) kvpairs: routing= 
        - ::35.511 23889 23889 I DEBUG : r0 76d94458 r1 00000000 r2 00000000 r3 00000000 
        - ::35.511 23889 23889 I DEBUG : r4 760c1a48 r5 751e2440 r6 00000001 r7 760c1a48 
        - ::35.511 23889 23889 I DEBUG : r8 00000001 r9 76c96f3c sl 76c861c0 fp 76d94444 
        - ::35.511 23889 23889 I DEBUG : ip 00000001 sp 76d94430 lr 75a81bd8 pc 75a81bdc cpsr 600f0010 
        - ::35.511 23889 23889 I DEBUG : d0 746968775f327865 d1 6a6e6169642f675f 
        - ::35.511 23889 23889 I DEBUG : d2 5f6f616978757169 d3 676e702e6e776f6d 
        - ::35.511 23889 23889 I DEBUG : d4 0000000009000000 d5 0000000000000000 
        - ::35.511 23889 23889 I DEBUG : d6 0000000000000000 d7 0000000000000000

## 怎样获取android的系统日志

假设你已经安装了 Android Develop Tools, 可以成功调用 adb . 并打开android开发用机的调试模式, 连接到电脑.
打开命令行, 在命令行输入: adb logcat . 就可以看到满屏幕的日志啦.
输入 adb logcat --help 可以看到 logcat 的用法提示.
两个常用参数：
        1. -v XXXX : 用来选择log输出样式, 一般建议 threadtime , 更加详细.
        2. -d : 让log一次性输出后马上完毕. 如果没有此命令, logcat 工具会一直输出, 即使更新在界面上.

如果需要保存log到文件, 方便以后查看. 可输入命令:  
        **adb logcat -v threadtime -d > log.txt**

输出到文件中的是一段时间内的log(大概从凌晨**3点**开始,一直到敲完cmd命令时)   

adb工具在**\android-sdk-windows\platform-tools**下

未完待续