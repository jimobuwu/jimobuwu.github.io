---
title: curl
date: 2017-03-19 13:33:54
tags:
categories: 工具库
---

[curl](https://curl.haxx.se)是一个文件传输工具，利用url语法在命令行下工作，通过http，ftp等方式下载。  
库：libcurl  
libcurl提供了两种模式，两套接口。
Easy接口：  
    同步，高效，容易使用
Multi接口：  
    异步，在单线程或者多线程中提供并发传输，

<!--more-->
### 接口函数
* `curl_easy_init`，初始化并获取一个handler
* `curl_easy_setopt(m_curl, 配置项宏，配置值)`,设置参数  
* `curl_easy_perform`,开始传输数据，因为是同步，线程在此阻塞
* `curl_easy_getinfo`,在传输回调过程中或传输结束后，获取相关信息
* `curl_easy_cleanup`,清除handle

curl_easy_setopt中的参数设置：  
* `CURLOPT_URL`  
设置url
* `CURLOPT_CONNECTTIMEOUT`  
连接超时时间
* `CURLOPT_HTTP_CONTENT_DECODING`  
打开解码。lua中请求时，参数headers理由设置“Accept-Encoding：gzip”  
*支持三种编码：*  
identity，不压缩  
deflate，zlib压缩  
gzip，gzip压缩  
* `CURLOPT_WRITEFUNCTION`  
相应数据回调函数
* `CURLOPT_WRITEDATA`  
设置传入上面的回调函数的参数 
* `CURLOPT_HEADERFUNCTION`   
头数据写入回调,获取头部信息
* `CURLOPT_WRITEHEADER`  
传入上面的函数的参数
* `CURLOPT_NOPROGRESS`  
打开进度
* `CURLOPT_PROGRESSFUNCTION`  
进度回调函数
* `CURLOPT_PROGRESSDATA`  
传入进度回调函数的参数
* `CURLOPT_FOLLOWLOCATION`  
允许3XX重定向  
301，永久移动，请求的网页永久移动到新的位置
302，临时移动













