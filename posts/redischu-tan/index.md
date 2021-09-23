<!--
.. title: redis初探
.. slug: redischu-tan
.. date: 2021-09-23 22:36:52 UTC+08:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

## redis简介

- redis是单进程, 单线程, 所以具有很好的同步能力
- redis所有操作都是原子性的, 单个操作是原子性的, 多个操作也支持事务(通过MULTI和EXEC指令), 乍一看redis支持的数据类型也不特殊, 而其灵活性就来源于redis的操作原子性, 借助这个特性我们可以轻易实现分布式锁


## redis支持的数据类型

- string: 最基础的数据类型, 二进制安全字符串, 存储图片等二进制格式需要存储base64编码
- hash: 哈希
- list: 列表
- set: 集合
- zset(sorted set): 有序集合




