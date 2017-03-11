---
title: JJ笔试题
date: 2016-12-07 20:55:03
tags:
categories: 笔试题
---
### 基础类型，以及大小
- char(unsigned char)       1
- short int (unsigned short int)                2                
- int(unsigned int)　4
- long int   4
- long long  8
- float   4
- double  8
- bool 1

### 内存对齐
```c++
        typedef struct ss
        {
            char a;
            char b;
            long int c;
        }ss;

        //sizeof(ss) = (1+1+补齐2) + 4 = 8
```

### 进程和线程的联系和区别
#### 联系
- 都是实现多任务并发的手段
- 子进程（线程）的调度一般与父进程（线程）平等竞争

#### 区别
- 进程是资源分配的基本单位，线程是调度的基本单位
- 进程间的通信方式：共享内存，消息队列，信号量，有名管道，无名管道，信号，文件，socket
线程间通信方式，上述进程间的通信方式都可以沿用，另外还有自己的独特的几种：互斥量，自旋锁，条件变量，读写锁，线程信号，全局变量
- 速度上的区别：  
进程间通信可能会需要切换内核上下文，可能会与外设访问，速度会比较慢。  
而线程都是在进程空间内通信，速度会比较快。

### memmove
```c++
        void* memmove(void* dst, const void* src, size_t n)
        {
            char* tmp_dst;
            const char* tmp_src;

            if (dst <= src)
            {
                tmp_dst = (char*)dst;
                tmp_src = (const char*)src;
                while (n--)
                    *tmp_dst++ = *tmp_src++;
            }
            else //dst > src,要从末尾开始复制
            {
                tmp_dst = (char*)dst + n;
                tmp_src = (const char*)src + n;

                while (n--)
                    *--tmp_dst = *--tmp_src;
            }

            return tmp_dst;
        }
```

### new/delete和malloc/free联系和区别
#### 不同点
- malloc/free是标准库函数，new/delete是运算符
  new/delete可以在对象创建的同时自动执行构造函数，在对象消亡时自动执行析构函数。

#### 相同点

