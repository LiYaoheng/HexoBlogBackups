---
title: Sql语句查询慢问题排查和优化
date: 2018-10-15 21:05:26
tags: mysql
---

Sql语句查询慢原因排查和优化方法，及原理总结

> 1.原因排查

命令:explain sql 

查询分析结果的说明：[可以看这篇文章](http://www.cnblogs.com/xuanzhi201111/p/4175635.html)

命令:


1.set profiling = 1;	//开启操作记录分析
2.show profiles;	//查看最近所有的执行sql统计，主要是操作耗费的时长
3.show profile [params...] [for query id]	//查询id对应的sql的详细过程耗时记录



> [实战:sending data 导致查询很慢的问题分析和解决](https://blog.csdn.net/yunhua_lee/article/details/8573621)


