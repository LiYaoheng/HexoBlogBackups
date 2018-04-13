---
title: '[转载]HashMap,LinkedHashMap,TreeMap的有序性'
date: 2018-04-13 19:15:30
tags: Java
---

> [原文地址](https://blog.csdn.net/zhuhao717/article/details/47444763)

总结：

+ ``HashMap完全无序的，不能保证任何的顺序``
+ ``LinkedHashMap是内部增加链表的方式保证顺序，顺序取决于元素添加或访问顺序``
+ ``TreeMap实现了SortedMap接口，元素顺序取决于Comparator的排序方法(Key值)``

