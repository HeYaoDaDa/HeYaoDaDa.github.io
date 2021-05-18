---
title: spring in action 读书笔记 第三章 高级装配
date: 2021-05-16 17:24:30
tags:
  - java
  - spring
  - spring in action
categories:
  - 读书笔记
  - java
---
### 简介
这章的内容就是一些高级的bean技术。
- profile
- 条件化的bean声明（@Conditional profile也改成了使用这个技术
- 消除自动装配的歧义
- bean的作用域
- spring表达语言
### profile
profile主要用来设置不同的环境，需要什么bean，java中用@Profile（类和方法级别），xml中在&lt;beans&gt;设置profile属性，&lt;beans&gt;可以嵌套定义，实现多个profile bean放一个xml文件里。
激活profile，需要设置spring.profile.active和spring.profile.default（会优先使用active），有好几种法子：
- DispatcherServlet的初始化参数（在web应用的web.xml里设置
- web应用的上下文参数
- JNDI条目
- 环境变量
- JVM的系统属性
- 在集成测试类上，用@ActiveProfiles注解设置
### 条件化的bean声明（@Conditional）
使用@Conditional，参数设置一个实现了Condition接口的类，这接口里覆写matches方法，方法里有两个参数，ConditionContext和AnnotatedTypeMetadata
### 消除自动装配的歧义
对bean或component使用@Primary标记首选，xml设置bean的primary属性
使用限定符，@Qualifier（bean上，和要装配的地方都要用这个标签），因为java不能在同一个地方放两个同类型的注解（后面还补充了，java8其实就可以，但是需要@Repeatable，当时spring没有，不知道现在能不能），所以，可能还要自己定义自定义注解
### bean的作用域
- 单例（默认）
- 原型
- 会话
- 请求
设置作用域使用@Scope，xml的话，设置bean的scope属性
使用会话和请求作用域的时候，还得设置 proxymode，设置了这个，用来解决这些bean注入到单例bean的问题。
xml要设置proxyMode得用spring aop空间的一个新元素scoped-proxy
### spring表达式语言前
通过外部文件来初始化属性，最简单是用spring的environment，使用env.getProperty方法
这方法有4个重载，2个是返回string，2个是返回指定类型。
然后还有属性占位符的法子，主要用在xml里？“${....}”的格式，使用自动装配的话，需要用@value注解在变量上，为了用占位符还得搞个PropertyPlaceholderConfigurer或PropertySourcesPlaceholderConfigurer（推荐用这个）。
xml要用context空间的&lt;context:propertyplaceholder&gt;元素，
### spring表达语言
- 使用bean的id来引用bean
- 调用方法和访问对象的属性
- 对值进行运算
- 正则
- 集合
这个个表达式得放到#{...}里，要访问类作用域的方法和常量的话，得用T()，返回的结果是一个class对象。
