---
title: spring in action 读书笔记 第四章 面向切面的Spring
date: 2021-05-18 19:37:56
tags:
  - java
  - spring
  - spring in action
categories:
  - 读书笔记
  - java
---
### AOP术语 
DI：对象之间的解耦；AOP：横切关注点与它们所影响的对象之间的解耦
通知（advice），切点（pointcut），连接点（join point）
通知：包含了需要用于多个应用对象的横切行为，定义了切面是什么时候使用，描述切面的工作，什么时候执行，在哪执行
五种通知
- 前置通知
- 后置通知
- 返回通知
- 异常通知
- 环绕通知
连接点
连接点程序执行过程中能够应用通知的所有点，在程序中应用通知的时机就是连接点，连接点是在应用执行过程中能够插入切面的一个点
切点
切点定义了通知被应用的具体位置，切点定义了“何处”，会匹配通知要织入的一个或多个连接点。
切面
切面是通知和切点的集合
引入
引入让我们向现有的类中添加新方法和属性
织入
把切面应用到目标对象并创建新的代理对象的过程。
可以在：编译期，类加载期，运行期进行
### spring提供了4种AOP支持
- 基于代理的经典spring AOP
- 纯POJO切面
- @AspectJ注解驱动的切面
- 注入式AspectJ切面（适用于Spring各版本）
前三是Spring AOP实现的变种，Spring AOP构建在动态代理基础上，所以Spring对AOP的支持只有方法拦截
spring的通知是用java编写的，或在spring配置文件里用xml来编写
### 编写切点
![aspectJ指示器](/images/spring-aspectJ-list.png "aspectJ指示器")
只有execution是实际执行匹配的，其他都是限制匹配
![切点定义格式](/images/execution-format.png "切点定义格式")
这个表达式还支持&& || ！。
### 定义切面
反正就是是搞个bean，打上@Aspect注解，和在方法上声明通知，配置类里要记得启用aspectJ代理，xml里用aop命名空间。
切面方法里可以搞参数的，但是要在切点里设置好args。
### 引入新功能
用@DeclareParents注解就完事，value写哪个bean要引入接口，defaultImpl写引入功能的实现类，@DeclareParents注解的变量类型就是要引入的接口。
### 注入aspectJ切面
我们要用spring为aspectJ依赖注入，但要记得在bean标签里特别设置factory-method属性为aspectOf，因为AspectJ切面由AspectJ在运行期间创建，所以只能用aspectJ给出的静态方法。
