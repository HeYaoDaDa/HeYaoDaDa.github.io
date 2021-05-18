---
title: spring in action 读书笔记 第二章 装配Bean
date: 2021-05-17 19:54:30
tags:
  - java
  - spring
  - spring in action
categories:
  - 读书笔记
  - java
---
这章的主要内容就是三种bean装配（DI）的方法。
推荐方法：自动装配 &gt; java显式配置 &gt; xml显式配置
### 自动装配
自动装配默认是关的。自动装配之前需要组件扫描，要打开的话，还是需要先搞一个显式配置的文件（java和xml都行），使用ComponentScan。默认会扫描配置文件所在的包，你也可以自己指定扫描包列表和类列表
然后需要组件扫描的bean在定义的时候，在类上面加个注解@Component。要给bean改名直接给@Component一个字符串参数就完事。
给需要自动装配的方法（构造方法，set方法啥的）上面加一个@Autowired注解。
注：@Componen，@Autowired和 @Named，@Inject大部分场景一样（什么时候不一样我也不知道）
### java装配
在配置类里，写名字和bean类名一样的公开方法，上面要一个@Bean注解，要装配的话，可以直接调用需要装配进来的bean的公开方法（只能在一个配置类里），比较推荐的法子，是写一个形参，让容器自己找对应的bean。
### xml装配
这个东西最烦了，好长，定义bean就是&lt;bean&gt;标签，注入就是用&lt;constructor-arg&gt;和&lt;property&gt;两个标签，或者有短的&lt;c&gt;和&lt;p&gt;，不过搞之前需要先在前面写上他们的命名空间，而且要弄数组啥的容器类，还得用&lt;util&gt;命名空间。
### 混用
java中用@Import可以引用其他java配置，用@ImportResource可以引用xml配置。xml就是&lt;import&gt;导入xml配置，然后得用&lt;bean&gt;来导入java配置，也是离谱。