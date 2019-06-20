---
title: 使用Sonar Runner
date: 2017-08-15 17:29:25
tags: CSDN迁移
---
  2. 简单工程  在项目根路径下，创建配置文件，文件名为sonar-project.properties。sonar-runner执行

 分析时，会读取该文件。

 文件内容示意： sonar-project.properties

 
# required metadata

 sonar.projectKey=my:project

 sonar.projectName=My project

 sonar.projectVersion=1.0

 
# path to source directories (required)

 sonar.sources=srcDir1,srcDir2

 
# path to test source directories (optional)

 sonar.tests=testDir1,testDir2

 
# path to project binaries (optional), for example directory of Javabytecode，需要设置，不然findbugs可能报错，没有编译的项目

 sonar.binaries=binDir

 
# optional comma-separated list of paths to libraries. Only path toJAR file and path to directory of classes are

 supported.

 sonar.libraries=path/to/library.jar,path/to/classes/dir

 
# The value of the property must be the key of the language.

 sonar.language=cobol

 
# Additional parameters

 sonar.my.property=value

 配置好上述文件后，从命令行在根路径下执行下面命令启动项目的代码分析。

 sonar-runner

  
  2. 多模块工程  在Sonar分析时可以使用两种方式配置项目的结构。一种需要在项目下配置一个总文件，

 一种可以在每个模块下各自配置一个文件。

 方式一：

 将所有的配置放在一个sonar-project.properties文件，并放在项目的根路径下。

 文件内容示意：

 
# Root project information

 sonar.projectKey=org.mycompany.myproject

 sonar.projectName=My Project

 sonar.projectVersion=1.0-SNAPSHOT

 
# Some properties that will be inherited by the modules

 sonar.sources=src

 
# List of the module identifiers

 sonar.modules=module1,module2

 
# Properties can obviously be overriden for

 
# each module - just prefix them with the module ID

 module1.sonar.projectName=Module 1

 module2.sonar.projectName=Module 2

 方式二：

 每个模块下的配置放在各自的独立文件中。

 总配置的内容 “MyProject/sonar-project.properties” 

 
# Root project information

 sonar.projectKey=org.mycompany.myproject

 sonar.projectName=My Project

 sonar.projectVersion=1.0-SNAPSHOT

 
# Some properties that will be inherited by the modules

 sonar.sources=src

 
# List of the module identifiers

 sonar.modules=module1,module2

 子配置一 “MyProject/module1/sonar-project.properties”

 
# Redefine properties

 
# Note that you do not need to prefix the property here

 sonar.projectName=Module 1

 子配置二 “MyProject/module2/sonar-project.properties”

 
# Redefine properties

 
# Note that you do not need to prefix the property here

 sonar.projectName=Module 2

 值得注意：(1)子配置继承于父配置，子配置将可以覆盖父配置，通过两种方法：

 在父配置中配置属性前增加模块标识前缀；在子配置中直接定义配置。

 (2)特殊情况可以指定根目录

 默认情况下，模块的根目录默认为模块的标识符（如上面的示例）。特殊情况下，可以在配置文件中使用“sonar.projectBaseDir”属性来指定根目录。如：

 module1.sonar.projectBaseDir=My Module One #含空格

 module1.sonar.projectBaseDir=modules/mod1 #多层级

 module2.sonar.projectBaseDir=modules/mod2

 (3)多模块项目使用Sonar做分析时不能只指定一个源代码目录。

 (4)相同结构的多模块

 (5)不同结构的多模块

 (6)模块独自配置

  
  2. 多模块多语言功能  在多模块工程的基础上增加一个语言属性的配置，如：

 module.sonar.language

 module1.sonar.language=Java

 module2.sonar.language=JavaScript

  
  2. 高级用法  如果不在项目的工程的根路径创建sonar-project.properties文件，还可以进行其他选择。

 通过command命令行直接指定：

 sonar-runner -Dsonar.projectKey=myproject-Dsonar.sources=src1 ..

 通过command命令行指定配置文件路径：’project.settings’

 sonar-runner-Dproject.settings=../myproject.properties

 通过设置’sonar.working.directory’属性可以设置Sonar的工作目录，默认为’.sonar’

 通过设 置’project.home’ 属性可 以指定项目的根路 径 。根路径下必须包含

 sonar-project.properties配置文件（除非执行指令时显示给定路径。）

 命令行执行分析时，可以追加参数。

  
  2. 注意  对于较大的工程项目，经常出现内存不够的问题，需要利用环境变量设置虚拟机的内存。

 SONAR_RUNNER_OPTS=-Xmx512m-XX:MaxPermSize=128m

   
  