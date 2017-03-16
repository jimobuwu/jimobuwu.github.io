---
title: minizip
date: 2017-03-12 02:18:28
tags:
categories: 工具库
---

### minizip简介
minizip时zlib的上层库，它封装了与zip文件相关的操作。   
minizip中与解压缩相关的API:  
* `unzFile unzOpen(const char *path);`  
打开压缩文件
<!--more-->
* `int unzGetGlobalInfo(unzFile file, unz_global_info* pglobal_info);`  
获取zip压缩文件的信息（内部文件个数），保存在pglobal_info

* `int unzGoToNextFile(unzFile file)`  
遍历zip中的文件，初始自动定位到第一个文件，使用该函数跳转到下一个文件

* `int unzGetCurrentFileInfo()`  
获取单个内部文件的信息：文件路径，大小

* `int unzOpenCurrentFile(unzFile file)`  
打开该内部文件  

* `int unzReadCurrentFile(unzFile file)`  
读取该内部文件 

* `int unzCloseCurrentFile(unzFile file)`  
关闭内部文件

* `int unzClose(unzFile file);`  
关闭打开的zip文件


### 加载相关的头文件及库文件
在使用zlib以及minizip之前，我们需要加载相关的头文件及库文件到工程中。需要加载的头文件有zlib.h、unzip.h、zip.h。需要加载的库文件有zlib.lib、minizip.lib。需要添加的动态链接库zlib1.dll。这些文件都可以从网上下载得到。

```
     #include "zlib/zlib.h"  
     #include "zlib/unzip.h"  
     #include "zlib/zip.h"  
     #pragma comment(lib, "zlib.lib")  
     #pragma comment(lib, "minizip.lib")  
```

### 示例程序
```c++
  1  /*
  2  * 函数功能 :  解压zip文件
  3  * 备    注 : 参数strFilePath表示zip压缩文件的路径
  4  *           参数strTempPath表示要解压到的文件目录
  6  */
  7 void CZlibDemoDlg::UnzipFile(CString strFilePath, CString strTempPath)
  8 {
  9     int nReturnValue;
 10 
 11     //打开zip文件
 12     unzFile unzfile = unzOpen(strFilePath);                    
 13     if(unzfile == NULL)
 14     {
 15         MessageBox("打开zip文件失败!", "提示", MB_OK|MB_ICONWARNING);
 16         return;
 17     }
 18 
 19     //获取zip文件的信息
 20     unz_global_info* pGlobalInfo = new unz_global_info;
 21     nReturnValue = unzGetGlobalInfo(unzfile, pGlobalInfo);
 22     if(nReturnValue != UNZ_OK)
 23     {
 24         MessageBox("获取zip文件信息失败!", "提示", MB_OK|MB_ICONWARNING);
 25         return;
 26     }
 27 
 28     //解析zip文件
 29     unz_file_info* pFileInfo = new unz_file_info;
 30     char szZipFName[MAX_PATH];                            //存放从zip中解析出来的内部文件名
 31     for(int i=0; i<pGlobalInfo->number_entry; i++)
 32     {
 33         //解析得到zip中的文件信息
 34         nReturnValue = unzGetCurrentFileInfo(unzfile, pFileInfo, szZipFName, MAX_PATH, 
 35             NULL, 0, NULL, 0);
 36         if(nReturnValue != UNZ_OK)
 37         {
 38             MessageBox("解析zip文件信息失败!", "提示", MB_OK|MB_ICONWARNING);
 39             return;
 40         }
 41 
 42         //判断是文件夹还是文件
 43         switch(pFileInfo->external_fa)
 44         {
 45         case FILE_ATTRIBUTE_DIRECTORY:                    //文件夹
 46             {
 47                 CString strDiskPath = strTempPath + _T("//") + szZipFName;
 48                 CreateDirectory(strDiskPath, NULL);
 49             }
 50             break;
 51         default:                                        //文件
 52             {
 53                 //创建文件
 54                 CString strDiskFile = strTempPath + _T("//") + szZipFName;
 55                 HANDLE hFile = CreateFile(strDiskFile, GENERIC_WRITE,
 56                     0, NULL, OPEN_ALWAYS, FILE_FLAG_WRITE_THROUGH, NULL);
 57                 if(hFile == INVALID_HANDLE_VALUE)
 58                 {
 59                     MessageBox("创建文件失败!", "提示", MB_OK|MB_ICONWARNING);
 60                     return;
 61                 }
 62 
 63                 //打开文件
 64                 nReturnValue = unzOpenCurrentFile(unzfile);
 65                 if(nReturnValue != UNZ_OK)
 66                 {
 67                     MessageBox("打开文件失败!", "提示", MB_OK|MB_ICONWARNING);
 68                     CloseHandle(hFile);
 69                     return;
 70                 }
 71 
 72                 //读取文件
 73                 const int BUFFER_SIZE = 4096;
 74                 char szReadBuffer[BUFFER_SIZE];
 75                 while(TRUE)
 76                 {
 77                     memset(szReadBuffer, 0, BUFFER_SIZE);
 78                     int nReadFileSize = unzReadCurrentFile(unzfile, szReadBuffer, BUFFER_SIZE);
 79                     if(nReadFileSize < 0)                //读取文件失败
 80                     {
 81                         MessageBox("读取文件失败!", "提示", MB_OK|MB_ICONWARNING);
 82                         unzCloseCurrentFile(unzfile);
 83                         CloseHandle(hFile);
 84                         return;
 85                     }
 86                     else if(nReadFileSize == 0)            //读取文件完毕
 87                     {
 88                         unzCloseCurrentFile(unzfile);
 89                         CloseHandle(hFile);
 90                         break;
 91                     }
 92                     else                                //写入读取的内容
 93                     {
 94                         DWORD dWrite = 0;
 95                         BOOL bWriteSuccessed = WriteFile(hFile, szReadBuffer, BUFFER_SIZE, &dWrite, NULL);
 96                         if(!bWriteSuccessed)
 97                         {
 98                             MessageBox("读取文件失败!", "提示", MB_OK|MB_ICONWARNING);
 99                             unzCloseCurrentFile(unzfile);
100                             CloseHandle(hFile);
101                             return;
102                         }
103                     }
104                 }
105             }
106             break;
107         }
108         unzGoToNextFile(unzfile);
109     }
110 
111     //关闭
112     if(unzfile)
113     {
114         unzClose(unzfile);
115     }
116 }
```


