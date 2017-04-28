---
title: iOS 国际化
date: 2017-04-28 12:19:09
categories: 编程
tags: [iOS]
---

![](http://upload-images.jianshu.io/upload_images/808722-4e513244a3d423f3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 应用国际化准备

1. 点击`工程/PROJECT/Info/Localizations`，添加简体中文支持，如果想支持繁体，也可继续添加，其他语言亦然。
![](http://upload-images.jianshu.io/upload_images/808722-c2430fb5b2f0b2d5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 修改`Info.plist`文件，添加`Application has localized display name`，值为 `Boolean` 类型的 `YES`
![
](http://image.xiaomantou.net/FuxJ7t1NeU5cJNp1dMLAfvSbzoqT)

#### 应用名称国际化

1. 创建`InfoPlist.strings`，文件名必须为`InfoPlist`，否则无效。
![](http://upload-images.jianshu.io/upload_images/808722-b3339186b83c43e7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/808722-c5d71a42018259d3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 点击之前创建的`InfoPlist.strings` - 点击右边的`Localizion/添加简体中文`
![](http://upload-images.jianshu.io/upload_images/808722-5e0eebb7710c59c4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 在`InfoPlist.strings`文件中对应的语言文件填入应用的名称
```
"CFBundleDisplayName" = "English";
```
![](http://upload-images.jianshu.io/upload_images/808722-bf7274872fa97c84?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/808722-463026e17dd37eb4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 应用文字国际化

1. 按上边`应用名称国际化`步骤创建 `Localizable.strings` 文件
![](http://upload-images.jianshu.io/upload_images/808722-728a5fbbc60759fd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/808722-c76cb0efc27635f7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 在`Localizable.strings`文件中对应的语言文件填入对应内容的键值对
![](http://upload-images.jianshu.io/upload_images/808722-2a542f7d63ad63a4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/808722-728a5fbbc60759fd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3. 使用国际化文字
```
let loginStr = NSLocalizedString("account_login", comment: "");
```

#### 应用图标国际化

`有机会更新^-^`

以下是测试工程的文件：
![](http://upload-images.jianshu.io/upload_images/808722-712d5e13e773c811?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)