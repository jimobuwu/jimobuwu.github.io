---
title: c文件操作
date: 2017-03-19 16:25:11
tags:
categories: C
---

```c++
FILE *fp = fopen(fullpath.c_str(), mode)
fseek(fp, 0, SEEK_END);         //指向文件尾部
size = ftell(fp);               //fp位置到文件首部的偏移字节数
fseek(fp, 0 ,SEEK_SET);         //指向文件头部

char input[30];
fread(input, size, sizeof(char), fp); //从fp读取input

fclose(fp);

int rename(char* oldname, char* newname); //文件重命名
```

