---
title: MyEclipse 优化技巧
date: 2016-03-23 23:11:33
tags: CSDN迁移
---
  MyEclipse 2015优化技巧

 MyEclipse 2015优化速度方案仍然主要有这么几个方面：去除无需加载的模块、取消冗余的配置、去除不必要的检查、关闭更新。

 第一步: 去除不需要加载的模块   
 一个系统20%的功能往往能够满足80%的需求，MyEclipse也不例外，我们在大多数时候只需要20%的系统功能，所以可以将一些不使用的模块禁止加载启动。通过Windows- Preferences打开配置窗口，依次选择左侧的General–> Startup and Shutdown，这个时候在右侧就显示出了Eclipse启动时加载的模块，可以根据自己的实际情况去除一些模块。   
 选择菜单：Window –> Preferences –>General –> Startup and Shutdown

 可以关掉的启动项有：   
 JSF辅助开发插件   
 1）ICEfaces Integration for MyEclipse；   
 2）JSF Editor Preview Support for MyEclipse；   
 交付相关的插件   
 3）Delivery Runtime JRE   
 4）Delivery Package Runtime UI等3个   
 5）m2e Marketplace；   
 6）Mylyn Tasks UI和Mylyn Team UI；   
 关闭自动更新   
 7）Equinox Provisioning Platform Automatic Update Support   
 第二步：取消MyEclipse的拼写检查   
 拼写检查会给我们带来不少的麻烦，我们的方法命名都会是单词的缩写，MyEclipse会提示有错，所以最好去掉，毕竟我们不是在写英文文章。   
 选择菜单：Window –> Preferences –>General –> Editors –> Text Editors –> Spelling

 取消Enable spellchecking。   
 第三步：取消MyEclipse启动时的自动验证项目配置文件   
 一般来说，我们只需验证XML和JSF文件，其它的验证基本用不上。   
 取消方法：   
 选择菜单：Window –> Preferences –>MyEclipse –> Validation   
 除XML和JSF外，其它的都可以不选。   
 点击Apply按钮，会弹出Validation Settings Changed提示。

 可以把所有Build部分的钩取消掉。   
 手动验证方法：   
 在要验证的文件上，单击鼠标右键–> MyEclipse –> run validation；一样可以达到效果。   
 第四步：换用JDK8   
 选择菜单：Window –> Preferences –>Java –> Installed JREs   
 停用MyEclipse内置的JDK 1.7，改用外部安装的JDK 8。

 紧接着，在Window –> Preferences –> Java –> Compiler   
 选择JDK编译器级别为1.8，点击Apply。

 第五步：关闭Maven自动下载   
 选择菜单：Window –> Preferences –>MyEclipse –> Maven4MyEclipse   
 取消选择Downloadrepository index updates on startup选项，且Maven JDK也选择JDK8。

 第六步：更改JSP默认打开的方式   
 安装了MyEclipse后，编辑JSP页面，会打开JSP的编辑页面，同时也有预览页面，速度很慢。   
 选择菜单：Window –> Preferences –>General –> Editors –> File Associations

 选择MyEclipseJSP Editor编辑器,，然后点击左边的Default按钮。   
 第七步：更改文件编码   
 1）在Window–> Preferences的左上角，输入encod   
 选择Workspace，文字编码改为UTF-8。

 2）Window –>Preferences –> MyEclipse –> Files and Editors –> JSP，编码也改为UTF-8。

   
  