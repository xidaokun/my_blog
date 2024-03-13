---
title: AOSP Build
date: 2020-08-15 15:27:45
tags:
    - Android
---

### 安装Repo
1，下载
{% codeblock %}
$mkdir ~/bin
$PATH=~/bin:$PATH
$curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$chmod a+x ~/bin/repo
{% endcodeblock %}

2，将repo url改为国内镜像url
将REPO_URL = 'https://gerrit.googlesource.com/git-repo'，改为
REPO_URL = 'https://aosp.tuna.tsinghua.edu.cn/android/git-repo'


### 检出分支
{% codeblock %}
$mkdir AOSP
$cd AOSP
$repo init -u https://aosp.tuna.tsinghua.edu.cn/android/platform/manifest
$repo init -u https://aosp.tuna.tsinghua.edu.cn/android/platform/manifest -b android-9.0.0_r18
{% endcodeblock %}

### 下载源码
{% codeblock %}
$repo sync
{% endcodeblock %}

### 安装jdk
{% codeblock %}
$sudo apt-get install openjdk-8-jdk
{% endcodeblock %}

### 安装依赖
{% codeblock %}
$sudo apt-get install -y git flex bison gperf build-essential libncurses5-dev:i386
$sudo apt-get install libx11-dev:i386 libreadline6-dev:i386 libgl1-mesa-dev g++-multilib
$sudo apt-get install tofrodos python-markdown libxml2-utils xsltproc zlib1g-dev:i386
$sudo apt-get install zip curl zlib1g-dev gcc-multilib g++-multilib
$sudo apt-get install lib32ncurses5-dev x11proto-core-dev libx11-dev 
$sudo apt-get install lib32z-dev ccache
{% endcodeblock %}

### 配置ccache
{% codeblock %}
$export USE_CCACHE=1  (在你home主目录的.bashrc中加入)
$export CCACHE_DIR=/home/ubuntu/.ccache  (指定一个缓存目录，也可以不指定，默认目录为你当前用户目录下的.ccache)
$prebuilts/misc/linux-x86/ccache/ccache -M 50G （这个命令在Android源码中，缓存大小按照自己的硬盘来适当调整）
$source ~/.bashrc  （source命令使修改立即生效）
{% endcodeblock %}

### 编译
{% codeblock %}
//向shell脚本中写入环境变量
$source build/envsetup.sh
// 选择构建目标
$lunch (lunch选定的版本按照实际情况来)
// 开始编译
$make -j16  （和cpu有关，适当调整数字）
{% endcodeblock %}

### 启动模拟器
{% codeblock %}
//在编译完成后可以直接运行，若关闭了终端, 或重新加载了环境, 则有可能导致执行错误,建议执行前确认是否已经执行
//source ./build/envsetup.sh
//lunch xxxx
$ emulator
{% endcodeblock %}

### 遇到的问题
1，prebuilts/clang/host/linux-x86/clang-3289846/bin/clang.real: error while loading shared libraries: libncurses.so.5: cannot open shared object file: No such file or directory
{% codeblock %}
//安装依赖库
sudo apt install libncurses5
{% endcodeblock %}


2，解决python版本问题
{% codeblock %}
//安装python2.7
sudo apt-get install python2-minimal
// 重置软连接
sudo ln -s /usr/bin/python2.7 python
{% endcodeblock %}

3，[  2% 1921/83098] Lex: checkpolicy <= external/selinux/checkpolicy/policy_scan.l
FAILED: out/host/linux-x86/obj/EXECUTABLES/checkpolicy_intermediates/policy_scan.c 
/bin/bash -c "prebuilts/misc/linux-x86/flex/flex-2.5.39 -oout/host/linux-x86/obj/EXECUTABLES/checkpolicy_intermediates/policy_scan.c external/selinux/checkpolicy/policy_scan.l"
flex-2.5.39: loadlocale.c:130: _nl_intern_locale_data: Assertion `cnt < (sizeof (_nl_value_type_LC_TIME) / sizeof (_nl_value_type_LC_TIME[0]))' failed.

{% codeblock %}
//重新编译flex
cd prebuilts/misc/linux-x86/flex
rm flex-2.5.39
tar zxf flex-2.5.39.tar.gz
cd flex-2.5.39
./configure
make
mv flex ../
cd ../
rm -rf flex-2.5.39
mv flex flex-2.5.39
{% endcodeblock %}

4，android p build failing - apache-xml_intermediates - hiddenapi
{% codeblock %}
$ make clean-ims-common
$ make ims-common
$ make apache-xml
$ make -j12
{% endcodeblock %}

5，Try increasing heap size with java option '-Xmx<size>'
{% codeblock %}
$export JACK_SERVER_VM_ARGUMENTS="-Dfile.encoding=UTF-8 -XX:+TieredCompilation -Xmx4g"
$./prebuilts/sdk/tools/jack-admin kill-server
$./prebuilts/sdk/tools/jack-admin start-server
{% endcodeblock %}

6，make sdk
SDK: warning: including GNU target out/target/product/generic/system/lib/libext2fs.so SDK: warning: including GNU target out/target/product/generic/system/lib/libiprouteutil.so

SDK: warning: including GNU target out/target/product/generic/system/lib/libmicrohttpd.so

SDK: warning: including GNU target out/target/product/generic/system/lib/libnetlink.so

SDK: warning: including GNU target out/target/product/generic/system/lib/libnl.so

SDK: warning: including GNU target out/target/product/generic/system/lib/libxmlrpc++.so

SDK: warning: including GNU target out/target/product/generic/system/xbin/fio

SDK: warning: including GNU target out/target/product/generic/system/xbin/mmc_utils

development/build/sdk.atree:49: couldn't locate source file: bin/etc1tool

development/build/sdk.atree:83: couldn't locate source file: bin/split-select

development/build/sdk.atree:88: couldn't locate source file: bin/bcc_compat

development/build/sdk.atree:203: couldn't locate source file: framework/layoutlib.jar

development/build/sdk.atree:588: couldn't locate source file: framework/layoutlib-tests.jar

development/build/sdk.atree:589: couldn't locate source file: system/app/EmulatorSmokeTests/EmulatorSmokeTests.apk

ninja: build stopped: subcommand failed.

make: *** [ninja_wrapper] Error 1
{% codeblock %}
注释以上development/build/sdk.atree 中的 49 83 88 203 586 589行的内容   再次make sdk
编译成功，在相关目录下生成以下的文件:
Package SDK: out/host/linux-x86/sdk/aosp_arm/android-sdk_eng.builder_linux-x86.zip
{% endcodeblock %}






