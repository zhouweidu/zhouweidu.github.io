---
title: JavaWeb教程
date: 2022-09-06 19:10:08
tags:
- JavaWeb
- 后端
categories:
- 后端
---

# 1. JavaScript

# 2. XML

## 2.1 XML的概念：可扩展的标记语言

## 2.2 XML包含三个部分

- XML声明，必须有而且声明这一行代码必须在XML文件的第一行
- DTD(document type defination) 文档类型定义
- XML正文

# 3. MVC

## 3.1 MVC：Model（模型）、View（视图）、Controller（控制器）

- 视图层：用于做数据展示以及用户交互的界面

- 控制层：能够接受客户端的请求，具体的业务功能需要借助于模型组件来完成

- 模型层：模型分为很多种，有比较简单的pojo/vo(value object)（数据库的一条记录所对应的对象） ，有业务模型组件，有数据访问组件

  - pojo/vo：值对象
  - DAO：数据访问对象
  - BO(business object)：业务对象

- 区分业务对象和数据访问对象

  （1）DAO中的方法都是单精度方法或者细粒度方法，一个方法只考虑一个操作，比如添加就是insert操作，查询就是select操作等等。

  （2）BO中的方法属于业务方法，而实际的业务是比较复杂的，因此业务方法的粒度比较粗
  
  <!--more-->

# 4. IOC

## 4.1 耦合/依赖

依赖指的是某某某离不开某某某，在软件系统中，层与层之间是存在依赖的，也称之为耦合，系统架构设计的原则是**高内聚低耦合**，层内部的组成应该是高度耦合的，而层与层之间的关系应该是低耦合的，最理想的情况是零耦合（就是没用耦合）

## 4.2 IOC-控制反转/DI-依赖注入

### 4.2.1 控制反转

（1）之前在Servlet中，我们创建Service对象，`FruitService fruitService=new FruitServiceImpl();`，这句代码如果出现在Servlet中的某个方法内部，那么这个fruitService的作用域就是这个方法级别，如果这句话出现在Servlet类中（即作为成员变量），那么这个fruitService的作用域就是这个Servlet的实例级别。

（2）之后我们在applicationContext.xml中定义了这个fruitService，然后通过解析XML产生fruitService实例，存放在beanMap中，这个beanMap在一个BeanFactory中，因此我们改变了之前的Service实例，DAO实例等等的生命周期。也就意味着控制权从程序员转移到BeanFactory（BeanFactory又称为IOC容器）。这个现象我们称之为控制反转。

### 4.2.2 依赖注入

（1）之前我们在控制层出现代码`FruitService fruitService=new FruitServiceImpl();`，那么，控制层和service层存在耦合。

（2）之后，我们将代码修改成`FruitService fruitService=null;`，然后在配置文件中配置：

```Xml
<bean id="fruit" class="fruit.controller.FruitController">
    <property name="fruitService" ref="fruitService"/>
</bean>
```

这个配置文件表明FruitController需要fruitService，IOC容器在解析配置文件时，会去寻找Service实例，通过反射注入到Controller中，以前是程序员主动去获取绑定的，现在靠配置文件解析，容器帮我们注入。

# 5. 过滤器

**过滤器链**

- 如果采取的是注解的方式配置过滤器，那么过滤器链的拦截顺序是按照全类名（字母）的先后顺序进行排序的。
- 如果采取xml的方式进行配置，那么按照配置的先后顺序进行排序

# 6. 事务管理

ThreadLocal称之为本地线程，用于同一个线程中的数据通信，我们可以通过set(obj)方法在当前线程上存储数据，通过get()方法在当前线程上获取数据

# 7. 监听器

