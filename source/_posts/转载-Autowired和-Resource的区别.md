---
title: '[转载]@Autowired和@Resource的区别'
date: 2018-04-12 11:09:38
tags: Java
---

+ [原文链接](https://www.zhihu.com/question/39356740)
+ [另一篇文章](https://blog.csdn.net/wangzuojia001/article/details/54312074)


----

@Autowired注解是按类型装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许null值，可以设置它required属性为false。

@Resource注解和@Autowired一样，也可以标注在字段或属性的setter方法上，但它默认按名称装配。名称可以通过@Resource的name属性指定，如果没有指定name属性，当注解标注在字段上，即默认取字段的名称作为bean名称寻找依赖对象，当注解标注在属性的setter方法上，即默认取属性名作为bean名称寻找依赖对象。 

@Resources按名字，是jdk的，@Autowired按类型，是spring的。
