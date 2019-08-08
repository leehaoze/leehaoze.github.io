---
title: Spring面试题
date: 2018-04-08 11:15:24
tags: 面试题
---

# Spring面试题

## 什么是Spring
- Spring是一个开源的Java EE开发框架。
- Spring框架的目标是使得Java EE应用程序的开发更简洁，通过使用POJO为基础的编程模式促进良好的编程风格。

## Spring工作原理
- spring mvc请所有的请求都提交给DispatcherServlet,它会委托应用系统的其他模块负责负责对请求进行真正的处理工作。
- DispatcherServlet查询一个或多个HandlerMapping,找到处理请求的Controller. 
- DispatcherServlet请请求提交到目标Controller 
- Controller进行业务逻辑处理后，会返回一个ModelAndView 
- Dispathcher查询一个或多个ViewResolver视图解析器,找到ModelAndView对象指定的视图对象 
- 视图对象负责渲染返回给客户端。 

## Spring的优点
- **轻量级**：Spring在大小和透明性方面绝对属于轻量级的，基础版本的Spring框架大约只有2MB。
- **控制反转**：（IOC）Spring使用控制反转技术实现了松耦合。依赖注入到对象，而不是创建或寻找依赖对象。
- **面向切面编程（AOP）**：Spring支持面向切面编程，同时把应用的业务逻辑与系统的服务分离开来。
- 容器：Spring包含并管理应用程序对象的配置及生命周期。
- MVC框架：Spring的web框架是一个设计优良改的web MVC 框架，很好的取代了一些web框架。
- 事务管理：Spring对下至本地业务上至全局业务(JAT)提供了统一的事务管理接口。
- 异常处理：Spring提供一个方便的API将特定技术的异常(由JDBC, Hibernate, 或JDO抛出)转化为一致的、Unchecked异常。
- 
## Spring的几种配置方式
- 基于XML的配置
- 基于注解的配置
- 基于Java的配置


## Spring的IOC容器
**<p align = left> Spring IOC容器是什么？</p>**
  1. 负责创建对象，管理对象（通过依赖注入），整合对象，配置对象以及管理这些对象的生命周期。<br />

**<p align = left> 使用IOC容器的好处？</p>**
  1. 将**耦合代码**放到统一的XML文件中管理；<br />
  2. 通过**配置文件**来管理对象的生命周期、依赖关系等，不用重新修改并编译具体的代码，实现组件之间的解耦。<br />

**<p align = left>Spring中的依赖注入是什么？</p>**
  1. 不用创建对象，只用描述它如何被创建。不用在代码里直接组装组件和服务，只用在配置文件里描述哪些组件需要哪些服务，之后用一个容器（IOC容器）把他们组装起来。
**<p align = left>Spring支持三种依赖注入方式：</p>**
  1. 属性（Setter方法）注入 <br />
  2. 构造注入<br />
  3. 接口注入<br />

## AOP
**<p align = left> 什么是AOP？</p>**
- 意为：通过预编译方式和动态代理，实现程序功能的统一维护的一种技术。<br />
- 提供另外一种角度来思考程序结构，通过这种方式弥补了面向对象编程（OOP）的不足。<br />
- 主要意图：将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码。<br />

**<p align = left> AOP有什么作用？</p>**
- 除了类（classes）以外，AOP提供了切面。切面对关注点进行模块化，例如横切多个类型和对象的事务管理<br />
- Spring的一个关键的组件就是AOP框架，可以自由选择是否使用AOP<br />
- 提供声明式企业服务，特别是为了替代EJB声明式服务。最重要的服务是声明性事务管理，这个服务建立在Spring的抽象事物管理之上<br />
- 允许用户实现自定义切面，用AOP来完善OOP的使用<br />
- 可以把Spring AOP看作是对Spring的一种增强<br />


## Spring核心组件
**<p align = left>Spring核心组件</p>**
**Bean**（最核心）、Context、Core  

**<p align = left> Bean组件</p>**
功能：Bean的定义、Bean 的创建以及对Bean的解析

**<p align = left> Context组件</p>**
1.Context作为Spring的Ioc容器，基本上整合了Spring的大部分功能。<br />
2.**功能：** 实际上就是给Spring提供一个运行时的环境，用以保存各个对象的状态。<br />
3.**任务：** 1.标识一个应用环境 2.利用BeanFactory创建Bean对象 3.保存对象关系表  4.能够捕获各种事件

**<p align = left> Core组件</p>**
1.Core包含了很多的关键类，其中一个重要组成部分Resource就是定义了资源的访问方式<br />
2.Resource接口封装了各种可能的资源类型，它屏蔽了文件类型的不同、屏蔽了资源加载者的差异，只需要实现这个接口就可以加载所有的资源， 他的默认实现是DefaultResourceLoader。<br />


## 什么是SSH
**<p align = left> SSH：</p>**
Struts（表示层）+Spring（业务层）+Hibernate（持久层）
**<p align = left> Struts：</p>**
Struts是一个表示层框架，主要作用是界面展示，接收请求，分发请求。
在MVC框架中，Struts属于VC层次，负责界面表现，负责MVC关系的分发。（View：沿用JSP，HTTP，Form，Tag，Resourse ；Controller：ActionServlet，struts-config.xml，Action）
**<p align = left> Hibernate：</p>**
Hibernate是一个持久层框架，它只负责与关系数据库的操作。
**<p align = left> Spring：</p>**
Spring是一个业务层框架，是一个整合的框架，能够很好地黏合表示层与持久层。

## Spring中有哪些核心类，各有什么作用
- BeanFactory：产生一个新的实例，可以实现单例模式
- BeanWrapper：提供统一的get及set方法
- ApplicationContext:提供框架的实现，包括BeanFactory的所有功能


-----

以下请了解

## 什么是Spring Beans？
- Spring beans 是那些形成Spring应用的主干的java对象。它们被Spring IOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中“<“bean/“> ”的形式定义。

- Spring 框架定义的beans都是单件beans。在bean tag中有个属性”singleton”，如果它被赋为TRUE，bean 就是单件，否则就是一个 prototype bean。默认是TRUE，所以所有在Spring框架中的beans 缺省都是单件。

## Spring Bean 的作用域
- singleton：在Spring IOC容器中仅存在一个Bean实例，Bean以单实例的方式存在。
- prototype：一个bean可以定义多个实例。
- request：每次HTTP请求都会创建一个新的Bean。该作用域仅适用于WebApplicationContext环境。
- session：一个HTTP Session定义一个Bean。该作用域仅适用于WebApplicationContext环境.
- globalSession：同一个全局HTTP Session定义一个Bean。该作用域同样仅适用于WebApplicationContext环境.

## Spring中的单例bean是线程安全的吗
不是

##  解释Spring框架中bean的生命周期。
- Spring容器 从XML 文件中读取bean的定义，并实例化bean。
- Spring根据bean的定义填充所有的属性。
- 如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
- 如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
- 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
- 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
- 如果有BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用。
- 如果bean实现了 DisposableBean，它将调用destroy()方法。

## 什么是bean装配
装配，或bean 装配是指在Spring 容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。

## 什么是基于Java的Spring注解配置？给一些注解的例子
基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。

以@Configuration 注解为例，它用来标记类可以当做一个bean的定义，被Spring IOC容器使用。另一个例子是@Bean注解，它表示此方法将要返回一个对象，作为一个bean注册进Spring应用上下文。

## @Required  注解
这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。

## @Autowired 注解
@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。

## @Qualifier 注解
当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的bean。






