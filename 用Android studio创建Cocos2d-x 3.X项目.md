# 用Android studio创建Cocos2d-x 3.X项目

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 http://blog.csdn.net/u014078216/article/details/49026035

如果你还在用 eclipse 那你就 out 了，Android Studio 从 1.3 版开始支持对 C++ 的编辑（虽然从目前来看用 Android Studio 来编辑 C++ 还比较蛋疼）。而且 Cocos2d-x 从 3.7 版开始优化对 android studio 的支持，相信这一趋势还会继续。可惜目前相关帮助文档还不完善，很多东西需要自己摸索。在此对自己所学所得做一些总结，也希望能帮助到那些处在摸索之中的朋友们。

本文所用 Cocos2d-x 为 3.8 版，Android Studio 为 1.3.2 版。Mac OS X 系统亲测有效。Windows 系统如有出入请见谅。

# 准备工作

1) 官网下载并安装 Android Studio，并通过其中的 AVD manager 和 SDK manager 下载配置相应的安卓模拟器和安卓开发包。

2) 官网下载 Android NDK。用于支持与原生 C++ 代码混编。

3) 官网下载并安装 JAVA JDK。可以通过 terminal 或者 cmd 输入 java -version 进行检测。

4) 官网下载 Cocos2d-x。

5) Windows 系统还需要官网下载并安装 python。Mac 自带 python。

# 开干正事

1) 打开 terminal 或者 cmd，运行 cd 进入你的 cocos2d-x 文件夹，我的就是 cd ~/Documents/cocos2d-x-3.8。<br><br>
2) 运行./setup.py，进行环境配置。在运行该命令时可能需要更高权限，因此需要在命令前加 sudo，也就是 sudo ./setup.py （可能会要求输入管理员密码）。在这一步中会配置 COCOS_CONSOLE_ROOT, COCOS_TEMPLATES_ROOT 环境变量。还会检查是否配置了 ANDROID_SDK_ROOT 和 NDK_ROOT 两个环境变量，如果之前没有配置过会要求进行配置。如果配置过当然也可以视需要对其进行修改。Mac 可以通过以下两条命令进行配置：export ANDROID_SDK_ROOT="/Users / 你的用户名 / Libray/android/sdk" 和 export NDK_ROOT="你的 android ndk 存放路径"。可以打开 Android Studio 进入 Preferences->Appearance & Behavior->System Settings->Android SDK 看看里面路径是否和环境变量配置一致。Windows 的环境变量需要通过计算机 -> 系统属性 -> 高级系统属性 -> 环境变量进行配置。<br>
最后还会要求运行 source Users/XXX/.bash_profile 使配置生效。<br><br>
3) 运行 cocos new HelloWorld -p com.memeda.HelloWorld -l cpp -d ~/Documents，新建 HelloWorld 项目。由于我们通过第二步配置好了环境，cocos 命令才能在任意目录下运行。

4) 运行 cd 进入新建的项目目录下，我的是 cd ~/Documents/HelloWorld。注意如果在上一步命令中写的是 - d ~/Documents/HelloWorld，那么要进入第二层 HelloWorld 才行，也就是 cd ~/Documents/HelloWorld/HelloWorld。

5) 运行 cocos compile -p android --android-studio，进行编译。这一步会在 proj.android-studio/app 下生成一个 libs 文件夹，里面是编译出来的 libcocos2dcpp.so 库文件。**注意命令里含有 --android-studio，这是专门针对 Android Studio 进行编译。**如果不运行这一步，原生 C++ 代码无法运行。

另外还要注意，如果不是新建的 HelloWorld 项目，而是已经添加了其他 C++ 源文件的项目，那么直接运行这一步会出现如下报错：error: undefined reference to 'vtable for XXX'。其中 XXX 就是某个源文件名。这是因为编译器没有在 Android.mk 文件里面找到相关源文件的地址。所以解决办法就是在这一步之前再添一步，用 vi，或者 nano，或者其他你喜欢的方式打开 proj.android-studio/app/jni/Android.mk，往里面添加相关源文件地址（只要你打开这个 mk 文件一看就秒懂）。

6) 打开 Android Studio，加载已有项目，也就是载入 HelloWorld 下面的 **proj.android-studio 文件夹**（这是 cocos2d-x 3.7 版之后才出现的，原来只有 proj.android 文件夹）。<br><br>
7) 打开模拟器，运行项目，成功！

水平有限，如有不妥，欢迎指正！