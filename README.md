# Nuttx
## 一、代码下载
* 1.创建Nuttx文件夹:  
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
---
* 2.安装ARM Toolchain  
`apt-git install gcc-arm-none-eabi`
