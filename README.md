# Nuttx #
## 一、Linux下环境搭建   
### 1.代码下载   
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
### 2.安装ARM Toolchain   
`apt-get install gcc-arm-none-eabi`  
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
### 3.例：  
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
## 二、系统移植   
### 1.创建自己的板卡配置，以stm32f407为例   
在Nuttx/nuttx/config目录下新建目录stm32f4  
将Nuttx/nuttx/config/stm32f4discovery中的src、include、scripts、nsh复制到stm32f4目录下  
scripts目录下只保留ld.scripts和Make.defs  
src目录下  
Makefile修改为：    
`-include $(TOPDIR)/Make.defs`  
`ASRCS =`  
`CSRCS = stm32_boot.c`  
`include $(TOPDIR)/configs/Board.mk`  
stm32_boot.c修改为：  
`#include <stdio.h>`  
`#include "stm32.h"`  
`int board_app_initialize(uintptr_t arg)`  
`{`  
	`return OK;`  
`}`  
`void stm32_boardinitialize(void)`  
`{`  

`}`  
更改主板信息：  
打开/Nuttx/nuttx/configs/stm32f4/scripts中的make.defs文件  
将`ARCHSCRIPT = -T$(TOPDIR)/configs/$(CONFIG_ARCH_BOARD)/scripts/$(LDSCRIPT)`  
改为：  
`ARCHSCRIPT = -T$(TOPDIR)/configs/$(CONFIG_ARCH_BOARD_CUSTOM_NAME)/scripts/$(LDSCRIPT)`  
切换到nuttx目录下  
`make distclean`  --彻底清除nuttx当前的板级配置  
`cd tools`  
`./configure.sh stm32f4/nsh` --加载自己的创建的板卡  
`cd ..`  
`make oldconfig`  --应用配置  
更改源码的定位：  
`make menuconfig`  
将Board Selection --> Select target board(STMicro STM32F4-Discovery board) 修改为 Custom development board  
将Board Selection --> Custom Board Configuration --> Custom board name 修改为 stm32f4  
将Board Selection --> Custom Board Configuration --> Custom board directory 修改为 configs/stm32f4  
开始编译：  
`make`  
### 2.nuttx基本功能开启   
`make menuconfig`  
* 1.定制输出文件  
如果不需要生成.hex文件  
Build Setup --> Binary Output Formats中将\[ \]Intel HEX binary format取消选中即可  
* 2.打开调试信息  
Build Setup --> Debug Options中选中：  
\[\*\]Enable Debug Features  
\[\*\]Enable Error Output  
\[\*\]Enable Warnings Output  
\[\*\]Enable Informational Debug Output  
* 3.开启浮点运算单元  
System Type中选中：  
\[\*\]Use BASEPRI Register  
\[\*\]Use common ARMv7-M vectors  
\[\*\]FPU support  
修改Nuttx/nuttx/configs/stm32f4/scripts目录下的ld.script  
将`ENTRY(_stext)`  
改为：  
`ENTRY(__start)`  
`EXTERN(_vectors)`  
* 4.关闭未使用的功能和系统总线  
System --> STM32 Peripheral Support  
只保留\[\*\]USART2  
* 5.修改ROTS Features  
ROTS Features 取消选中  
\[ \]Disable Nuttx interfaces  
ROTS Features --> Clocks and Timers将系统时间力度改为1000：  
(1000)System timer tick period(microseconds)  
* 6.修改Device Drivers  
Device Drivers 取消选中  
\[ \]Disable driver poll interfaces  
\[ \]Enable /dev/null  
* 7.关闭文件系统  
File Systems 取消选中  
\[ \]PROCFS File System  
* 8.开启数学库支持  
Library Routines 选中  
\[\*\]Standard Math library  
* 9.打开板级回调函数  
Application Configuration --> NSH Library 选中  
\[\*\]Have architecture-specific initializetion  







