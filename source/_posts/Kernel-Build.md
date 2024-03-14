---
title: Kernel Build
date: 2020-08-16 18:20:13
tags:
    - Android
---

### 查看内核
启动模拟器，查看要编译的内核信息。
{% codeblock %}
xidaokun@ubuntu:~/WORKING_DIRECTORY/out/host/linux-x86/bin$ ./adb shell
generic_arm64:/ # cat /proc/version
Linux version 4.4.124+ (android-build@wphp12.hot.corp.google.com) 
(gcc version 4.9.x 20150123 (prerelease) (GCC) ) #1 SMP PREEMPT Wed Jan 30 07:13:09 UTC 2019
{% endcodeblock %}

### 拉源码
{% codeblock %}
$mkdir kernel
$cd kernel
$git clone https://aosp.tuna.tsinghua.edu.cn/android/kernel/goldfish.git
$git branch -a
$git checkout -b android-goldfish-3.18-dev remotes/origin/android-goldfish-3.18-dev
{% endcodeblock %}


### GNU和NDK
##### 1，下载地址
https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-a/downloads

https://developer.android.google.cn/ndk/downloads


##### 2，配置NDK
{% codeblock %}
export PATH=/home/xidaokun/android-ndk-r25/toolchains/llvm/prebuilt/linux-x86_64/bin:$PATH
{% endcodeblock %}

##### 3，配置源码
{% codeblock %}
export PATH=/home/xidaokun/AOSP/prebuilts/gcc/linux-x86/x86/x86_64-linux-android-4.9/bin:$PATH
export ARCH=x86_64
export CROSS_COMPILE=x86_64-linux-android-
// 查看googel提供的默认配置
find ./arch/x86/ -name *defconfig
// 开始配置，生成.config文件
make x86_64_ranchu_defconfig
//修改配置.config (为了可以gdb调试)
CONFIG_DEBUG_KERNEL=y
CONFIG_DEBUG_INFO=y
CONFIG_FRAME_POINTER=y
CONFIG_KGDB=y 
CONFIG_DEBUG_RODATA=n
CONFIG_RANDOMIZE_BASE=n
//修改配置./arch/arm/mm/Kconfig（为了防止.config的修改被覆盖）
config DEBUG_RODATA
	bool "Make kernel text and rodata read-only"
	depends on ARM_KERNMEM_PERMS
	default n
	help

//修改配置b/arch/x86/Kconfig（为了防止.config的修改被覆盖）
config DEBUG_RODATA
	def_bool n
{% endcodeblock %}

### 编译
{% codeblock %}
make -j8
{% endcodeblock %}
编译成功后会得到./goldfish/arch/arm64/boot/Image -show-kernel

### 模拟器运行
{% codeblock %}
$emulator -avd Pixel_XL_API_28 -show-kernel -verbose -wipe-data -netfast -kernel arch/x86/boot/bzImage -show-kernel  -qemu -s
或者
emulator -kernel ./goldfish/arch/x86_64/boot/bzImage -show-kernel
{% endcodeblock %}







