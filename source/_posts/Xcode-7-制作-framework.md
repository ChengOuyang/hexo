---
title: Xcode 7 制作 framework
date: 2017-04-28 11:36:17
categories: 编程
tags: [iOS, Framework, Xcode]
---

#### 创建工程
![创建工程](http://image.xiaomantou.net/Fir9ENe0SJLgJiIKksvnc_M7iGj2 "创建工程")

#### 添加源代码

1. 添加OC源文件
在 swift 制作 framework 添加 OC文件时，不能设置桥接文件，而是将 OC头文件放到框架的头文件中，如下图：
![](http://upload-images.jianshu.io/upload_images/808722-6fa61b4db117f9ae?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
同时将该OC头文件设置为 public（默认添加到Private，可拖动到Public）：
![](http://upload-images.jianshu.io/upload_images/808722-c8bf1ea9c2734d58?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 添加框架源代码
![](http://upload-images.jianshu.io/upload_images/808722-2cc2ef95703636a5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 添加脚本

添加生成模拟器与真机都可使用的 framework 的运行脚本。

1. 给框架工程添加Target：File/New/Target
![](http://upload-images.jianshu.io/upload_images/808722-d0af6b40098c0172?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![脚本内容](http://image.xiaomantou.net/FmotWoB3NZkGWER0du76zdbav66t "脚本内容")
脚本内容如下（脚本内容是从网上搜索到的，但在工程中一直出现问题，无法用于模拟器，后来发现是缺少了`cp -R "${SIMULATOR_DIR}/" "${INSTALL_DIR}/"`）：
```
# Sets the target folders and the final framework product.
# 如果工程名称和Framework的Target名称不一样的话，要自定义FMKNAME
# 例如: FMK_NAME = "MyFramework"
FMK_NAME=${PROJECT_NAME}
# Install dir will be the final output to the framework.
# The following line create it in the root folder of the current project.
INSTALL_DIR=${SRCROOT}/Products/${FMK_NAME}.framework
# Working dir will be deleted after the framework creation.
WRK_DIR=build
DEVICE_DIR=${WRK_DIR}/Release-iphoneos/${FMK_NAME}.framework
SIMULATOR_DIR=${WRK_DIR}/Release-iphonesimulator/${FMK_NAME}.framework
# -configuration ${CONFIGURATION}
# Clean and Building both architectures.
xcodebuild -configuration "Release" -target "${FMK_NAME}" -sdk iphoneos clean build
xcodebuild -configuration "Release" -target "${FMK_NAME}" -sdk iphonesimulator clean build
# Cleaning the oldest.
if [ -d "${INSTALL_DIR}" ]
then
rm -rf "${INSTALL_DIR}"
fi
mkdir -p "${INSTALL_DIR}"
cp -R "${DEVICE_DIR}/" "${INSTALL_DIR}/"
cp -R "${SIMULATOR_DIR}/" "${INSTALL_DIR}/"
# Uses the Lipo Tool to merge both binary files (i386 + armv6/armv7) into one Universal final product.
lipo -create "${DEVICE_DIR}/${FMK_NAME}" "${SIMULATOR_DIR}/${FMK_NAME}" -output "${INSTALL_DIR}/${FMK_NAME}"
rm -r "${WRK_DIR}"
open "${INSTALL_DIR}"
```

2. 编译脚本：Product/Build For/Profiling
![](http://upload-images.jianshu.io/upload_images/808722-c0e81f46ae34d67e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
编译完脚本会自动弹出生成的framework的文件夹。

#### 添加framework到工程

1. 将目标 framework 和其所用到的资源文件拖到目标工程中
![](http://upload-images.jianshu.io/upload_images/808722-5201d15c8f62bcc6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 添加目标框架到复制文件中去
![](http://upload-images.jianshu.io/upload_images/808722-136adf812bf55784?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 打包提交到 App Store

在使用自制的 *Framework* 的应用程序打包提交到 *App Store* 的时候，可能会遇到以下问题`“Unsupported architectures. Your executable contains unsupported architectures '[x86_64, i386]'”`，此时在你**要打包提交的目标程序**的`Build Phases`下添加`Run Script`，并将以下内容复制进去：
```
APP_PATH="${TARGET_BUILD_DIR}/${WRAPPER_NAME}"
# This script loops through the frameworks embedded in the application and
# removes unused architectures.
find "$APP_PATH" -name '*.framework' -type d | while read -r FRAMEWORK
do
FRAMEWORK_EXECUTABLE_NAME=$(defaults read "$FRAMEWORK/Info.plist" CFBundleExecutable)
FRAMEWORK_EXECUTABLE_PATH="$FRAMEWORK/$FRAMEWORK_EXECUTABLE_NAME"
echo "Executable is $FRAMEWORK_EXECUTABLE_PATH"
EXTRACTED_ARCHS=()
for ARCH in $ARCHS
do
echo "Extracting $ARCH from $FRAMEWORK_EXECUTABLE_NAME"
lipo -extract "$ARCH" "$FRAMEWORK_EXECUTABLE_PATH" -o "$FRAMEWORK_EXECUTABLE_PATH-$ARCH"
EXTRACTED_ARCHS+=("$FRAMEWORK_EXECUTABLE_PATH-$ARCH")
done
echo "Merging extracted architectures: ${ARCHS}"
lipo -o "$FRAMEWORK_EXECUTABLE_PATH-merged" -create "${EXTRACTED_ARCHS[@]}"
rm "${EXTRACTED_ARCHS[@]}"
echo "Replacing original executable with thinned version"
rm "$FRAMEWORK_EXECUTABLE_PATH"
mv "$FRAMEWORK_EXECUTABLE_PATH-merged" "$FRAMEWORK_EXECUTABLE_PATH"
done
```
![](http://upload-images.jianshu.io/upload_images/808722-66eccd423305ba62.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*到此就结束了*


#### 参考
1. [Creating your first iOS Framework](https://robots.thoughtbot.com/creating-your-first-ios-framework)