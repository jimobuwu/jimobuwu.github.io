---
title: 骨骼动画数据预加载
date: 2016-12-03 07:42:49
tags:
categories: Cocos2d-x优化
---
## 为什么要做预加载？

在做xyj战斗系统的过程中，我们发现，每次切换敌人波次的时候，会产生大约2~4秒的延迟（根据不同手机的处理能力不同，时间也不同）。  
Cocos2d-x本身提供的skeletonAnimation构造函数，是以skeletonDataFile和altasFile为参数。这其中的IO操作，会在骨骼动画比较大的情况下，在战斗中加载骨骼动画造成卡顿。
