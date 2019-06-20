---
title: Git Master回滚
date: 2017-04-11 17:14:58
tags: CSDN迁移
---
 版权声明：本文为博主原创文章，未经博主允许不得转载。 https://blog.csdn.net/Sunny_Ran/article/details/70062400   
  远端master的回滚，跟dev的回滚不同。

 master的回滚步骤如下：

 亲自测试通过；

 1.首先备份当前的master分支，防止回滚失败。

 方法为：从origin master中新建一个分支，名称随便，比如，master_backup。

 2.备份完成后，将master回滚到指定的版本

 注意这个地方，是将本地的master分支回滚到指定的版本。

 方法：Git reset –hard commit-id

 3.回滚本地master完成后，

 将回滚后的代码push到远端master，用于覆盖远端master分支。

 方法为：通过git命令，必须通过命令来，sourcetree的方式不可行。

 命令为：git push -f origin master。必须有-f，表示强制的意思。

 此时，会要求用户输入远端仓库的用户名和密码，用于确认当前用户具有-f的权限。

 4.push成功后，就可以删除备份的master了。

 方法，也是使用git命令，sourcetree的方式也是不可以的。

 命令：git branch -D master_backup

   
  