# Adb 环境配置

Android_SDK 下载：

- platform-tools：

  - https://android-sdk.en.softonic.com/download
  - https://developer.android.com/studio/releases/platform-tools 
- android tools：https://www.androiddevtools.cn/



Android SDK下载：

实测单独安装 `platforms-tools` 不行，缺少执行文件，至少需要三个文件包：

- build-tools 
- tools
- platform-tools

方式一：使用 Android Studio 下载 Android SDK

安装 Android Studio 比较简单，直接 next 就行，安装后打开，这里我选择 `Custom` 自定义配置



或者打开 Android Studio，随便创建个项目或随便打开个文件夹，然后点击左上角`File` - `Settings` - `Appearance & Behavior` - `System Settings` - `Android SDK`，这里我勾选的是最新的 `SDK`。

`SDK Tools` 默认是已经勾选了，如果没有的话也可以自己勾上，然后点击上面的 `Edit` 选择安装目录，点击下载就可以了





![image-20210522080038206](C:\Users\Jamais-vu\AppData\Roaming\Typora\typora-user-images\image-20210522080038206.png)

![](C:\Users\Jamais-vu\AppData\Roaming\Typora\typora-user-images\image-20210522082409860.png)



主要文件：

- build-tools : 主要是Android开发中所需要的工具
- emulator: 主要是用于管理模拟器
- platforms: 存放下载的所有的sdk platforms包
- platforms-tools:主要用我们常用的adb.exe等

配置环境变量：

- D:\Android_SDK\Sdk\build-tools\30.0.3（注意版本号）

- D:\Android_SDK\Sdk\tools

- D:\Android_SDK\platform-tools

# Appium 配置



### Edit  Configurations

- ANDROID_HOME：Andriod sdk 目录
- JAVA_HOME：java jdk 目录

### Desired Capabilities

- **platformName** ：声明是 ios 还是 Android 系统
- **platformVersion** ： Android 内核版本号，可通过命令 `adb shell getprop ro.build.version.release` 查看
- **deviceName** ：连接的设备名称，通过命令 `adb devices -l `中model查看
- **app**：安装包路径
- **appPackage** ：apk的包名， 通过命令 `adb shell dumpsys activity | findstr “mResume”` 查看（需先打开手机应用）或 通过 `aapt dump badging apk路径`
- **appActivity**：apk的 launcherActivity，通过命令 `adb shell dumpsys activity | findstr “mResume”` 查看（需先打开手机应用）或 通过 `aapt dump badging apk路径`
注意：Android 8.1之前应使用 `adb shell dumpsys activity | findstr “mFocus”`

**Appuim 连接真机**

- 手机开发者模式开启调试模式
- 配置 `Desired Capabilities`

- Start Session
- 手机弹窗安装 `io.appium.uiautomator2.server`

**Appuim 连接模拟器**

以夜神模拟器为例：

- 用 `Andriod_SDK\platform-tools` 下的 `adb` 文件替换模拟器安装目录 `bin` 下的 `adb`、`nox_adb`
- 模拟器开启调试模式
- Start Session







