---
title: Carthage 安装与使用
date: 2017-04-28 12:21:30
categories: 工具
tags: [iOS, Carthage]
---

#### 安装

在终端下运行：
```
brew install carthage
```

#### 配置第三方类库

1. 到目标工程目录下创建 *Carthage* 文件：
![目标工程目录](http://upload-images.jianshu.io/upload_images/808722-c578f75eca8e50b3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 在终端上用vim写好要配置的库信息：
```
vim Cartfile
```
![Cartfile文件内容](http://upload-images.jianshu.io/upload_images/808722-2091cde8f817eb44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 运行：
```
carthage update --platform iOS    # 仅编译 iOS 平台
```
4. 如果目标工程是 *OS X* 应用， 在 *Xcode* 的目标应用程序 *target* 的 `General` 设置标签中的 `Embedded Binaries` 区域，将框架从 *Carthage.build* 文件夹拖拽进去。*OS X* 工程设置到此为止。
5. 如果是目标工程是 *iOS* 应用，在 *Xcode* 的目标应用程序 *target* 的 `General` 设置标签中的 `Linked Frameworks and Libraries` 区域，将目标框架从 *Carthage/Build* 文件夹拖拽进去。继续接步骤6。
![iOS](http://upload-images.jianshu.io/upload_images/808722-aacae0c89e898e5f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
6. 在 *Xcode* 的目标应用程序 *target* 的 `Build Phases` 添加新脚本 `New Run Script Phase`，输入内容：
```
/usr/local/bin/carthage copy-frameworks
```
![](http://upload-images.jianshu.io/upload_images/808722-fbbe373c506e4624.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
input Files处输入为：
```
$(SRCROOT)/Carthage/Build/iOS/ReactiveCocoa.framework
```
对应为原工程目录 `/ Carthage/Build/iOS/xxx.framework` 文件。
 
7. `Carthage` 中指定编译源码版本，有三种方式：
 1. `github "Alamofire/Alamofire" ~> 3.0`，表示使用版本3.0以上但是低于4.0的最新版本，如3.5, 3.9
 2. `github "Alamofire/Alamofire" == 3.0`，表示使用3.0版本
 3. `github "Alamofire/Alamofire" >= 3.0`，表示使用3.0或更高的版本
 4. `github "Alamofire/Alamofire"`，没有指明版本号，则会自动使用最新的版本

PS：在这个过程当中，*Carthage* 将创建一些 *build artifacts*，其中最重要的是 *Cartfile.lock* 文件，里面将列出每个框架的具体版本，确保你提交了这个文件到版本控制工具里面（如Git、SVN），因为每个用到项目的人都需要它来编译相同版本的框架。完成上面的步骤并提交你的修改，项目的其他用户就只需要获取该仓库并执行 `carthage bootstrap` 就能使用你所添加的框架。