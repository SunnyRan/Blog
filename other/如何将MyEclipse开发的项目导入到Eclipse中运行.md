---
title: 如何将MyEclipse开发的项目导入到Eclipse中运行
date: 2016-03-23 23:11:59
tags: CSDN迁移
---
  由于以前的项目都是用myeclipse开发的，现在要换成eclipse来开发。但是项目导入到eclipse中发现该项目并不是web项目，也不能部署到tomcat里面去。 现在解决了这个问题了。

 
```
   一.请首先确保你的机器上的eclipse是javaee版本的，或者已经安装看wtp插件 

   二.先Close Project,然后修改eclipse工程下的.project文件： 

      在 <natures> </natures>中加入 

<nature>org.eclipse.wst.common.project.facet.core.nature</nature> 

<nature>org.eclipse.wst.common.modulecore.ModuleCoreNature</nature> 

<nature>org.eclipse.jem.workbench.JavaEMFNature</nature> 

```
 在 中加入 

 
```
 <buildCommand> 

    <name>org.eclipse.wst.common.project.facet.core.builder</name> 

    <arguments> 

    </arguments> 

</buildCommand> 

<buildCommand> 

    <name>org.eclipse.wst.validation.validationbuilder</name> 

    <arguments> 

    </arguments> 

</buildCommand> 

```
 三.Open Project,然后刷新项目，在项目->右击->Properties->Project Facets->Modify Project，选择Java和Dynamic Web Module   
 配置Context Root 和Content Directory 以及源码路径。 

 这样就解决了。

 方法二：

 在直接Import   
 MyEclipse的项目文件导入到Eclipse之后，需要在项目所放的workspace内修改引入项目目录下的.project文件，修改如下：   
 1、在eclipse中新建一个WEB项目将根目录下下的.project文件覆盖到导出的项目同样目录下，   
 2、打开导入项目的.project文件，修改下test中间的值即可   
 之后，刷新项目工程文件。继而，右键项目——>Properties——>选择Project Facets,勾选Dynamic Web Module以及Java，在Runtime内勾选Tomcat。   
 然后进入项目目录，在其.settings的目录下修改org.eclipse.wst.common.component内   
   
 然后再刷新项目工程文件，删除WebContent，即可在Tomcat中发布并运行。

   
  