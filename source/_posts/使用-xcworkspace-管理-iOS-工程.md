---
title: 使用 xcworkspace 管理 iOS 工程
date: 2017-04-28 11:34:07
categories: 编程
tags: [iOS, Xcode]
---

1. 首先创建目标工程
![](http://upload-images.jianshu.io/upload_images/808722-93579b276bc93fa3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![](http://upload-images.jianshu.io/upload_images/808722-4cd04d007ff6d5e2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

2. 创建工作空间`xcworkspace`文件，并将创建的`*.xcworkspace`文件放到刚创建的目标工程同级目录下
![](http://upload-images.jianshu.io/upload_images/808722-6d983cd8d472954d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![](http://upload-images.jianshu.io/upload_images/808722-9cfe3f4ff8feeec6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 关闭刚刚创建的目标工程，打开`*.xcworkspace`文件，把刚刚创建的目标工程添加到工作空间中来
![](http://upload-images.jianshu.io/upload_images/808722-dab6c30a1c207ed0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![](http://upload-images.jianshu.io/upload_images/808722-167e788bd16469c8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![](http://upload-images.jianshu.io/upload_images/808722-6c6a78020755b51d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  

4. 如果要添加一些框架，则将目标框架放到与目标工程同级目录下
![](http://upload-images.jianshu.io/upload_images/808722-f7ff128d3688e3c5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/808722-3829418ecfecf67a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5. 与`步骤3`一样，将框架添加到工作空间中来
![](http://upload-images.jianshu.io/upload_images/808722-930aa09ff00d97d7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/808722-f6e24dd5ba35db97?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6. 在目标工程中引入框架
![](http://upload-images.jianshu.io/upload_images/808722-fc384fad36f87ba8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![](http://upload-images.jianshu.io/upload_images/808722-57719786b7321759?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  
![](http://upload-images.jianshu.io/upload_images/808722-49386ebec0e4cc4d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

7. 接下来就可以在目标工程中使用引入的框架
![](http://upload-images.jianshu.io/upload_images/808722-2c198d51d087bc6b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
*PS：如果要使用到引入框架的方法，需要将对应的类和其方法设置为`public`，具体在[Xcode 7 制作 framework](/2017/04/28/Xcode-7-制作-framework/)里有介绍。*