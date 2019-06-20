---
title: Vss 批量处理
date: 2016-12-09 15:07:31
tags: CSDN迁移
---
  批量analyze:

 第一步，新建一个txt文档可以叫analyze.txt   
 第二步，在analyze.txt中输入你的vss安装目录   
 第三步，输入analyze -C -D -F -V4 \XXX（服务器名）\XXX（项目名）\data   
 如果有很多项目只要重复第三步就可以了   
 第四步：把analyze.txt的后缀改成.bat   
 第五步：双击analyze.bat   
 ok了，它就会一个项目接着一个项目的analyze了，就不用你一个一个analyze那么麻烦了！

 例如：我在d盘的vss下安装我的vss就可以这样编辑：   
 d：   
 cd vss   
 analyze -C -D -F -V4 \XXX\XXX\data   
 analyze -C -D -F -V4 \XXX\YYY\data   
 analyze -C -D -F -V4 \XXX\ZZZ\data   
 这样就可以了，简单吧！

 自动从vss下载代码并编译

 
```
rem =============================================================================================
rem 自动编译工具1.0  gjung 2008-11-1 
rem =============================================================================================

set backupfolder=D:\work\yxjxh\yxjxh项目产品\3代码\backup

set serverfolder=$/yxjxh项目产品/3代码/CWORKSNET
set workfolder=D:\work\yxjxh\yxjxh项目产品\3代码\CWORKSNET

set serverfolder_sql=$/yxjxh项目产品/资料备份/gjung/人力资源/实施/SqlScript
set workfolder_sql=D:\work\yxjxh\yxjxh项目产品\资料备份\gjung\人力资源\实施\SqlScript

set compilefolder=D:\work\yxjxh\yxjxh项目产品\3代码\CWORKSNET\PrecompiledWeb
set testfolder=D:\work\yxjxh\yxjxh项目产品\资料备份\gjung\人力资源\送测\SD_YXJXH-RL08010



set t0=%TIME:~0,1%
set logTIME=_%DATE:~0,4%%DATE:~5,2%%DATE:~8,2%_%TIME:~0,2%%TIME:~3,2%%TIME:~6,2%
if "%t0%"==" " set logTIME=_%DATE:~0,4%%DATE:~5,2%%DATE:~8,2%_0%TIME:~1,1%%TIME:~3,2%%TIME:~6,2%

set logfile0=%testfolder%生成记录%logTIME%.log

ECHO ......脚本程序开始运行时间：[%DATE:~0,10% %TIME:~0,8%] 
ECHO ......脚本程序开始运行时间：[%DATE:~0,10% %TIME:~0,8%]  >> %logfile0%


rem 停止Web服务器
net stop w3svc
rem ------------------------------------------------------------


@rem 设置ss.exe路径
Path=%PATH%;E:\Program Files\Microsoft Visual SourceSafe\;E:\Program Files\WinRAR;

@rem 设置配置库所在目录
Set ssDir=\\192.168.1.253\yxjxh

@rem 设置vss用户名密码
Set ssUser=gjg
Set ssPwd=gjung


rem **********************下载最新数据库脚本开始**********************
ECHO .....................下载最新数据库脚本开始 >>%logfile0% 
@rem 指定项目路径与本地目录
ss cp %serverfolder_sql%  >> %logfile0%
ss workfold %serverfolder_sql% %workfolder_sql%  >> %logfile0%

rem 备份文件
rem rar a -ed  -ag[YYYY-MM-DD] %backupfolder%\backup_sql.rar  %workfolder_sql%  >> %logfile0%
rem ----------------------------------------------------------

rem 删除原有文件
rmdir %workfolder_sql%  /S /Q >> %logfile0%
rem ----------------------------------------------------------

rem 创建目录
mkdir %workfolder_sql% >> %logfile0%
rem ----------------------------------------------------------

rem 从SourceSafe下载最新文件
cd  %workfolder_sql%     >> %logfile0%
d:
ss Get %serverfolder_sql%  -R -W -I-Y  >> %logfile0%
rem ----------------------------------------------------------
ECHO .....................下载最新数据库脚本结束 >>%logfile0% 
rem **********************下载最新数据库脚本结束**********************


rem **********************下载最新程序开始**********************
ECHO .....................下载最新程序开始 >>%logfile0% 
@rem 指定项目路径与本地目录
ss cp %serverfolder%   >> %logfile0%
ss workfold %serverfolder% %workfolder%  >> %logfile0%

rem 备份文件
rem rar a -ed   -ag[YYYY-MM-DD] %backupfolder%\backup.rar   %workfolder%  >> %logfile0%
rem ----------------------------------------------------------

rem 删除原有文件
rmdir %workfolder%  /S /Q  >> %logfile0%
rem ----------------------------------------------------------

rem 创建目录
mkdir %workfolder%  >> %logfile0%
rem ----------------------------------------------------------

rem 从SourceSafe下载最新文件
cd  %workfolder%
d:
ss Get %serverfolder%  -R -W -I-Y  >> %logfile0%
rem ----------------------------------------------------------

rem 更改文件属性
ATTRIB -R %workfolder%*.*  >> %logfile0%
rem ----------------------------------------------------------
ECHO .....................下载最新程序结束 >>%logfile0% 
rem **********************下载最新程序结束**********************


rem **********************编译整个解决方案、发布网站开始**********************
ECHO ....................编译整个解决方案、发布网站开始 >>%logfile0% 
@Set Path=E:\Program Files\Microsoft Visual Studio 8\SDK\v2.0\Bin;E:\WINDOWS\Microsoft.NET\Framework\v2.0.50727;E:\Program Files\Microsoft Visual Studio 8\VC\bin;E:\Program Files\Microsoft Visual Studio 8\Common7\IDE;E:\Program Files\Microsoft Visual Studio 8\VC\vcpackages;%PATH%
@Set LIB=E:\Program Files\Microsoft Visual Studio 8\VC\lib;E:\Program Files\Microsoft Visual Studio 8\SDK\v2.0\Lib;%LIB%
@Set INCLUDE=E:\Program Files\Microsoft Visual Studio 8\VC\include;E:\Program Files\Microsoft Visual Studio 8\SDK\v2.0\include;%INCLUDE%
@Set NetSamplePath=E:\Program Files\Microsoft Visual Studio 8\SDK\v2.0
@Set VCBUILD_DEFAULT_CFG=Debug^|Win32
@Set VCBUILD_DEFAULT_OPTIONS=/useenv
@echo Setting environment to use Microsoft .NET Framework v2.0 SDK tools.
@echo For a list of SDK tools, see the 'StartTools.htm' file in the bin folder.

msbuild %workfolder%\CWorksNet.sln  >> %logfile0%
ECHO ....................编译整个解决方案、发布网站结束 >>%logfile0% 
rem **********************编译整个解决方案、发布网站结束**********************


rem **********************拷贝数据到发布目录开始**********************
ECHO ....................拷贝数据到发布目录开始 >>%logfile0%

rem 删除原有文件
rem rmdir %testfolder%  /S /Q >> %logfile0%
rem ----------------------------------------------------------


rem 拷贝数据库脚本到发布目录
rem xcopy %workfolder_sql% %testfolder% /s /e /h /i /y  >> %logfile0%
rem ----------------------------------------------------------

rem 拷贝编译后的程序到发布目录
rem xcopy %compilefolder% %testfolder% /s /e /h /i /y  >> %logfile0%
rem ----------------------------------------------------------

ECHO ....................拷贝数据到发布目录结束>>%logfile0% 
rem **********************拷贝数据到发布目录结束**********************


rem 重新启动IIS
net start w3svc
rem --------------------------


ECHO [%DATE:~0,10% %TIME:~0,8%]完成。
ECHO [%DATE:~0,10% %TIME:~0,8%]完成。 >> %logfile0%

rem =======================end ======================================

pause
```
 自动备份脚本

 
