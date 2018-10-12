---
title: '@Controller+@ExceptionHandle全局处理controller的Exception'
date: 2018-10-12 19:06:28
tags: spring
---

使用@Controller + @ExceptionHandle进行controller层的全局异常处理：

推荐[csdn这篇博客](https://blog.csdn.net/kinginblue/article/details/70186586)，写的很不错。

对于与数据库相关的 Spring MVC 项目，我们通常会把 事务 配置在 Service层，当数据库操作失败时让 Service 层抛出运行时异常，Spring 事物管理器就会进行回滚。

如此一来，我们的 Controller 层就不得不进行 try-catch Service 层的异常，否则会返回一些不友好的错误信息到客户端。但是，Controller 层每个方法体都写一些模板化的 try-catch 的代码，很难看也难维护，特别是还需要对 Service 层的不同异常进行不同处理的时候。
使用 @ControllerAdvice + @ExceptionHandler 进行全局的 Controller 层异常处理，只要设计得当，就再也不用在 Controller 层进行 try-catch 了！而且，@Validated 校验器注解的异常，也可以一起处理，无需手动判断绑定校验结果。

优点：将 Controller 层的异常和数据校验的异常进行统一处理，减少模板代码，减少编码量，提升扩展性和可维护性。
缺点：只能处理 Controller 层未捕获（往外抛）的异常，对于 Interceptor（拦截器）层的异常，Spring 框架层的异常，就无能为力了。

被 [@ExceptionHandler](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/ExceptionHandler.html) 注解的方法还可以声明很多参数，详见[文档](http://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/ExceptionHandler.html)。
