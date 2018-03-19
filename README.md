# Nuttx
## 一、环境搭建  
* 1.代码下载  
创建Nuttx文件夹:  
`mkdir Nuttx`
通过git仓库下载核心组件：  
`git clone https://bitbucket.org/nuttx/nuttx.git nuttx`（下载操作系统内核源码）  
`git clone https://bitbucket.org/nuttx/apps.git apps`（下载操作系统builtin app）  
`git clone https://bitbucket.org/nuttx/tools.git tools`（nuttx的一些工具）  
其他可选组件：  
`git clone https://bitbucket.org/nuttx/buildroot.git buildroot`（nuttx提供的build工具）  
`git clone https://bitbucket.org/nuttx/nxwidgets.git NxWidgets`（nuttx提供的图形化界面）  
`git clone https://bitbucket.org/nuttx/pascal.git Pascal`（nuttx提供的pascal脚本解析器）  
`git clone https://bitbucket.org/nuttx/uclibc.git uClibc++`（nuttx提供的c++ stl库）  
* 2.安装ARM Toolchain  
`apt-git install gcc-arm-none-eabi`  
切换路径  
`cd <Desktop>/Nuttx/tools/kconfig-frontends`  
编译kconfig-frontends  
`./configure --enable-mconf`  
`make`  
`make install`  
打开文件 /etc/ld.so.conf  
最后一行添加：  
`include /usr/local/lib`  
终端执行：  
`ldconfig`  
至此配置工具已安装完毕，可以开始编译Nuttx系统了。  
例：  
选择stm32f4discovery开发板bsp的nsh配置：  
`cd <Desktop>/Nuttx/nuttx/tools`  
`./configure.sh stm32f4discovery/nsh`  
`cd ..`  
进入配置界面：  
`make menuconfig`  
将Build Setup --> Build Host Platform由Windows改为Linux  
保存退出  
进行编译：  
`make oldconfig`  
`make`  
编译成功则会在Nuttx/nuttx目录下生成nuttx、nuttx.bin文件  
---
## 二、
