---
title: iOS 客户端与服务端做时间同步
date: 2017-04-28 09:37:05
categories: 编程
tags: [iOS, 服务端]
---

![](https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-416679.jpg)

### 需求

我们做客户端的时候，有时会需要对客户端与服务器的时间进行同步，比如抢购活动、倒计时等。这时我们要考虑如何准备地与服务器的时间进行同步，同时防止用户本地的时间有误差时导致的问题。

### 分析

#### 描述

为了实现以上需求，我们需要：

1. 获取服务器某一时刻 `A` 的时间；

2. 记录获取到时刻 `A` 时的本地时间 `B`；

3. 需要用到时间时，获取当前本地时间 `C`，当 `C - B` 作为时间间隔 `D`，则 `A + D` 则是当前服务器的时间。

#### 实现

1. 从上面的步骤，我们可以得到，要消除用户修改时间导致的影响，必须保证 `B` 和 `C` 与系统时间无关；

2. `iOS` 中正好有提供这样两个接口：

 1. 获取设备当前时间 `Now`，该值受系统时间影响，用户如果修改时间，值也会随着变化；
 
 2. 获取设备上次重启的时间 `BootTime`，该值受系统时间影响，用户如果修改时间，值也会随着变化；；

3. 由上面 `iOS` 提供的两个接口，我们可以获取本地时间 `B`、`C`：设备自上次重启后运行的时间（`BootTime - Now`），该值与系统时间无关；

### 代码实现

获取当前 Unix Time：

```
    static func now() -> Int {
        var now =  timeval()
        var tz = timezone()
        gettimeofday(&now, &tz)
        return now.tv_sec
    }
```

获取设备上次重启的 Unix Time：

```
    func boottime() -> Int {
        
        var mid = [CTL_KERN, KERN_BOOTTIME]
        var boottime = timeval()
        var size = MemoryLayout.size(ofValue: boottime)
        
        if sysctl(&mid, 2, &boottime, &size, nil, 0) != -1 {
            return boottime.tv_sec
        }
        return 0
    }
```

时间校准：

```
// 接口获取服务器时间处理
let serverTime = xxx						// 获取到的服务器时间
let runTime0 = now() - boottime()			// 当前设备运行时间

// 需要用到时间时
let runTime1 = now() - boottime()			// 当前时刻设备运行时间
let currentTime = serverTime + runTime1 	// 当前服务器时间
```

### 参考

[iOS关于时间的处理
](http://mrpeak.cn/blog/ios-time/)