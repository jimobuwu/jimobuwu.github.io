---
title: 汉诺塔
date: 2016-12-04 13:43:10
tags:
categories: 算法
---
#### 递归实现
```c
        //从x,借助y，到z
        void move(int n, char x, char y, char z)
        {
            if (1 == n)
            {
                printf("%c --> %c\n", x,z); //把最下面的一个从x到z
            }
            else
            {
                move(n - 1, x, z, y);     //先把上面n-1从x，借助z，到y
                printf("%c --> %c\n", x, z);
                move(n - 1, y, x, z); //再把n-1，从y，借助x， 到z
            }
        }
        
        move(3, 'x', 'y', 'z');
```