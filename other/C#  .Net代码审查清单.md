---
title: C#  .Net代码审查清单
date: 2017-08-14 15:28:16
tags: CSDN迁移
---
  这是为C#开发者准备的通用性代码审查清单，可以当做开发过程中的参考。这是为了确保在编码过程中，大部分通用编码指导原则都能注意到。对于新手和缺乏经验（0到3年工作经验）的开发者，参考这份清单编码会很帮助。

 清单   
 1. 确保没有任何警告（warnings）。

 2.如果先执行Code Analysis（启用所有Microsoft Rules）再消除所有警告就更好了。

 3. 去掉所有没有用到的usings。编码过程中去掉多余代码是个好习惯。（参考：[msdn](http://msdn.microsoft.com/en-us/magazine/ee335722.aspx)）

 4. 在合理的地方检查对象是否为’null’，避免运行的时候出现Null Reference Exception。

 5. 始终遵循命名规范。一般而言变量参数使用驼峰命名法，方法名和类名使用Pascal命名法。（参考：[msdn](http://msdn.microsoft.com/en-us/library/ms229043.aspx)）

 6. 请确保你了解SOLID原则。

 根据维基百科定义：在程序设计领域，SOLID (单一功能、开闭原则、里氏替换、接口隔离以及依赖反转)是由罗伯特·C·马丁在21世纪早期引入的记忆术首字母缩略字，指代了面向对象编程和面向对象设计的五个基本原则。当这些原则被一起应用时，它们使得一个程序员开发一个容易进行软件维护和扩展的系统变得更加可能。SOLID所包含的原则是通过引发编程者进行软件源代码的代码重构进行软件的代码异味清扫，从而使得软件清晰可读以及可扩展时可以应用的指南。SOLID被典型的应用在测试驱动开发上，并且是敏捷开发以及自适应软件开发的基本原则的重要组成部分。参考：[wiki/SOLID_(面向对象设计)](http://zh.wikipedia.org/wiki/SOLID_%28%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E8%AE%BE%E8%AE%A1)

 7. 代码可重用性：如果一块代码已经被使用超过一次，或者你希望将来使用它，请提取成一个方法。将重复的工作做成通用的方法放在相关的类中，这样一旦你完成别人就可以使用了。将常用功能开发成用户控件，这样可以跨项目重用它们。（参考：[①](http://msdn.microsoft.com/en-us/library/office/aa140806%28v=office.10%29.aspx) 、 [②](http://blogs.msdn.com/b/frice/archive/2004/06/11/153709.aspx)）

 8. 代码一致性：比方说，Int32写成int，String写成string，应该在代码里保持统一形式。不能一会二写成int一会儿写成Int32。

 9. 代码可读性：代码应该是可维护的，便于其他开发者理解。（参考：[msdn](http://msdn.microsoft.com/en-IN/library/aa291591%28v=vs.100%29.aspx)）

 10. 释放非托管资源，比如文件I/O，网络资源等。一旦使用结束就应该释放它们。如果你想一旦超出使用范围就自动释放对象，可以使用usings将非托管代码括起来。参考：[msdn](http://msdn.microsoft.com/en-us/library/498928w2.aspx)

 11. 合理实现异常处理(try/catch和finally块)和异常记录。参考：[msdn](http://msdn.microsoft.com/en-us/library/vstudio/ms229005%28v=vs.100%29.aspx)

 12. 确保代码中方法的行数不要过多，不超过30到40行。

 13. 及时用代码管理工具check-in/check-out代码。(比如TFS) 参考：[codeproject.com](http://www.codeproject.com/Tips/593014/Steps-Check-in-Check-Out-Mechanism-for-TFS-To-avoi)

 14. 相互审查代码：和你的同事交换代码，实现内部审查。

 15. 单元测试：编写开发测试用例完成单元测试，确保代码被送到QA以前，基本测试完成。参考：msdn

 16. 尽量避免for/foreach循环嵌套和if条件嵌套。参考:[避免if代码嵌套](http://blog.csdn.net/qq_34986769/article/details/62041345)

 17. 如果代码只会使用一次，请使用匿名类型。参考：[msdn](http://msdn.microsoft.com/en-us/magazine/cc163665.aspx)

 18. 尽量使用LINQ查询和Lambda表达式，增加可读性。参考：[msdn](http://msdn.microsoft.com/en-us/library/bb308959.aspx)

 19. 合理使用var、object和dynamic关键字。由于很多开发者会感到困惑或者知道的很少，会觉得它们有些相似，故而交换使用，这是要避免的。参考：[blogs.msdn](http://blogs.msdn.com/b/csharpfaq/archive/2010/01/25/what-is-the-difference-between-dynamic-and-object-keywords.aspx)

 20. 使用访问限定符(private, public, protected, internal, protected internal)限定每个方法、类或变量的需要范围。比方说如果一个类只会在程序集内使用，那么定义成internal就足够了。参考：[msdn](http://msdn.microsoft.com/en-us/library/kktasw36.aspx)

 21. 在需要保持解耦的地方使用接口，有些设计模式的出现也是由于接口的使用。参考：[msdn](http://msdn.microsoft.com/en-IN/library/3b5b8ezk%28v=vs.100%29.aspx)

 22. 按照用法和需要将类定义为sealed、static或abstract。参考：[msdn](http://msdn.microsoft.com/en-us/library/ms173150%28v=vs.100%29.aspx)

 23. 如果需要多次串联，请使用Stringbuilder代替string，这可以节省堆内存。

 24. 检查是否有不可能执行的代码，如果有，请修改。

 25. 在每个方法前注释，说明它的用法、输入类型和返回值类型信息。

 26. 使用类似Silverlight Spy的工具，检查和操控Silverlight应用在运行时对XMAL的渲染，以此来改善效率。这可以在设计执行XAML时，节省大量退回和来回修改的时间。

 27. 使用filddler工具通过检查HTTP/网络流量和带宽，来跟踪web应用和服务的性能。

 28. 如果你想确认Visual Studio以外的方法，请使用WCFTestClient.exe工具，或者装载它的进程到Visual Studio来进行调试。

 29. 在任何合理的地方使用constants和readonly。参考：/[msdn](http://msdn.microsoft.com/en-us/library/acdd6hb7%28v=vs.100%29.aspx)、[msdn](http://msdn.microsoft.com/en-us/library/e6w8fe1b%28v=vs.100%29.aspx)

 30. 尽量避免强制转换和类型转换，因为会造成性能损失。参考：[msdn](http://msdn.microsoft.com/en-us/library/ms173105.aspx)

 31. 对于你想提供自定义信息的类，请重载ToString(来自Object类)。参考：[msdn](http://msdn.microsoft.com/en-us/library/ms173154%28v=vs.100%29.aspx)

 32. 避免直接从其他代码中ctrl+c/ctrl+v。一直建议还是自己用手敲，即使你已经找到相关代码。这样可以锻炼自己写代码能力，还能正确理解那段代码的用法。最终你永远都不会忘记那段代码。

 33. 保持阅读书籍和文章的良好习惯，遵循大神们的实践指导。（比如微软专家和一些著名的专家，Martin Fowler, Kent Beck, Jeffrey Ritcher, Ward Cunningham, Scott Hanselman, Scott Guthrie, Donald E Knuth.）

 34. 确认代码是否有内存泄漏。如果有，请确保已修正。参考：[blogs.msdn.com](http://blogs.msdn.com/b/davidklinems/archive/2005/11/16/493580.aspx)

 35. 尽可能参加专家们组织的技术研讨会，可以接触到最新的软件趋势、技术和最佳实践

 36. 要透彻理解OOP概念，并尽可能在代码里实现。

 37. 知道项目设计架构，可以从整体上理解程序的执行流程。

 38. 采取必要措施阻止避免任何交叉脚本攻击、SQL注入和其他安全漏洞。

 39. 永远记得将保密和敏感信息加密(通过使用好的加密算法)，比如保存到数据库的密码和保存在web.config文件中的连接字符，要避免被非认证的用户操纵。

 40. 避免对已知类型(原始类型)使用默认关键字，比如int, decimal, bool等。多数情况下，如果不确定是值类型还是引用类型，就使用泛型类型(T)。参考：[msdn](http://msdn.microsoft.com/en-us/library/xwth0h0d%28v=vs.100%29.aspx)

 41. 微软（在代码分析条例和指导中）并不推荐使用’out’和’ref’，这些关键字是通过引用传参，请注意，’ref’参数在传入被调用方法之前，应当在调用方法中先初始化，但’out’参数就不是这样。参考：[msdn](http://msdn.microsoft.com/en-us/library/ms182131%28v=vs.100%29.aspx)

 原文链接： [Mohammed Hameed](https://www.codeproject.com/Articles/593751/Code-Review-Checklist-and-Guidelines-for-Csharp-De) 翻译：伯乐在线 - 伯乐在线读者

 译文链接： [http://blog.jobbole.com/46255/](http://blog.jobbole.com/46255/)

   
  