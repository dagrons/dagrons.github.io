<!--
.. title: 恶意代码分析基础-第一天
.. slug: e-yi-dai-ma-fen-xi-ji-chu-di-yi-tian
.. date: 2021-08-26 21:01:29 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

# 恶意代码分析基础-第一天

PE分析工具：PEID, ExecInfo

[PE结构详解 | Thunder_J (thunderjie.github.io)](https://thunderjie.github.io/2019/03/27/PE结构详解/) 

[PE结构分析 | 输入表结构和输入地址表(IAT)](https://www.cnblogs.com/night-ride-depart/p/5776107.html)
> HNT和IAT内容相同, 但HNT被载入内存后不再改变, IAT会被覆写从而直接指向API函数地址

[IAT导入函数地址表](https://blog.csdn.net/Cody_Ren/article/details/77815952) 
> 用实例讲解, 有IAT解析流程


脱壳需要做的两件事: 修复IAT表, 覆写OEP, dump

**为什么要有IAT修复?**
> 程序在脱壳完成进入OEP的时候, IAT已经被覆写了,
> 为了让dump出的程序正常运行, 我们需要将IAT进行修复
> 从INT重建IAT不靠谱是因为在脱壳过程中, 完全可以毁掉INT, 而原始加壳文件payload被加密, 无法确定INT的位址和内容


