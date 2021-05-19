---
title: spring in action 读书笔记 第五章 构建Spring Web应用程序
date: 2021-05-19 21:28:03
tags:
  - java
  - spring
  - spring in action
categories:
  - 读书笔记
  - java
---
### 概述
这章就是简单的介绍一下最基础的mvc程序怎么搞
### 搭建 spring mvc
首先配置DispatcherServlet，这是个前端控制器。传统法子是配置在web.xml文件里，但是spring3.1后，直接用java配置也行。
![配置DispatcherServlet](/images/init-dispatcher-servlet.png "配置DispatcherServlet")
容器会在类路径中自动找实现了javax. servlet. ServletContainerinitialize接口的类，拿它来配置servlet容器
spring搞了实现，叫SpringServletContainerInitializer，这个类会反过来找实现了WebApplicationInitializer的类，把配置交给他们。
spring3.2搞了个后者的实现，就是AbstractAnnotationConfigDispatcherServletInitializer。
getServletMappings会映射到DispatcherServlet。
getServletConfigClasses会返回带@Configuration的类来定义DispatcherServlet应用上下文中的bean
getRootConfigClasses会返回带@Configuration的类来配置ContextLoaderListener（加载应用中其他bean，通常是驱动应用后端的中间层和数据层组件）创建的应用上下文的bean
## 启用spring mvc
我们在配置类里搞个@EnableWebMvc注解。
![webConfiguration的简单配置](/images/web-configuration.png "webConfiguration的简单配置")
底下那个方法是未来让静态资源转发到spring的默认servlet上，而不是DispatcherServlet直接处理
## 控制器
控制器只是方法上加个@RequestMapping注解，类上加个@Controller的类。
@RequestMapping，定义value来指明处理啥东西，method来指明处理什么方法get，post这种。
哦，@Controller是基于@Component的，所以你就不用加这个了。
