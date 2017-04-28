---
title: CocoaPods 安装与使用
date: 2017-04-28 12:21:21
categories: 工具
tags: [iOS]
---

#### 安装

1. 打开Finder/应用程序/实用工具/终端；
2. *CocoaPods* 是用 *Ruby* 写的，所以运行需要安装 *Ruby* 环境。*Mac* 中已经自带 *Ruby* 环境，如果认为 *Ruby* 环境不够新，可以先在终端输入以下命令进行更新：
```
gem update –system
```
由于安装 *CocoaPods* 时要访问[cocoapods](https://cocoapods.org)，因为可能被屏蔽了，因此用淘宝的Ruby镜像来访问该网站，在终端输入以下命令进行替换镜像：
```
gem sources --remove https://rubygems.org/
gem sources -a https://ruby.taobao.org/
```
成功后，使用以下命令查看：
```
gem sources –l
```
可以看到替换镜像成功：
```
*** CURRENT SOURCES ***
https://ruby.taobao.org/
```
接下来是真正的安装，在终端输入以下命令（如果 Ruby 版本过低，以下命令会报错）：
```
sudo gem install cocoapods
```
如果 Ruby 版本过低导致无法安装，则运行以下命令进行升级：
```
curl -L get.rvm.io | bash -s stable    // 安装 rvm
source ~/.bashrc  // 更新
source ~/.bash_profile  // 更新
ruby -v  // 查看当前 ruby 版本
rvm list known  // 列出可安装版本
rvm install 2.2  // 安装 ruby 2.2 版本
```
等待安装成功后使用以下命令配置 *cocoapods*：
```
pod setup
```
3. 安装指定版本的cocoapod：
```
sudo gem install cocoapods -v 0.34.4
```

#### 创建工程并配置第三方类库

1. 首先打开 *Xcode* 新建一个工程，假设为 *Desktop/CocoaPodsDemo* ，并且我们要往该工程中导入 *AFNetworking* 这个类库。
*AFNetworking* 在 *Github* 中的地址为：https://github.com/AFNetworking/AFNetworking
这里也说明了如何使用CocoaPods配置该类库：
![配置](http://upload-images.jianshu.io/upload_images/808722-be113f89b0be8c6e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2. 以上说明是我们应该如何书写 *Podfile* 文件。一般非常流行和成熟的类库都得到了 *CocoaPods* 的支持，并且有这个说明。如何确定 *CocoaPods* 是否支持我们想要加入的目标类库？使用 `Search` 命令搜索类库名：
```
pod search AFNetworking
```
如果 *CocoaPods* 支持，将会输出搜索到的所有类库版本和信息，以及在 *Podfile* 中配置的写法，例如：
![Podfile文件](http://upload-images.jianshu.io/upload_images/808722-7c928ad3aeabadb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
3. 先看看普通的工程目录：
![普通工程目录](http://upload-images.jianshu.io/upload_images/808722-86e60d788da8a1c7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
首先在我们的工程目录下创建 *Podfile* 文件，该文件用来控制 *CocoaPods* 的下载内容，该文件是没有后缀的，每个项目只需要一个 *Podfile* 文件，如果需要导入多个类库那么统一在该文件中书写下载内容。
创建过程：首先 `cd` 到工程目录，然后创建 *Podfile* 并且使用 *vim* 编写：
  1. 创建文件：
```
cd Desktop/CocoaPodsDemo/
pod init    # 自动创建 Podfile 文件，也可以使用 touch Podfile 手动创建 Podfile 文件
```
  2. 编写命令：
```
platform:ios, '7.0'
pod "AFNetworking", "~>2.1"
```
*Podfile* 中的两句文字的意思是，当前 *AFNetworking* 支持的 *iOS* 最高版本是 *iOS 7.0*, 要下载的 *AFNetworking* 版本是2.1。
  3. 在有了 *Podfile* 后，在 *Podfile* 文件所在目录下输入以下命令安装类库：
```
pod install
```
安装完成后，输出信息如下：
```
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.1.0)
Generating Pods project
Integrating client project
 [!] From now on use `CocoaPodsDemo.xcworkspace`.
```
最后一句表明，如果要正确打开工程我们应该打开最新生成的 `.xcworkspace` 文件。

#### 编译运行

如果一个项目中已经包含了 *CocoaPods* 的配置文件，但是编译却出现错误，那么我们仅需要一行命令就可以配置好所有的第三方类库了：
```
pod update
```

#### 仅添加要加入的库
```
pod install --verbose --no-repo-update
```

#### 卸载

在终端运行：
```
sudo gem uninstall cocoapods
```