```
@echo off 
@title Backing up SourceSafe databases

SET VSS_DB=D:\vss_data
set BakPath=D:\vss_data_bak\
set VSS_Admin_Name="admin"
set VSS_Admin_Password="Your Admin Password" 
FOR /F "tokens=1-3 delims=- " %%i IN ('date /t') DO SET DATE=%%i-%%j-%%k
ssarc.exe -d- -s%VSS_DB% -y%VSS_Admin_Name%,%VSS_Admin_Password% -o%BakPath%Backup_output_log(%DATE%).txt -l %BakPath%Backup_Database(%DATE%).ssa $/

if errorlevel 1 (
echo 备份失败
) ELSE (
echo 备份完成
)

@echo on
```
 解释一下：   
 VSS_DB VSS的安装目录   
 VSS_Admin_Name VSS管理员账号   
 VSS_Admin_Password VSS管理员密码

 FOR /F “tokens=1-3 delims=- ” %%i IN (‘date /t’) DO SET DATE=%%i-%%j-%%k 设置一个获取当前日期的变量，用于每日备份的文件名   
 ssarc.exe -d- -s%VSS_DB% -y%VSS_Admin_Name%,%VSS_Admin_Password% -o%BakPath%Backup_output_log(%DATE%).txt -l %BakPath%Backup_Database(%DATE%).ssa $/ BAT文件的核心：使用了VSS安装路径下的 ssarc.exe 这个实用的工具来备份指定的VSS DB.

 需要的操作：   
 1、进入备份服务器，在桌面上新建一个以.bat结尾的文件，如vss.bat   
 2、开始——程序——附件——系统工具——任务计划——添加任务计划   
 3、选择保存的我们做成的VSS备份的bat文件，设置执行频率为“每日”，时间可设置为凌晨1、2点钟，那时大家应该都下班了吧。   
 4 OK！大功告成，不用管备份的事情了。不过一定得把备份服务器的硬盘弄大一点，项目大了的话，备份文件还是挺大的，小心撑爆硬盘，过了保质期后，可以手动废掉一些过期的备份文件。（需要测试一下你备份的文件是否可以恢复哟）

   
  