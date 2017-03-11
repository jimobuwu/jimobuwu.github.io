---
title: 热更新
date: 2016-12-03 08:52:10
tags:
categories: Cocos2d-x
---

### 关键类
- AssetsManagerEx，热更新核心类
- Manifest，更新清单类
- EventAssetsManagerEx，热更新事件类
- EventListenerAssetsManagerEx，事件监听类
<!--more-->
### 使用方法
- C++   
        auto dispatcher = Director::getInstance()->getEventDispatcher();
        //manifestUrl, 本地资源列表文件
        //storagePath, 新版本资源存储路径 . 因为旧的资源是只读的, 所以需要放到新的位置
        auto manager = AssetsManagerEx::create(manifestUrl, storagePath);
        
        auto callback = [](EventAssetsManagerEx* event){ do_some_thing(); };
        auto listener = EventListenerAssetsManagerEx::create(manager, callback);
        dispatcher->addEventListenerWithSceneGraphPriority(listener, one_node);

        dispatcher->removeEventListener(listener);

        //检测是否有更新
        void checkUpdate();


- Lua   
        self.assetsManagerEx = cc.AssetsManagerEx:create(MY_CONFIG.manifestUrl, storagePath)
        self.assetsManagerEx:retain()

        --设置下载消息listener
        local eventListenerAssetsManagerEx = cc.EventListenerAssetsManagerEx:create(self.assetsManagerEx, handler(self, self.handleAssetsManagerEx))
        cc.Director:getInstance():getEventDispatcher():addEventListenerWithFixedPriority(eventListenerAssetsManagerEx, 1

        --检查版本并升级
        self.assetsManagerEx:checkUpdate()
        --更新函数
        self.assetsManagerEx:update()
        
#### EventAssetsManagerEx事件类型,在callback中监听的事件类型
enum class EventCode
  {  
  ERROR_NO_LOCAL_MANIFEST, //**本地没有manifest文件**  
  ERROR_DOWNLOAD_MANIFEST,//**下载manifest文件失败**  
  ERROR_PARSE_MANIFEST,//**解析manifest文件失败**  
  NEW_VERSION_FOUND,//**发现新版本**  
  ALREADY_UP_TO_DATE,//**已经更新到最新**  
  UPDATE_PROGRESSION,//**更新进度**  
  ASSET_UPDATED,     //**更新成功一个文件**  
  ERROR_UPDATING,//**更新中错误**
  UPDATE_FINISHED,     //**全部更新完成**  
  UPDATE_FAILED,//**更新失败**  
  ERROR_DECOMPRESS //**解压失败**  
  };   
    
### 遇到的问题
- **本地资源列表的存放位置**
只能是放在src中,不能放在可读写区域,因为无法确认具体设备的可读写区域在哪,     
所以project.manifest和version.manifest，也要作为热更新的内容写在project.manifest中

- **热更新后,没有使用更新后的文件**  
因为src目录已经很早之前就添加进去了,这个时候需要把更新目录加到最前面去  
        cc.FileUtils:getInstance():addSearchPath("versionFiles/", true)  
第二个参数设置为ture，可以将路径加到第一个查找的位置  
- **更新到509个文件时停止**
因为Cocos在热更新时，会同时打开所有待更新的文件，创建临时文件tmp，如果更新文件数量过多，会同时创建过多的文件句柄，  
超过了系统进程打开文件最大句柄数的限制后，就不能再继续打开新的文件。
**为什么是509呢？** 
系统进程打开文件最大句柄数的限制是512个，其中三个是标准输入文件__stdin__,标准输出文件__stdout__,标准错误输出文件__stderr__
设置  
获取和设置当前系统允许打开的最大句柄数的函数：__getmaxstdio()__和__setmaxstdio()__
