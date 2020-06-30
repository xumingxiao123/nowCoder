## 第一章  牛客网论坛项目

#### 1. 技术架构

* spring boot

  起步依赖、自动配置、端点监控

* spring、spring MVC、Mybatis

* redis、kaflka、elasticsearch

* spring security、spring actuator

#### 2. 开发工具

* 构建工具：Apache Maven https://maven.apache.org

  可以帮助我们构建项目、管理项目中的jar包

* 集成开发工具：IntelliJ IDEA  https://start.spring.io

* 数据库：MySQL、Redis

* 应用服务器： Apache Tomcat

* 版本控制工具：Git

> 传统Maven项目和基于spring bootMaven项目二者结构一致，区别如下：传统项目如果需要打成war包，需要在WEB-INF目录结构配置web.xml文件；springboot则不需要。

#### 3. Spring 入门

www.spring.io

**1. Spring全家桶**

* Spring framework
* Spring boot: 构建一切
* Spring cloud：微服务，协调一切
* Spring cloud data flow ：连接一切

**2. Spring、Spring Framework、Spring Boot、Spring Cloud的区别**

- **Spring**

**Spring**是一个生态体系（也可以说是技术体系），是集大成者，它包含了Spring Framework、Spring Boot、Spring Cloud等（还包括Spring Cloud data flow、spring data、spring integration、spring batch、spring security、spring hateoas），可以参考链接：https://spring.io/projects

- **Spring Framework**

**Spring Framework**是整个spring生态的**基石**，它可是硬生生的消灭了Java官方主推的企业级开发标准EJB，从而实现一统天下。Spring官方对Spring Framework简短描述：为依赖注入、事务管理、WEB应用、数据访问等提供了核心的支持。Spring Framework专注于企业级应用程序的“管道”，以便开发团队可以专注于应用程序的业务逻辑。

笔者要提醒的是，千万不要把Spring和Spring Framework搞混淆了，很多文章都错误的定义了spring：**spring是一个一站式的轻量级的java开发框架，核心是控制反转（IoC）和面向切面（AOP）**，针对于开发的WEB层(springMVC)、业务层(IoC)、持久层(jdbcTemplate)等都提供了多种配置解决方案。这是Spring Framework的定义，**至于Spring，是整个生态**。

但是，无论Spring Framework接口如何简化，设计如何优美，始终无法摆脱**被动**的境况：由于它自身并非容器，所以基本上不得不随JavaEE容器启动而装载，例如Tomcat、Jetty、JBoss等。然而Spring Boot的出现，改变了Spring Framework甚至整个Spring技术体系的现状（摘自小马哥的《SpringBoot编程思想》）。

- **Spring Boot**

Spring Boot这家伙简直就是对Java企业级应用开发进行了一场浩浩荡荡的革命。如果稍微有几年工作经验的老油条，应该都记得以前的Java Web开发模式：**Tomcat + WAR包**。WEB项目基于spring framework，项目目录一定要是标准的WEB-INF + classes + lib，而且大量的xml配置。如果说，以前搭建一个SSH架构的Web项目需要1个小时，那么现在应该10分钟就可以了。

Spring Boot能够让你非常容易的创建一个单机版本、生产级别的基于spring framework的应用。然后，"just run"即可。Spring Boot默认集成了很多第三方包，以便你能以最小的代价开始一个项目。

我们看看官方对Spring Boot的定义：

> Spring Boot is designed to get you up and running as quickly as possible, with minimal upfront configuration of Spring. Spring Boot takes an opinionated view of building production-ready applications.

即Spring Boot为快速启动且最小化配置的spring应用而设计，并且它具有用于构建生产级别应用的一套固化的视图（摘自小马哥的《SpringBoot编程思想》）。这里的固化的视图，笔者认为可以理解成Spring Boot的约定，因为Spring Boot的设计是约定大于实现的。

- **Spring Cloud**

最后就是大名鼎鼎的Spring Cloud了，Spring Cloud事实上是一整套**基于Spring Boot的微服务解决方案**。它为开发者提供了很多工具，用于快速构建分布式系统的一些通用模式，例如：配置管理、注册中心、服务发现、限流、网关、链路追踪等。

Spring Boot是build anything，而Spring Cloud是coordinate anything，Spring Cloud的每一个微服务解决方案都是基于Spring Boot构建的。

> **Spring Boot是大势所趋**，而且它就像当年Spring Framework干掉EJB一样，干掉WEB容器+WAR的开发模式，统一现在的Java企业级应用开发标准。至于Spring Cloud？请**谨慎**选择每一个引入项目的组件，毕竟它的每一个微服务组件都面对很多优秀的开源可替代方案。

#### **4. Spring核心：思想**

https://www.zhihu.com/question/23277575

![image-20200526203503170](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200526203503170.png)

 **Spring ioc：**

思想为**控制反转**，实现方式为**依赖注入**，基于**ioc容器**。

**优势**：原来每次方法调用，都需要new一个对象。使a和b之间产生了耦合。**ioc**降低了项目的耦合度。

> 将类与类之间的依赖从代码中脱离出来
>
> 我们可以通过反射来解耦，**反射可以根据类的全限定名在程序运行时创建对象**，可以这样做，将类的全限定名配置在xml文件中，在程序运行时通过反射读取该类的全限定名，动态的创建对象，赋值给userDao接口userDaoImpl.这样做后UserServiceImpl和UserDaoImpl之间没有了直接的关系，当我们需要替换UserDaoImpl对象的时候只需要在配置文件中去修改类的全限定名就可以了，非常的灵活方便,ioc容器的实现就是这个原理。

**Spring aop：**

面向切面编程

优势：代码复用

**深入理解：**

**控制反转（**IOC**）**就是Spring中两个主要的概念之一，另外一个就是**AOP**（面向切面），它的主要思路应用动态代理，这里不详细展开。

下面通过一个简单的例子来介绍一下IOC。

**假设一个场景：**目前有三个角色，买水果的人（用户），卖水果的人（业务层），水果（持久化层），

![img](https://pic4.zhimg.com/50/v2-00e41ad1cf13fa3ef0cc5e4f990a239c_hd.jpg)![img](https://pic4.zhimg.com/80/v2-00e41ad1cf13fa3ef0cc5e4f990a239c_720w.jpg)

 先写一个接口，

```text
public interface Fruit {
    public void get();
}
```

现在实现3种水果的类，为了方便展示，把它们先写在一起，

```text
// Apple.java
public class Apple implements Fruit{
    public void get() {
        System.out.println("get an apple");
    }
}

// Orange.java
public class Banana implements Fruit{
    public void get() {
        System.out.println("get a banana");
    }
}

// Banana.java
public class Orange implements Fruit{
    public void get() {
        System.out.println("get a organe");
    }
}
```

现在实现一个业务层，也就是从3个水果类中获取水果，

```text
// UserService.java
public class UserService {
    private Fruit fruit = new Apple();
    public void getFruit() {
        fruit.get();
    }
}
```

然后，实现一个用户类，

```text
// User.java
public class User {
    public static void main(String[] args) {
        UserService user = new UserService();
        user.getFruit();
    }
}
```

上述就是我们实现一个程序的惯用方式，这样看上去没有什么问题，目前我们调用业务层UserService获取到**苹果**，那么试想一下，**如果现在我想获取橘子怎么办？**这样就需要修改业务层代码，

```text
// UserService.java
public class UserService {
    private Fruit fruit = new Orange();
    public void getFruit() {
        fruit.get();
    }
}
```

也许很多同学会认为这样没什么问题，那就修改一下业务层代码啊？

显然，这不是一个优秀的程序员做的事情，**每当用户需求做出改变时，我们的代码都要做出相应的修改**，那么有两个问题，

- 如果工程量较大，修改的内容较多怎么办？
- 如果我们修改代码对其他业务造成影响怎么办？

所以，一个好的设计思路就应该在不改变原代码的基础上实现我们想要的功能。

那么，接下来就应该转变思维，考虑一下，目前的**控制权**在业务层，所以每次用户需求改变时，业务层也要跟着改变，既然这样，我们把控制权交给用户不就行了吗？

下面来修改一下业务层的代码实现控制权的转换，

```text
public class UserService {
    private Fruit fruit;
    public void setFruit(Fruit fruit) {
        this.fruit = fruit;
    }
    public void getFruit() {
        this.fruit.get();
    }
}
```

细心的同学应该可以看得出改变，我在加了一个**set**方法，使得用户层可以**注入**不同的对象，这样我们在用户层传入哪个对象，就会获得哪个结果，

```text
// 1. 获取橘子
public class User {
    public static void main(String[] args) {
        UserService user = new UsUserServiceerImpl();
        user.setFruit(new Orange());    //在这里注入对象
        user.getFruit();
    }
}
//2. 获取香蕉
public class User {
    public static void main(String[] args) {
        UserService user = new UserService();
        user.setFruit(new Banana());
        user.getFruit();
    }
}
```

现在来总结一下，经过改变前后到底发生了什么，  

![img](https://pic3.zhimg.com/50/v2-523bbb35239175af0fb74a6583ffe55a_hd.jpg)![img](https://pic3.zhimg.com/80/v2-523bbb35239175af0fb74a6583ffe55a_720w.jpg)

 上图展示的很明确，就是控制权的反转，之前主动权在业务层，每次用户提出需求业务层就需要跟着做出改变，现在我们把主动权交给了用户，它传进什么，就得到什么样的结果，这样业务代码就不用跟着改变了。

**这就是IOC（控制反转）的核心思想。**

所以，IOC的优点就显而易见了，它降低组件之间的耦合度，实现软件各层之间的解耦，同时在保障不改变源码的情况下实现外部对象动态的注入到组件中，这样就能减少后期维护成本。

#### 5. Spring IOC有什么好处？

前面用一个示例介绍了IOC的来龙去脉，那么这样做到底有什么好处呢？下面就来说一下。

控制反转有很多优点，这里我就举例子介绍其中一点。

一个版本发布至少要经历以下几个过程（以镜像化部署为力），

- 合入代码
- 代码规范和安全检查
- 构建成docker镜像
- 启动任务
- alpha测试
- beta测试
- gamma测试
- ……

试想一下，如果控制权在业务层（程序员手中），那么每次用户需求改变，我们都要经历这样一个繁琐的流程，需要耗费大量的时间和经历。

把控制权反转一下，交出去就不一样了，我们**暴露**出一个配置文件给用户，用户需要做什么改变，他之间修改配置文件即可，代码不需要跟随改动，这样就没必要再走一遍繁琐的版本发布流程。

#### 6. Spring 四大注解

 Spring 四大注解有：@Component，@Controller，@ Service，@ Repository

@Repository：持久层，用于标注数据访问组件，即DAO组件。 
@Service：业务层，用于标注业务逻辑层主键。 
@Controller：控制层，用于标注控制层组件。 
@Component：当你不确定是属于哪一层的时候使用。 
之所以区分开几种类型，一是spring想在以后的版本中为它们添加特殊技能，二是这种分层的做法使web架构更清晰，易读性与维护性更好。

~~~java
package org.springframework.stereotype;

import java.lang.annotation.Documented;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import org.springframework.core.annotation.AliasFor;

@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}

~~~

**1、@Component**
是所有受Spring 管理组件的通用形式，@Component注解可以放在类的头上，@Component不推荐使用。

**2、@Controller**

@Controller对应表现层的Bean，也就是Action，例如：

```
1 @Controller
2 @Scope("prototype")
3 public class UserAction extends BaseAction<User>{
4 ……
5 }
```

使用@Controller注解标识UserAction之后，就表示要把UserAction交给Spring容器管理，在Spring容器中会存在一个名字为"userAction"的action，这个名字是根据UserAction类名来取的。注意：如果**@Controller**不指定其**value【@Controller】，**则默认的bean名字为这个类的类名首字母小写，如果指定**value【@Controller(value="UserAction")】**或者**【@Controller("UserAction")】，**则使用value作为bean的名字。

这里的UserAction还使用了**@Scope**注解，@Scope("prototype")表示将Action的范围声明为原型，可以利用容器的scope="prototype"来保证每一个请求有一个单独的Action来处理，避免struts中Action的线程安全问题。spring默认scope是单例模式(scope="singleton")，这样只会创建一个Action对象，每次访问都是同一Action对象，数据不安全，struts2是要求每次次访问都对应不同的Action，scope="prototype可以保证当有请求的时候都创建一个Action对象。

**3、@ Service** 

@Service对应的是业务层Bean，例如：

```
1 @Service("userService")
2 public class UserServiceImpl implements UserService {
3 ………
4 }
```

@Service("userService"当Action需要使用UserServiceImpl的的实例时,就可以由Spring创建好的"userService"，然后注入给Action：在Action只需要声明一个名字叫“userService”的变量来接收由Spring注入的"userService"即可，具体代码如下：

```
1 // 注入userService
2 @Resource(name = "userService")
3 private UserService userService;
```

注意：在Action声明的“userService”变量的类型必须是“UserServiceImpl”或者是其父类“UserService”，否则由于类型不一致而无法注入，由于Action中的声明的“userService”变量使用了@Resource注解去标注，并且指明了其name = "userService"，这就等于告诉Spring，说我Action要实例化一个“userService”，你Spring快点帮我实例化好，然后给我，当Spring看到userService变量上的@Resource的注解时，根据其指明的name属性可以知道，Action中需要用到一个UserServiceImpl的实例，此时Spring就会把自己创建好的名字叫做"userService"的UserServiceImpl的实例注入给Action中的“userService”变量，帮助Action完成userService的实例化，这样在Action中就不用通过“UserService userService = new UserServiceImpl();”这种最原始的方式去实例化userService了。

如果没有Spring，那么当Action需要使用UserServiceImpl时，必须通过“UserService userService = new UserServiceImpl();”主动去创建实例对象，但使用了Spring之后，Action要使用UserServiceImpl时，就不用主动去创建UserServiceImpl的实例了，创建UserServiceImpl实例已经交给Spring来做了，Spring把创建好的UserServiceImpl实例给Action，Action拿到就可以直接用了。Action由原来的主动创建UserServiceImpl实例后就可以马上使用，变成了被动等待由Spring创建好UserServiceImpl实例之后再注入给Action，Action才能够使用。这说明Action对“UserServiceImpl”类的“控制权”已经被“反转”了，原来主动权在自己手上，自己要使用“UserServiceImpl”类的实例，自己主动去new一个出来马上就可以使用了，但现在自己不能主动去new“UserServiceImpl”类的实例，new“UserServiceImpl”类的实例的权力已经被Spring拿走了，只有Spring才能够new“UserServiceImpl”类的实例，而Action只能等Spring创建好“UserServiceImpl”类的实例后，再“恳求”Spring把创建好的“UserServiceImpl”类的实例给他，这样他才能够使用“UserServiceImpl”，这就是Spring核心思想“**控制反转**”，也叫“**依赖注入**”，“依赖注入”也很好理解，Action需要使用UserServiceImpl干活，那么就是对UserServiceImpl产生了依赖，Spring把Acion需要依赖的UserServiceImpl注入(也就是“给”)给Action，这就是所谓的“依赖注入”。对Action而言，Action依赖什么东西，就请求Spring注入给他，对Spring而言，Action需要什么，Spring就主动注入给他。

4、@ **Repository**

@Repository对应数据访问层Bean ，例如：

```
1 @Repository(value="userDao")
2 public class UserDaoImpl extends BaseDaoImpl<User> {
3 ………
4 }
```

@Repository(value="userDao")注解是告诉Spring，让Spring创建一个名字叫“userDao”的UserDaoImpl实例。

当Service需要使用Spring创建的名字叫“userDao”的UserDaoImpl实例时，就可以使用@Resource(name = "userDao")注解告诉Spring，Spring把创建好的userDao注入给Service即可。

```
1 // 注入userDao，从数据库中根据用户Id取出指定用户时需要用到
2 @Resource(name = "userDao")
3 private BaseDao<User> userDao;
```

#### 7. Spring MVC

三层架构：表现层、业务层、数据访问层

MVC 是 Model、View 和 Controller 的缩写，分别代表 Web 应用程序中的 3 种职责。

- 模型：用于存储数据以及处理用户请求的业务逻辑。
- 视图：向控制器提交数据，显示模型中的数据。
- 控制器：根据视图提出的请求判断将请求和数据交给哪个模型处理，将处理后的有关结果交给哪个视图更新显示。

#### 8. SpringMVC框架理解

https://blog.csdn.net/litianxiang_kaola/article/details/79169148

JavaEE体系结构包括四层，从上到下分别是应用层、Web层、业务层、持久层。Struts和SpringMVC是Web层的框架，Spring是业务层的框架，Hibernate和MyBatis是持久层的框架。

**为什么要使用SpringMVC？**
很多应用程序的问题在于**处理业务数据的对象**和**显示业务数据的视图**之间存在紧密耦合，通常，更新业务对象的命令都是从视图本身发起的，使视图对任何业务对象更改都有高度敏感性。而且，当多个视图依赖于同一个业务对象时是没有灵活性的。

SpringMVC是一种基于Java，实现了Web MVC设计模式，请求驱动类型的轻量级Web框架，即使用了MVC架构模式的思想，将Web层进行职责解耦。基于请求驱动指的就是使用请求-响应模型，框架的目的就是帮助我们**简化开发**，SpringMVC也是要简化我们日常Web开发。

**MVC设计模式**
MVC设计模式的任务是将包含业务数据的模块与显示模块的视图解耦。这是怎样发生的？在模型和视图之间引入重定向层可以解决问题。此重定向层是控制器，控制器将接收请求，执行更新模型的操作，然后通知视图关于模型更改的消息。

**SpringMVC架构**
SpringMVC是Spring的一部分，如图：

SpringMVC的核心架构：

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20190630145911981.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdGlhbnhpYW5nX2thb2xh,size_16,color_FFFFFF,t_70)


具体流程：

（1）首先浏览器发送请求——>DispatcherServlet，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制；

（2）DispatcherServlet——>HandlerMapping，处理器映射器将会把请求映射为HandlerExecutionChain对象（包含一个Handler处理器对象、多个HandlerInterceptor拦截器）对象；

（3）DispatcherServlet——>HandlerAdapter，处理器适配器将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器；

（4）HandlerAdapter——>调用处理器相应功能处理方法，并返回一个ModelAndView对象（包含模型数据、逻辑视图名）；

（5）ModelAndView对象（Model部分是业务对象返回的模型数据，View部分为逻辑视图名）——> ViewResolver， 视图解析器将把逻辑视图名解析为具体的View；

（6）View——>渲染，View会根据传进来的Model模型数据进行渲染，此处的Model实际是一个Map数据结构；

（7）返回控制权给DispatcherServlet，由DispatcherServlet返回响应给用户，到此一个流程结束。



​                                                        <img src="https://img-blog.csdnimg.cn/20200107140858428.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4MDU0MjE5,size_16,color_FFFFFF,t_70" alt="img" style="zoom: 67%;" />

SpringMVC入门程序
（1）web.xml

~~~xml
<web-app>
  <servlet>
  <!-- 加载前端控制器 -->
  <servlet-name>springmvc</servlet-name>
  <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  <!-- 
  	   加载配置文件
  	   默认加载规范：
  	   * 文件命名：servlet-name-servlet.xml====springmvc-servlet.xml
  	   * 路径规范：必须在WEB-INF目录下面
  	   修改加载路径：
   -->
   <init-param>
   <param-name>contextConfigLocation</param-name>
   <param-value>classpath:springmvc.xml</param-value>   
   </init-param>
  </servlet>
  
  <servlet-mapping>
  <servlet-name>springmvc</servlet-name>
  <url-pattern>*.do</url-pattern>
  </servlet-mapping>
</web-app>

~~~

（2）springmvc.xml

~~~xml
<beans>
	<!-- 配置映射处理器：根据bean(自定义Controller)的name属性的url去寻找handler；springmvc默认的映射处理器是
	BeanNameUrlHandlerMapping
	 -->
	<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
	
	
	<!-- 配置处理器适配器来执行Controlelr ,springmvc默认的是
	SimpleControllerHandlerAdapter
	-->
	<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
	
	<!-- 配置自定义Controller -->
	<bean id="myController" name="/hello.do" class="org.controller.MyController"></bean>
	
	<!-- 配置sprigmvc视图解析器：解析逻辑试图； 
		后台返回逻辑试图：index
		视图解析器解析出真正物理视图：前缀+逻辑试图+后缀====/WEB-INF/jsps/index.jsp
	-->
	<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/jsps/"></property>
		<property name="suffix" value=".jsp"></property>		
	</bean>
</beans>
~~~


（3）自定义处理器

```xml
public class MyController implements Controller{

	public ModelAndView handleRequest(HttpServletRequest arg0,
			HttpServletResponse arg1) throws Exception {
		ModelAndView mv = new ModelAndView();
		//设置页面回显数据
		mv.addObject("hello", "欢迎学习springmvc！");
		
		//返回物理视图
		//mv.setViewName("/WEB-INF/jsps/index.jsp");
		
		//返回逻辑视图
		mv.setViewName("index");
		return mv;
	}
}
```

（4）index页面

~~~xml
<html>
<body>
<h1>${hello}</h1>
</body>
</html>
~~~

（5）测试地址

http://localhost:8080/springmvc/hello.do
**HandlerMapping**
处理器映射器将会把请求映射为 HandlerExecutionChain 对象（包含一个 Handler 处理器对象、多个 HandlerInterceptor 拦截器）对象，通过这种策略模式，很容易添加新的映射策略。

处理器映射器有三种，三种可以共存，相互不影响，分别是BeanNameUrlHandlerMapping、SimpleUrlHandlerMapping和ControllerClassNameHandlerMapping；

BeanNameUrlHandlerMapping

默认映射器，即使不配置，默认就使用这个来映射请求。

~~~xml
<bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"></bean>
//映射器把hello.do请求映射到该处理器
<bean id="testController" name="/hello.do" class="org.controller.TestController"></bean>
~~~

impleUrlHandlerMapping

该处理器映射器可以配置多个映射对应一个处理器.

~~~xml
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
	<property name="mappings">
		<props>
			<prop key="/ss.do">testController</prop>
			<prop key="/abc.do">testController</prop>
		</props>
	</property>
</bean>
//上面的这个映射配置表示多个*.do文件可以访问同一个Controller。	
<bean id="testController" name="/hello.do" class="org.controller.TestController"></bean>

~~~


ControllerClassNameHandlerMapping

该处理器映射器可以不用手动配置映射, 通过[类名.do]来访问对应的处理器.

~~~xml
//这个Mapping一配置, 我们就可以使用Controller的 [类名.do]来访问这个Controller.
<bean class="org.springframework.web.servlet.mvc.support.ControllerClassNameHandlerMapping"></bean>

~~~

HandlerAdapter
处理器适配器有两种，可以共存，分别是SimpleControllerHandlerAdapter和HttpRequestHandlerAdapter。

SimpleControllerHandlerAdapter
SimpleControllerHandlerAdapter是默认的适配器，所有实现了org.springframework.web.servlet.mvc.Controller 接口的处理器都是通过此适配器适配, 执行的。

HttpRequestHandlerAdapter
该适配器将http请求封装成HttpServletResquest 和HttpServletResponse对象. 所有实现了 org.springframework.web.HttpRequestHandler 接口的处理器都是通过此适配器适配, 执行的. 实例如下所示:

（1）配置HttpRequestHandlerAdapter适配器

~~~xml
<!-- 配置HttpRequestHandlerAdapter适配器 --> <bean class="org.springframework.web.servlet.mvc.HttpRequestHandlerAdapter"></bean>
~~~

（2）编写处理器

~~~xml
public class HttpController implements HttpRequestHandler{

	public void handleRequest(HttpServletRequest request, HttpServletResponse response)
			throws ServletException, IOException {
		//给Request设置值，在页面进行回显
		request.setAttribute("hello", "这是HttpRequestHandler！");
		//跳转页面
		request.getRequestDispatcher("/WEB-INF/jsps/index.jsp").forward(request, response);
	}
}
~~~

（3）编写处理器

~~~xml
<html>
<body>
<h1>${hello}</h1>
</body>
</html>
~~~


**Adapter源码分析**
模拟场景：前端控制器（DispatcherServlet）接收到Handler对象后，传递给对应的处理器适配器（HandlerAdapter），处理器适配器调用相应的Handler方法。

（1）模拟处理器

~~~xml
//以下是Controller接口和它的是三种实现 
public interface Controller {
}

public class SimpleController implements Controller{
	public void doSimpleHandler() {
		System.out.println("Simple...");
	}
}

public class HttpController implements Controller{
	public void doHttpHandler() {
		System.out.println("Http...");
	}
}

public class AnnotationController implements Controller{
	public void doAnnotationHandler() {
		System.out.println("Annotation..");
	}
} 
~~~

（2）模拟处理器适配器

~~~xml
//以下是HandlerAdapter接口和它的三种实现
public interface HandlerAdapter {
	public boolean supports(Object handler);
	public void handle(Object handler);
}

public class SimpleHandlerAdapter implements HandlerAdapter{
	public boolean supports(Object handler) {
		return (handler instanceof SimpleController);
	}

	public void handle(Object handler) {
		((SimpleController)handler).doSimpleHandler();
	}
}

public class HttpHandlerAdapter implements HandlerAdapter{
	public boolean supports(Object handler) {
		return (handler instanceof HttpController);
	}

	public void handle(Object handler) {
		((HttpController)handler).doHttpHandler();
	}
}

public class AnnotationHandlerAdapter implements HandlerAdapter{
	public boolean supports(Object handler) {
		return (handler instanceof AnnotationController);
	}

	public void handle(Object handler) {
		((AnnotationController)handler).doAnnotationHandler();
	}
}

~~~

（3）模拟DispatcherServlet

~~~xml
public class Dispatcher {
	public static List<HandlerAdapter> handlerAdapter = new ArrayList<HandlerAdapter>();
	
	public Dispatcher(){
		handlerAdapter.add(new SimpleHandlerAdapter());
		handlerAdapter.add(new HttpHandlerAdapter());
		handlerAdapter.add(new AnnotationHandlerAdapter());
	}
	
	//核心功能
	public void doDispatch() {
		//前端控制器（DispatcherServlet）接收到Handler对象后
		//SimpleController handler = new SimpleController();
		//HttpController handler = new HttpController();
		AnnotationController handler = new AnnotationController();
		
		//传递给对应的处理器适配器（HandlerAdapter）
		HandlerAdapter handlerAdapter = getHandlerAdapter(handler);
		
		//处理器适配器调用相应的Handler方法
		handlerAdapter.handle(handler);
	}
	
	//通过Handler找到对应的处理器适配器（HandlerAdapter）
	public HandlerAdapter getHandlerAdapter(Controller handler) {
		for(HandlerAdapter adapter : handlerAdapter){
			if(adapter.supports(handler)){
				return adapter;
			}
		}
		return null;
	}
}
~~~

（4）测试

~~~xml
public class Test {
	public static void main(String[] args) {
		Dispatcher dispather = new Dispatcher();
		dispather.doDispatch();
	}
}
~~~



Handler
这里只介绍上文提到的两种处理器, 除此之外还有很多适用于各种应用场景的处理器, 尤其是Controller接口还有很多实现类, 大家可以自行去了解.

Controller

org.springframework.web.servlet.mvc.Controller, 该处理器对应的适配器是 SimpleControllerHandlerAdapter.

~~~xml
public interface Controller {

	/**
	 * Process the request and return a ModelAndView object which the DispatcherServlet
	 * will render. A {@code null} return value is not an error: it indicates that
	 * this object completed request processing itself and that there is therefore no
	 * ModelAndView to render.
	 * @param request current HTTP request
	 * @param response current HTTP response
	 * @return a ModelAndView to render, or {@code null} if handled directly
	 * @throws Exception in case of errors
	 */
	ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception;

}

~~~

该处理器方法用于处理用户提交的请求, 通过调用service层代码, 实现对用户请求的计算响应, 并最终将计算所得数据及要响应的页面封装为一个ModelAndView 对象, 返回给前端控制器(DispatcherServlet).

Controller接口的实现类:

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20190328153628457.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpdGlhbnhpYW5nX2thb2xh,size_16,color_FFFFFF,t_70)

HttpRequestHandler

org.springframework.web.HttpRequestHandler, 该处理器对应的适配器是 HttpRequestHandlerAdapter.

~~~xml
public interface HttpRequestHandler {
    void handleRequest(HttpServletRequest var1, HttpServletResponse var2) throws ServletException, IOException;
}
~~~


该处理器方法没有返回值, 不能像 ModelAndView 一样, 将数据及目标视图封装为一个对象, 但可以将数据放入Request, Session等域属性中, 并由Request 或 Response完成目标页面的跳转.

ViewResolver
视图解析器负责将处理结果生成View视图. 这里介绍两种常用的视图解析器:

InternalResourceViewResolver

该视图解析器用于完成对当前Web应用内部资源的封装和跳转. 而对于内部资源的查找规则是, 将ModelAndView中指定的视图名称与视图解析器配置的前缀与后缀想结合, 拼接成一个Web应用内部资源路径. 内部资源路径 = 前缀 + 视图名称 + 后缀.

InternalResourceViewResolver解析器会把处理器方法返回的模型属性都存放到对应的request中, 然后将请求转发到目标URL.

(1) 处理器

~~~xml
public class MyController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ModelAndView mv = new ModelAndView();
        mv.addObject("hello", "hello world!");
        mv.setViewName("index");
        return mv;
    }
~~~


(2) 视图解析器配置

~~~xml
<bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"></property>
        <property name="suffix" value=".jsp"></property>
   </bean>
~~~


当然, 若不指定前缀与后缀, 直接将内部资源路径写到 setViewName()中也可以, 相当于前缀与后缀均为空串.

~~~xml
mv.setViewName("/WEB-INF/jsp/index.jsp");
~~~


BeanNameViewResolver

InternalResourceViewResolver视图解析器存在一个问题, 就是只可以完成将内部资源封装后的跳转, 无法跳转向外部资源, 如外部网页.

BeanNameViewResolver 视图解析器将资源(内部资源和外部资源)封装为bean实例, 然后在 ModelAndView 中通过设置bean实例的id值来指定资源. 在配置文件中可以同时配置多个资源bean.

(1) 处理器

~~~xml
public class MyController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {

        return new ModelAndView("myInternalView");
//        return new ModelAndView("baidu");
    }
}

~~~

(2) 视图解析器配置

~~~xml
<!--视图解析器-->
<bean class="org.springframework.web.servlet.view.BeanNameViewResolver"/>
<!--内部资源view-->
<bean id="myInternalView" class="org.springframework.web.servlet.view.JstlView">
    <property name="url" value="/jsp/show.jsp"/>
</bean>
<!--外部资源view-->
<bean id="baidu" class="org.springframework.web.servlet.view.RedirectView">
    <property name="url" value="https://www.baidu.com/"/>
</bean>
~~~

了解完这两种视图解析器后是不是有种熟悉感, 是的, 就是请求转发和重定向.

中文乱码解决
Get请求乱码

Tomcat8已经解决了Get请求乱码, 如果是Tomcat8以下的版本, 可以使用以下两种方法:

更改Tomcat的配置文件server.xml

对参数进行重新编码

String userName =new
String(request.getParamter(“userName”).getBytes(“ISO-8859-1”),“UTF-8”); //ISO-8859-1是Tomcat8以下版本的默认编码

Post请求乱码
在web.xml中加入：

~~~xml
<filter>
    <filter-name>characterEncoding</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>characterEncoding</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
~~~

#### 9. [spring实战一:装配bean之注入Bean属性](https://www.cnblogs.com/longshiyVip/p/4574459.html)

https://www.cnblogs.com/longshiyVip/p/4574459.html

内容参考自spring in action一书。

创建应用对象之间协作关系的行为通常称为装配,这也是依赖注入的本质。

\1. 创建spring配置

spring是一个基于容器的框架。如果没有配置spring,那么它就是一个空的容器,所以需要配置spring来告诉容器它需要加载哪些Bean和如何装配这些bean,这样才能确保它们能够彼此协作。

 从spring3.0开始,spring容器提供了两种配置bean的方式。第一种是传统上的使用一个或多个XML文件作为配置文件。第二种提供了基于java注解的配置方式。

先说XML文件的配置方式:

在XML文件中声明Bean时,Spring配置文件的根元素是来源于Spring Beans命名空间所定义的<beans>元素。下面是一个典型的Spring XML配置文件。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
<!-- Bean declarations go here -->
</beans>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

在<beans>元素内可以放置所有的Spring配置信息,包括<bean>元素的声明。但是beans的命名空间并不是唯一的Spring命名空间。Spring的核心框架自带了10个命名空间配置。如下所示:

![img](https://images0.cnblogs.com/blog2015/676557/201506/132256398481511.png)

\2. 注入bean属性

spring注入bean属性有三种方式,常用的有两种,第一种是通过构造器注入。第二种是通过Setter注入。

第一种,通过构造器注入:

先看最简单的构造器注入方式,通过默认构造函数进行注入。

```
    <bean id="duke" class="com.pcitc.springInAction.Juggler" />
```

2.1.1 通过构造器注入简单值:

现在进行按需实例化,不走默认的构造方法:

```
<bean id="duke" class="com.pcitc.springInAction.Juggler">
        <constructor-arg value="15" />
    </bean>
```

总结:在构造bean的时候可以使用 <constructor-arg>元素来告诉spring额外的信息。如果不配置 <constructor-arg>元素,那么spring将使用默认的构造方法,上面配置了 <constructor-arg>的value属性设置为15,Spring将调用Juggler的另一个构造方法。

2.1.2 通过构造器注入对象引用:

```
<bean id="sonnet29" class="com.pcitc.springInAction.Sonnet29" />
 <bean id="poeticDuke" class="com.pcitc.springInAction.PoeticJuggler">
        <constructor-arg value="15" />
        <constructor-arg ref="sonnet29" />
    </bean>
```

总结: <constructor-arg ref="sonnet29" />,把bean引用传递给构造器,当spring碰到sonnet29和duke的声明时,它所执行的逻辑本质上和下面的java代码相同：

```
Poem sonnet29 = new Sonnet29();
Performer duke = new PoeticJuggler(15, sonnet29);
```

 

通过工厂方法创建Bean:

假如想声明的Bean没有一个公开的构造方法,那怎么办呢?方法是通过工厂方法创建bean。

有时候静态工厂方法是实例化对象的唯一方法。Spring支持通过<bean>元素的factory-method属性来装配工厂创建的bean。

定义一个单例类:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
package com.pcitc.springInAction;

public class Stage {
    private Stage() {
    }

    private static class StageSingletonHolder {
        static Stage instance = new Stage();
    }

    public static Stage getInstance() {
        return StageSingletonHolder.instance;
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

Stage类没有一个公开的构造方法,这时候只能通过Spring的factory-method属性调用一个指定的静态方法,从而代替构造方法来创建一个类的实例。

在Spring配置文件下这样来声明该bean

```
<bean id="theStage" class="com.pcitc.springInAction.Stage"
        factory-method="getInstance" />
```

第二种,通过Setter方法注入:

通常javabean的属性都是私有的,同时拥有一组存取器方法,以setXXX和getXXX形式存在.spring可以借助属性的set方法来配置属性的值,以实现setter方法注入。

简单说明:

2.2.1 注入简单值的方式:

```
<bean id="kenny"
class="com.springinaction.springidol.Instrumentalist">
<property name="song" value="Jingle Bells" />
</bean>
```

在spring配置文件中,一旦Instrumentalist被实例化,<peoperty>元素会指示Spring调用setSong方法将song属性的值设置为"Jingle Bells"。需要说明的是value值的类型不基于String类型,也可以是数值类型和boolean类型,spring会根据属性值的类型进行自动转换。

2.2.2 通过Setter注入对象引用:

```
<bean id="kenny2"
class="com.springinaction.springidol.Instrumentalist">
<property name="song" value="Jingle Bells" />
<property name="instrument" ref="saxophone" />
</bean>
```

注入内部bean:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<bean id="kenny"
class="com.springinaction.springidol.Instrumentalist">
<property name="song" value="Jingle Bells" />
<property name="instrument">
<bean class="org.springinaction.springidol.Saxophone" />
</property>
</bean>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

内部bean是通过直接声明一个<bean>元素作为<property>元素的子节点而定义的,内部bean并限于setter注入,还可以把内部bean装配到构造方法的入参中,如下所示:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<bean id="duke"
class="com.springinaction.springidol.PoeticJuggler">
<constructor-arg value="15" />
<constructor-arg>
<bean class="com.springinaction.springidol.Sonnet29" />
</constructor-arg>
</bean>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

在这里,Sonnet29的一个实例将作为内部bean被创建,并作为参数传递给PoeticJuggler的构造器。

总结:需要注意的是内部Bean没有ID属性。虽然为内部bean配置一个ID属性是合法的,但是完全没有必要,因为永远不会通过名字来引用内部bean。这也突出了使用内部bean的最大缺点:它们不能被复用。内部bean仅适用于一次注入,而且也不能被其他bean所引用,而且某种程度上还会影响Spring XML配置的可读性。

\3. 使用Spring的命名空间p装配属性

Spring的命名空间p提供了另外一种Bean属性的装配方式,命名空间的schema URI为

http://www.springframework.org/schema/p.如果想使用命名空间p,只需要在Spring的XML配置中增加如下一段声明:

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
xmlns:p="http://www.springframework.org/schema/p"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans-3.0.xsd">
```

使用方式如下:

```
<bean id="kenny" class="com.springinaction.springidol.Instrumentalist"
p:song = "Jingle Bells"
p:instrument-ref = "saxophone" />
```

p:song属性的值被设置为"Jingle Bells",将使用该值装配bean属性。p:instrument-ref属性的值被设置为"saxophone",将使用一个ID为saxophone的Bean引用来装配instrument属性。-ref后缀作为一个标识来告诉Spring应该装配一个引用而不是字面值。

\4. 装配集合元素

上面Spring的几种配置方式都只能配置简单属性值和引用其他bean的属性,当bean的属性值的类型是集合时只能使用集合的配置方式,Spring提供了四种类型的集合配置方式:

 

![img](https://images0.cnblogs.com/blog2015/676557/201506/141003538015779.png)

当装配类型为数值或者java.util.Collection任意实现的属性时,<List>和<Set>元素都可以用,其实属性实际定义的集合类型与选择<List>和<Set>没有任何关系。如果属性为任意的java.util.Collection类型时,这两个配置元素在使用时安全可以互换,不能因为属性为java.util.Set类型,就表示必须使用<set>元素来完成装配。使用<set>元素配置java.util.List类型的属性是可以的,只需要确保List中的每一个成员都必须是唯一的。

<map>和<props>这两个元素分别对应java.util.Map和java.util.Properties。当需要由键-值对组成的集合时,这两种配置元素非常有用,这两种配置元素的关键区别在于,<props>要求键和值都必须为String类型,而<map>允许键和值可以是任意类型。



4.1 装配集合元素之装配List,Set和Array

Java对instruments的声明如下:

```
private Collection<Instrument> instruments;
```

spring的配置文件配置如下:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<bean id="hank"
class="com.springinaction.springidol.OneManBand">
<property name="instruments">
<list>
<ref bean="guitar" />
<ref bean="cymbal" />
<ref bean="harmonica" />
</list>
</property>
</bean>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

<list>元素包含一个或多个值。<ref>元素用来定义Spring上下文中的其他bean的引用。当然还可以使用其他的Spring设置元素作为<list>的成员,包括<value>,<bean>和<null/>,实际上<list>还可以包含另外一个<list>作为其成员,形成多维列表。

即使像下面的方式来配置instruments属性,<list>元素也一样有效:

java.util.List<Instrument> instruments;或者Instrument[] instruments;

同样有可以使用<set>元素来装配集合类型或者数组类型的属性:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<bean id="hank"
class="com.springinaction.springidol.OneManBand">
<property name="instruments">
<set>
<ref bean="guitar" />
<ref bean="cymbal" />
<ref bean="harmonica" />
<ref bean="harmonica" />
</set>
</property>
</bean>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

4.2 装配集合元素之装配Map集合

Java对instruments的声明如下:

```
private Map<String, Instrument> instruments;
```

spring的配置文件配置如下:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<bean id="hank" class="com.springinaction.springidol.OneManBand">
<property name="instruments">
<map>
<entry key="GUITAR" value-ref="guitar" />
<entry key="CYMBAL" value-ref="cymbal" />
<entry key="HARMONICA" value-ref="harmonica" />
</map>
</property>
</bean>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

<map>元素声明了一个java.util.Map类型的值。每个<entry>元素定义Map的一个成员。key元素指定了entry的键,而value-ref属性定义了entry的值,并引用了spring上下文中的其他bean。<entry>元素还有两个属性分别可以用来指定entry的键和值。



![img](https://images0.cnblogs.com/blog2015/676557/201506/141009046443748.png)

当键和值都不是String类型时,将键-值对注入到Bean属性的唯一方法就是使用<map>元素。

4.3 装配集合元素之装配Properties集合

如果所配置的Map的每一个entry的键和值都为String类型时,可以考虑使用java.util.Properties代替Map。Properties类提供了和Map大致相同的功能,但是它限定键和值必须是String类型。

Java对instruments的声明如下:

```
private Properties instruments;
```

spring的配置文件配置如下:

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<bean id="hank" class="com.springinaction.springidol.OneManBand">
<property name="instruments">
<props>
<prop key="GUITAR">STRUM STRUM STRUM</prop>
<prop key="CYMBAL">CRASH CRASH CRASH</prop>
<prop key="HARMONICA">HUM HUM HUM</prop>
</props>
</property>
</bean>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

Properties集合的配置要点:<property>元素用于把值或Bean引用注入到Bean的属性中;<props>元素用于定义一个java.util.Properties类型的集合值;<prop>元素用于定义<props>集合的一个成员。

4.4 装配空值

如果需要把属性值设置为null值,那么可以显示的为该属性装配一个null值。显示的为属性装配null值将会覆盖自动装配的值。

```
<property name="someNonNullProperty"><null/></property>
```

#### 10.Hello Spring Boot

1. @SpringBootApplication

   https://www.jianshu.com/p/39ee4f98575c

   ~~~java
   ackage org.springframework.boot.autoconfigure;
   
   import java.lang.annotation.Documented;
   import java.lang.annotation.ElementType;
   import java.lang.annotation.Inherited;
   import java.lang.annotation.Retention;
   import java.lang.annotation.RetentionPolicy;
   import java.lang.annotation.Target;
   import org.springframework.boot.SpringBootConfiguration;
   import org.springframework.boot.context.TypeExcludeFilter;
   import org.springframework.context.annotation.ComponentScan;
   import org.springframework.context.annotation.FilterType;
   import org.springframework.context.annotation.ComponentScan.Filter;
   import org.springframework.core.annotation.AliasFor;
   
   @Target({ElementType.TYPE})
   @Retention(RetentionPolicy.RUNTIME)
   @Documented
   @Inherited
   //3
   @SpringBootConfiguration
   //2
   @EnableAutoConfiguration
   //1
   @ComponentScan(
       excludeFilters = {@Filter(
       type = FilterType.CUSTOM,
       classes = {TypeExcludeFilter.class}
   ), @Filter(
       type = FilterType.CUSTOM,
       classes = {AutoConfigurationExcludeFilter.class}
   )}
   )
   public @interface SpringBootApplication {
       @AliasFor(
           annotation = EnableAutoConfiguration.class
       )
       Class<?>[] exclude() default {};
   
       @AliasFor(
           annotation = EnableAutoConfiguration.class
       )
       String[] excludeName() default {};
   
       @AliasFor(
           annotation = ComponentScan.class,
           attribute = "basePackages"
       )
       String[] scanBasePackages() default {};
   
       @AliasFor(
           annotation = ComponentScan.class,
           attribute = "basePackageClasses"
       )
       Class<?>[] scanBasePackageClasses() default {};
   }
   
   ~~~

   @ComponentScan

   spring里有四大注解：`@Service`,`@Repository`,`@Component`,`@Controller`用来定义一个bean.`@ComponentScan`注解就是用来自动扫描被这些注解标识的类，最终生成ioc容器里的bean。

   可以通过设置`@ComponentScan`　basePackages，includeFilters，excludeFilters属性来动态确定自动扫描范围，类型已经不扫描的类型．***默认情况下:它扫描所有类型，并且扫描范围是`@ComponentScan`注解所在配置类包及子包的类***。

   使用`@SpringBootApplication`注解，就说明你使用了`@ComponentScan`的默认配置，这就建议你把使用`@SpringBootApplication`注解的类放置在root package(官方表述)下，其他类都置在root package的子包里面，这样bean就不会被漏扫描．

   @EnableAutoConfiguration

   这个注解的作用与`@Configuration`作用相同，都是用来声明当前类是一个配置类．可以通过`＠Bean`注解生成IOC容器管理的bean.在`QuickStartApplication`中定义bean，并在`＠HelloController`中注入使用。

   ~~~java
   @SpringBootApplication
   public class QuickStartApplication {
   public static void  main(String[]args){
       SpringApplication.run(QuickStartApplication.class,args);
     }
   @Bean
   public BeanTest beanTest(){
       return  new BeanTest();
       }
   }
   ~~~

   下面是`＠HelloController`

   ~~~java
   @RestController
   public class HelloController {
   @Autowired
   BeanTest beanTest;
   @RequestMapping(value = "/hello",method = RequestMethod.GET)
   public String hello(){
       return "hello world!";
   }
   @RequestMapping(value = "/beantest",method = RequestMethod.GET)
   public String beanTest(){
       return "beanTest!";
   }
   }
   ~~~

   @SpringBootConfiguration

   `@EnableAutoConfiguration`是springboot实现自动化配置的核心注解，通过这个注解把spring应用所需的bean注入容器中．`@EnableAutoConfiguration`源码通过`@Import`注入了一个`ImportSelector`的实现类
    `AutoConfigurationImportSelector`,这个`ImportSelector`最终实现根据我们的配置，动态加载所需的bean.

   AutoConfigurationImportSelector'的完成动态加载实现方法源码如下：

   ~~~java
   @Override
      //annotationMetadata 是＠import所用在的注解．这里指定是@EnableAutoConfiguration
   public String[] selectImports(AnnotationMetadata annotationMetadata) {
       if (!isEnabled(annotationMetadata)) {
           return NO_IMPORTS;
       }
        //加载XXConfiguration的元数据信息（包含了某些类被生成bean条件），继续跟进这个方法调用，就会发现加载的是：spring-boot-autoconfigure　jar包里面META-INF的spring-autoconfigure-metadata.properties
       AutoConfigurationMetadata autoConfigurationMetadata = AutoConfigurationMetadataLoader
               .loadMetadata(this.beanClassLoader);
   　　    //获取注解里设置的属性，在＠SpringBootApplication设置的exclude,excludeＮame属性值，其实就是设置＠EnableAutoConfiguration的这两个属性值
          AnnotationAttributes attributes = getAttributes(annotationMetadata);
          //从spring-boot-autoconfigure　jar包里面META-INF/spring.factories加载配置类的名称，打开这个文件发现里面包含了springboot框架提供的所有配置类
       List<String> configurations = getCandidateConfigurations(annotationMetadata,
               attributes);
        //去掉重复项
       configurations = removeDuplicates(configurations);
        //获取自己配置不需要生成bean的class
       Set<String> exclusions = getExclusions(annotationMetadata, attributes);
       //校验被exclude的类是否都是springboot自动化配置里的类，如果存在抛出异常
       checkExcludedClasses(configurations, exclusions);
      //删除被exclude掉的类
       configurations.removeAll(exclusions);
      //过滤刷选，满足OnClassCondition的类
       configurations = filter(configurations, autoConfigurationMetadata);
       fireAutoConfigurationImportEvents(configurations, exclusions);
   //返回需要注入的bean的类路径
       return StringUtils.toStringArray(configurations);
   }
   ~~~

   通过第二节我们可以看到，springboot是通过注解`@EnableAutoConfiguration`的方式，去查找，过滤，加载所需的`configuration`,`@ComponentScan`扫描我们自定义的bean,`@SpringBootConfiguration`使得被`@SpringBootApplication`注解的类声明为注解类．因此`@SpringBootApplication`的作用等价于同时组合使用`@EnableAutoConfiguration`，`@ComponentScan`，`@SpringBootConfiguration`．

2. CommunityApplication

   ```
   @SpringBootApplication//配置文件
   ```

   https://blog.csdn.net/qq_42261668/article/details/103029333

   https://blog.csdn.net/hzhahsz/article/details/83960139

~~~java
package com.nowcoder.community;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CommunityApplication {

	public static void main(String[] args) {
        //启动了tomcat,并且自动地创建了spring容器，会自动扫描类
		SpringApplication.run(CommunityApplication.class, args);
	}

}
~~~

![img](https://img-blog.csdnimg.cn/20181111182004661.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2h6aGFoc3o=,size_16,color_FFFFFF,t_70)

既然要了解原理，最直接的方式当然是看源码：

~~~java
public ConfigurableApplicationContext run(String... args) {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        ConfigurableApplicationContext context = null;
        Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList();
        this.configureHeadlessProperty();
        //初始化监听器
        SpringApplicationRunListeners listeners = this.getRunListeners(args);
        //发布ApplicationStartingEvent
        listeners.starting();
 
        try {
            //装配参数和环境
            ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
            //发布ApplicationEnvironmentPreparedEvent
            ConfigurableEnvironment environment = this.prepareEnvironment(listeners, applicationArguments);
            this.configureIgnoreBeanInfo(environment);
            Banner printedBanner = this.printBanner(environment);
            //创建ApplicationContext,并装配
            context = this.createApplicationContext();
            this.getSpringFactoriesInstances(SpringBootExceptionReporter.class, new Class[]{ConfigurableApplicationContext.class}, context);
            //发布ApplicationPreparedEvent
            this.prepareContext(context, environment, listeners, applicationArguments, printedBanner);
            this.refreshContext(context);
            this.afterRefresh(context, applicationArguments);
            stopWatch.stop();
            if (this.logStartupInfo) {
                (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), stopWatch);
            }
            
            //发布ApplicationStartedEvent
            listeners.started(context);
            //执行Spring中@Bean下的一些操作，如静态方法等
            this.callRunners(context, applicationArguments);
        } catch (Throwable var9) {
            this.handleRunFailure(context, listeners, exceptionReporters, var9);
            throw new IllegalStateException(var9);
        }
 
        //发布ApplicationReadyEvent
        listeners.running(context);
        return context;
    }
~~~



2. AlphaController

```java
package com.nowcoder.community.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
//使 AlphaController成为处理请求的控制器
@Controller
//处理来自/alpha uri的请求
@RequestMapping("/alpha")
public class AlphaController {
    //处理来自/alpha/hello uri的请求
    @RequestMapping("/hello")
    //@ResponseBody注解用于将Controller的方法返回的对象，通过springmvc提供的HttpMessageConverter接口转换为指定格式的数据如：json,xml等，通过Response响应给客户端。
    @ResponseBody
    public String sayHello() {
        return "Hello Spring Boot.";
    }

}
```

3. 页面显示

![image-20200528093317371](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200528093317371.png)

#### 11. 理解Spring容器、BeanFactory和ApplicationContext

https://www.jianshu.com/p/2854d8984dfc

一. spring容器理解

spring容器可以理解为生产对象（OBJECT）的地方，在这里容器不只是帮我们创建了对象那么简单，**它负责了对象的整个生命周期--创建、装配、销毁**。而这里对象的创建管理的控制权都交给了Spring容器，所以这是一种控制权的反转，称为IOC容器，而这里IOC容器不只是Spring才有，很多框架也都有该技术。

二. BeanFactory和ApplicationContext之间的关系

- BeanFactory和ApplicationContext是Spring的两大核心接口，而其中ApplicationContext是BeanFactory的子接口。它们都可以当做Spring的容器，Spring容器是生成Bean实例的工厂，并管理容器中的Bean。在基于Spring的Java EE应用中，所有的组件都被当成Bean处理，包括数据源，Hibernate的SessionFactory、事务管理器等。
- 生活中我们一般会把生产产品的地方称为工厂，而在这里bean对象的地方官方取名为BeanFactory，直译Bean工厂（com.springframework.beans.factory.BeanFactory），我们一般称BeanFactory为IoC容器，而称ApplicationContext为应用上下文。
- Spring的核心是容器，而容器并不唯一，框架本身就提供了很多个容器的实现，大概分为两种类型：
  一种是不常用的BeanFactory，这是最简单的容器，只能提供基本的DI功能；
  一种就是继承了BeanFactory后派生而来的ApplicationContext(应用上下文)，它能提供更多企业级的服务，例如解析配置文本信息等等，这也是ApplicationContext实例对象最常见的应用场景。

三. BeanFactory详情介绍

Spring容器最基本的接口就是BeanFactory。BeanFactory负责配置、创建、管理Bean，它有一个子接口ApplicationContext，也被称为Spring上下文，容器同时还管理着Bean和Bean之间的依赖关系。
 **spring Ioc容器的实现，从根源上是beanfactory，但真正可以作为一个可以独立使用的ioc容器还是DefaultListableBeanFactory，因此可以这么说，
 DefaultListableBeanFactory 是整个spring ioc的始祖。**

![img](https:////upload-images.jianshu.io/upload_images/12234310-6bf928fc2231465a.png?imageMogr2/auto-orient/strip|imageView2/2/w/753/format/webp)

BeanFactory结构.png



> #### 接口介绍：
>
> **1.BeanFactory接口：**
> 是Spring bean容器的根接口，提供获取bean，是否包含bean,是否单例与原型，获取bean类型，bean 别名的方法 。它最主要的方法就是getBean(String beanName)。
> **2.BeanFactory的三个子接口：**
> \* HierarchicalBeanFactory：提供父容器的访问功能
> \* ListableBeanFactory：提供了批量获取Bean的方法
> \* AutowireCapableBeanFactory：在BeanFactory基础上实现对已存在实例的管理
> **3.ConfigurableBeanFactory：**
> 主要单例bean的注册，生成实例，以及统计单例bean
> **4.ConfigurableListableBeanFactory：**
> 继承了上述的所有接口，增加了其他功能：比如类加载器,类型转化,属性编辑器,BeanPostProcessor,作用域,bean定义,处理bean依赖关系, bean如何销毁…
> **5.实现类DefaultListableBeanFactory[详细介绍](https://www.cnblogs.com/sten/p/5758161.html)：**
> 实现了ConfigurableListableBeanFactory，实现上述BeanFactory所有功能。它还可以注册BeanDefinition
> 接口详细介绍请参考:[揭秘BeanFactory](https://blog.csdn.net/u011179993/article/details/51636742)

四. ApplicationContext介绍

如果说BeanFactory是Sping的心脏，那么ApplicationContext就是完整的身躯了。



![img](https:////upload-images.jianshu.io/upload_images/12234310-a14ad5a594b524fb.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

ApplicationContext结构图.png

![img](https:////upload-images.jianshu.io/upload_images/12234310-be1edded652cea7b.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

ApplicationContext类结构树.png

|     ApplicationContext常用实现类      |                             作用                             |
| :-----------------------------------: | :----------------------------------------------------------: |
|  AnnotationConfigApplicationContext   | 从一个或多个基于java的配置类中加载上下文定义，适用于java注解的方式。 |
|    ClassPathXmlApplicationContext     | 从类路径下的一个或多个xml配置文件中加载上下文定义，适用于xml配置的方式。 |
|    FileSystemXmlApplicationContext    | 从文件系统下的一个或多个xml配置文件中加载上下文定义，也就是说系统盘符中加载xml配置文件。 |
| AnnotationConfigWebApplicationContext |            专门为web应用准备的，适用于注解方式。             |
|       XmlWebApplicationContext        | 从web应用下的一个或多个xml配置文件加载上下文定义，适用于xml配置方式。 |

Spring具有非常大的灵活性，它提供了三种主要的装配机制：

- 1. 在XMl中进行显示配置
- 1. 在Java中进行显示配置
- 1. 隐式的bean发现机制和自动装配
     *组件扫描（component scanning）：Spring会自动发现应用上下文中所创建的bean。
     *自动装配（autowiring）：Spring自动满足bean之间的依赖。

（使用的优先性: 3>2>1）尽可能地使用自动配置的机制，显示配置越少越好。当必须使用显示配置bean的时候（如：有些源码不是由你来维护的，而当你需要为这些代码配置bean的时候），推荐使用类型安全比XML更加强大的JavaConfig。最后只有当你想要使用便利的XML命名空间，并且在JavaConfig中没有同样的实现时，才使用XML。

代码示例：

1. 通过xml文件将配置加载到IOC容器中

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
     <!--若没写id，则默认为com.test.Man#0,#0为一个计数形式-->
    <bean id="man" class="com.test.Man"></bean>
</beans>
```

```java
public class Test {
    public static void main(String[] args) {
        //加载项目中的spring配置文件到容器
        //ApplicationContext context = new ClassPathXmlApplicationContext("resouces/applicationContext.xml");
        //加载系统盘中的配置文件到容器
        ApplicationContext context = new FileSystemXmlApplicationContext("E:/Spring/applicationContext.xml");
        //从容器中获取对象实例
        Man man = context.getBean(Man.class);
        man.driveCar();
    }
}
```

1. 通过java注解的方式将配置加载到IOC容器

```java
//同xml一样描述bean以及bean之间的依赖关系
@Configuration
public class ManConfig {
    @Bean
    public Man man() {
        return new Man(car());
    }
    @Bean
    public Car car() {
        return new QQCar();
    }
}
```

```java
public class Test {
    public static void main(String[] args) {
        //从java注解的配置中加载配置到容器
        ApplicationContext context = new AnnotationConfigApplicationContext(ManConfig.class);
        //从容器中获取对象实例
        Man man = context.getBean(Man.class);
        man.driveCar();
    }
}
```

1. 隐式的bean发现机制和自动装配

```java
/**
 * 这是一个游戏光盘的实现
 */
//这个简单的注解表明该类回作为组件类，并告知Spring要为这个创建bean。
@Component
public class GameDisc implements Disc{
    @Override
    public void play() {
        System.out.println("我是马里奥游戏光盘。");
    }
}
```

不过，组件扫描默认是不启用的。我们还需要显示配置一下Spring，从而命令它去寻找@Component注解的类，并为其创建bean。

```java
@Configuration
@ComponentScan
public class DiscConfig {
}
```

我们在DiscConfig上加了一个@ComponentScan注解表示在Spring中开启了组件扫描，默认扫描与配置类相同的包，就可以扫描到这个GameDisc的Bean了。这就是Spring的自动装配机制。

------

**除了提供BeanFactory所支持的所有功能外ApplicationContext还有额外的功能**

- 默认初始化所有的Singleton，也可以通过配置取消预初始化。
- 继承MessageSource，因此支持国际化。
- 资源访问，比如访问URL和文件。
- 事件机制。
- 同时加载多个配置文件。
- 以声明式方式启动并创建Spring容器。

> 注：由于ApplicationContext会预先初始化所有的Singleton Bean，于是在系统创建前期会有较大的系统开销，但一旦ApplicationContext初始化完成，程序后面获取Singleton Bean实例时候将有较好的性能。也可以为bean设置lazy-init属性为true，即Spring容器将不会预先初始化该bean。

参考文章(一部分在上述链接中，这里就不加了)：

- [BeanFactory和ApplicationContext](https://www.jianshu.com/p/f8d560962a68)
- [Spring容器BeanFactory和ApplicationContext对比](https://blog.csdn.net/u010871004/article/details/53589642)
- [Spring基础篇——Spring容器和应用上下文理解](https://www.cnblogs.com/chenbenbuyi/p/8166304.html)
- [ApplicationContext探究](https://www.jianshu.com/p/8aaad9cff96b)
- [Spring 装配bean的三种方式（注解，xml，自动装配）](https://www.jianshu.com/p/bae2e02b0c5d)

#### 12.理解ioc的执行过程

~~~java
package com.nowcoder.community;

import com.nowcoder.community.dao.AlphaDao;
import com.nowcoder.community.service.AlphaService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import java.text.SimpleDateFormat;
import java.util.Date;

@RunWith(SpringRunner.class)
@SpringBootTest
//将此类作为配置类
@ContextConfiguration(classes = CommunityApplication.class)
public class CommunityApplicationTests implements ApplicationContextAware {
//成员变量记录一下
	private ApplicationContext applicationContext;
//将此类作为配置类
	@Override
    //传入的参数applicationContext就是一个bean容器，applicationContext继承于顶层接口BeanFactory
	public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
		this.applicationContext = applicationContext;
	}

	@Test
	public void testApplicationContext() {
		System.out.println(applicationContext);
        //根据类型去获取bean
		AlphaDao alphaDao = applicationContext.getBean(AlphaDao.class);
		//面向接口编程
		System.out.println(alphaDao.select());
        //通过bean的名字去获取bean
		alphaDao = applicationContext.getBean("alphaHibernate", AlphaDao.class);
		System.out.println(alphaDao.select());
	}

	@Test
	public void testBeanManagement() {
		AlphaService alphaService = applicationContext.getBean(AlphaService.class);
		System.out.println(alphaService);

		alphaService = applicationContext.getBean(AlphaService.class);
		System.out.println(alphaService);
	}
//getbean笨拙注入
	@Test
	public void testBeanConfig() {
		SimpleDateFormat simpleDateFormat =
				applicationContext.getBean(SimpleDateFormat.class);
		System.out.println(simpleDateFormat.format(new Date()));
	}
//自动注入
	@Autowired
	//指定注入的bean
	@Qualifier("alphaHibernate")
	private AlphaDao alphaDao;

	@Autowired
	private AlphaService alphaService;

	@Autowired
	private SimpleDateFormat simpleDateFormat;

	@Test
	public void testDI() {
		System.out.println(alphaDao);
		System.out.println(alphaService);
		System.out.println(simpleDateFormat);
	}

}

~~~



#### 12. MVC处理请求

1. **复杂的方式：**

   获取请求数据：HttpServletRequest

   返回响应数据：HttpServletResponse

```java
@RequestMapping("/http")
public void http(HttpServletRequest request, HttpServletResponse response) {
    // 获取请求数据
    System.out.println(request.getMethod());
    System.out.println(request.getServletPath());
    //得到
    Enumeration<String> enumeration = request.getHeaderNames();
    while (enumeration.hasMoreElements()) {
        String name = enumeration.nextElement();
        String value = request.getHeader(name);
        System.out.println(name + ": " + value);
    }
    System.out.println(request.getParameter("code"));

    // 返回响应数据
    response.setContentType("text/html;charset=utf-8");
    try (
            PrintWriter writer = response.getWriter();
    ) {
        writer.write("<h1>牛客网</h1>");
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

~~~html
<!DOCTYPE html>
#http://www.thymeleaf.org->表明是一个模板，而不是动态页面
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Teacher</title>
</head>
<body>
    <p th:text="${name}"></p>
    <p th:text="${age}"></p>
</body>
</html>
~~~

1. 使用GET请求，浏览器向服务器获取数据

~~~java
// /students?current=1&limit=20   //http://localhost:8080/community/alpha/students?current=2&limit=8
//
//使用RequestMapping指定访问路径，method = RequestMethod.GET：限制处理请求方式为GET
    @RequestMapping(path = "/students", method = RequestMethod.GET)
    @ResponseBody
    public String getStudents(
        //@RequestParam取参数：需要参数名一致，required = false表示可以不传参数，defaultValue表示默认值
            @RequestParam(name = "current", required = false, defaultValue = "1") int current,
            @RequestParam(name = "limit", required = false, defaultValue = "10") int limit) {
        System.out.println(current);
        System.out.println(limit);
        return "some students";
    }
~~~


@RequestParam 参数绑定，required = false表示可以不传入参数， defaultValue = "1"表示默认值

直接跟参数：

```
// /student/123
@RequestMapping(path = "/student/{id}", method = RequestMethod.GET)
@ResponseBody
public String getStudent(@PathVariable("id") int id) {
    System.out.println(id);
    return "a student";
}
```

@PathVariable ：这种情况下使用这个注解绑定参数

3. POST：浏览器向服务器请求数据

   使用get请求也可以向服务器传递数据，但是会明文显示，而且uri是限制长度的。

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>增加学生</title>
</head>
<body>

    <form method="post" action="/community/alpha/student">
        <p>
            姓名: <input type="text" name="name">
        </p>
        <p>
            年龄: <input type="text" name="age">
        </p>
        <p>
            <input type="submit" value="保存">
        </p>
    </form>

</body>
</html>
~~~

上方/community/alpha/student路径要与post路径一致

~~~java
  @RequestMapping(path = "/student", method = RequestMethod.POST)
    @ResponseBody
    public String saveStudent(String name, int age) {
        System.out.println(name);
        System.out.println(age);
        return "success";
    }
~~~

4. 响应HTML数据

   之前都是响应字符串，现在响应html数据。

~~~html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Teacher</title>
</head>
<body>
    <p th:text="${name}"></p>
    <p th:text="${age}"></p>
</body>
</html>
~~~

~~~java
//第一种方式
@RequestMapping(path = "/teacher", method = RequestMethod.GET)
//所有的组件都是
    public ModelAndView getTeacher() {
        ModelAndView mav = new ModelAndView();
        mav.addObject("name", "张三");
        mav.addObject("age", 30);
        mav.setViewName("/demo/view");
        return mav;
    }

~~~

~~~java
//第二种方式
//Model由DispatcherServlet管理
@RequestMapping(path = "/school", method = RequestMethod.GET)
    public String getSchool(Model model) {
        model.addAttribute("name", "北京大学");
        model.addAttribute("age", 80);
        return "/demo/view";
    }
~~~

5. 返回JSON数据响应

一般在**异步**请求中：**页面没刷新的情况下 ，输入用户名，客户端偷偷访问服务器，并返回该用户名已被使用的信息。这是返回的不是HTML信息，而是一个json**（跨语言环境下好用）。

后端------------------------------------------> 前端：
Java对象 ----> JSON对象 （字符串格式）---->JS对象

// 响应JSON数据(异步请求)

@ResponseBody和类型会自动返回json数据，

不写@ResponseBody默认返回HTML。

~~~java
@RequestMapping(path = "/emp", method = RequestMethod.GET)
    @ResponseBody
    public Map<String, Object> getEmp() {
        Map<String, Object> emp = new HashMap<>();
        emp.put("name", "张三");
        emp.put("age", 23);
        emp.put("salary", 8000.00);
        return emp;
    }
//返回格式如下
//{"name":"张三","salary":8000.0,"age":23}
~~~

~~~java
 @RequestMapping(path = "/emps", method = RequestMethod.GET)
    @ResponseBody
    public List<Map<String, Object>> getEmps() {
        List<Map<String, Object>> list = new ArrayList<>();

        Map<String, Object> emp = new HashMap<>();
        emp.put("name", "张三");
        emp.put("age", 23);
        emp.put("salary", 8000.00);
        list.add(emp);

        emp = new HashMap<>();
        emp.put("name", "李四");
        emp.put("age", 24);
        emp.put("salary", 9000.00);
        list.add(emp);

        emp = new HashMap<>();
        emp.put("name", "王五");
        emp.put("age", 25);
        emp.put("salary", 10000.00);
        list.add(emp);

        return list;
    }
//返回格式如下
//[{"name":"张三","salary":8000.0,"age":23},{"name":"李四","salary":9000.0,"age":24},{"name":"王五","salary":10000.0,"age":25}]
~~~

#### 12. ThymeLeaf

**ThymeLeaf**：生成动态HTML的模版引擎，以HTML文件为模版

通过一些表达式动态替换模版文件中的数据，模版文件放在resources/templates中

#### 13. application.properties

导入依赖：

~~~xml
  <dependency>
	   <groupId>org.springframework.boot</groupId>
	   <artifactId>spring-boot-starter-logging</artifactId>
	</dependency>
  </dependencies>
~~~

配置中spring.thymeleaf.xxx = yyy;实际上是将配置类的xxx属性赋值。

#### 14. Mysql服务安装

1. ![image-20200529101536640](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200529101536640.png)
2. mysql安装教程 The service already exists解决方法

https://blog.csdn.net/weixin_43143345/article/details/102856312?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.nonecase

mysql安装教程，分享下mysql免安装版配置过程中出现The service already exists问题的解决方法。
一般遇到这种错误都是安装、mysql，卸载再安装，之前的mysql卸载了，但是mysql服务没有卸载干净。

1、管理员身份运行cmd
2、输入sc query mysql，查看一下名为mysql的服务：

3、命令sc delete mysql，删除该mysql

4、创建my.ini配置文件
my.ini配置文件参数解释(转)：https://www.cnblogs.com/feiquan/p/9937308.html

~~~txt
[mysqld]
port = 3306
basedir=D:\Program Files\mysql-5.6.26-winx64
datadir=D:\Program Files\mysql-5.6.26-winx64\data 
max_connections=200
character-set-server=utf8
default-storage-engine=INNODB
sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
[mysql]
default-character-set=utf8
~~~

5、进入D:\Program Files\mysql-5.6.26-winx64\bin目录，输入命令 mysqld --install 装载mysql服务
6、输入命令 mysqld --initialize-insecure --user=mysql 初始化数据库，成功后，会生成data目录并生成root用户
7、输入命令 net start mysql启动服务
8、登录 mysql，输入命令 mysql -u root -p，初始化之后没有密码，直接回车就可登录。
9、修改密码执行命令 mysqladmin -uroot -p password 新密码 设置新密码，确认新密码。

3. 解决“指定的服务已经标记为删除”问题

https://blog.csdn.net/xingxing513234072/article/details/7988869

在注册DotNetWinService服务时，再使用 "sc delete 服务器名称" 命令删除服务就出现“指定的服务已经标记为删除”的异常。

刚开始感觉很奇怪，因为在网上查到别人都是那么删除windows服务的。

在一次偶然情况，我关闭了服务管理窗口，问题自然解决了。

因此，出现上述原因是运行删除服务项命令的时候，服务管理窗口未关闭引起的。

**关闭服务管理窗口，重新删除、安装服务项即可。**

4. [ERROR 1045 (28000): Access denied for user 'root'@'localhost'](https://segmentfault.com/a/1190000015678751)

一.编辑my.ini文件

5.7以后的版本my.ini配置文件的目录发生了改变
放在了`C:\ProgramData\MySQL\MySQL Server 8.0之中`
用Notepad打开后，在[mysqld]下加入`skip-grant-tables`，保存退出

二.重启MySQL

进入cmd命令行，先后输入

```
net stop mysql
net start mysql
```

（如果拒绝访问的话请以管理员身份运行cmd，文末参考资料有教程）

三.登录

这时cmd中输`入mysql -u root -p`就不需要密码登录了，出现password直接回车进入
但操作会受到限制，因为没有权限

四.重设密码(**注意分号的使用**)

![image-20200529113905600](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200529113905600.png)

1.进入mysql数据库：

```
mysql>use mysql;
```

2.为root用户设置新密码

```
mysql> update user set password=password("19960318") where user="root";
```

3.刷新数据库

```
mysql>flush privileges;
```

4.退出mysql

```
mysql> quit
```

五.重新编辑my.ini

把刚才加入的“skip-grant-tables”去掉，再重启mysql

5. 导入表

6. 建表

![image-20200529114841021](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200529114841021.png)

2. 导入表

![image-20200529114908627](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200529114908627.png)

3. 导入数据

![image-20200529115005048](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200529115005048.png)

4. 查询数据

![image-20200529115054655](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200529115054655.png)

#### 15. MyBatis

MyBatis 是一款优秀的持久层框架，它支持定制化 SQL、存储过程以及高级映射。MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。MyBatis 可以使用简单的 XML 或注解来配置和映射原生类型、接口和 Java 的 POJO（Plain Old Java Objects，普通老式 Java 对象）为数据库中的记录。

核心组件

--SqlSessionFactory: 用于创建SqlSession的工厂类

--SqlSession：MyBatis的核心组件，用于面向数据库执行SQL

--主配置文件：XML配置文件，可以对MyBatis的底层行为做出详细的配置

--Mapper接口：Dao接口，在MyBatis中习惯称为Mapper

--Mapper映射器：用于编写SQL，并将SQL和实体类映射的组件，采用XML、注解均可实现

**spring整合MyBatis,mysql**

~~~xml
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<version>8.0.16</version>
		</dependency>
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.0.1</version>
		</dependency>
~~~

~~~properties
# 数据库以及连接池：统一管理链接的工厂
# 管理最大链接数，使连接复用
# DataSourceProperties
# 驱动启动路径
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
# mysql链接的路径，useSSL=false不适用安全链接
spring.datasource.url=jdbc:mysql://localhost:3306/community?characterEncoding=utf-8&useSSL=false&serverTimezone=Hongkong
# 账号和密码
spring.datasource.username=root
spring.datasource.password=19960318
# 性能最好的链接池
spring.datasource.type=com.zaxxer.hikari.HikariDataSource
# 最大连接数
spring.datasource.hikari.maximum-pool-size=15
# 最小空闲链接
spring.datasource.hikari.minimum-idle=5
# 超时时间，数据库断开时间
spring.datasource.hikari.idle-timeout=30000
# MybatisProperties
#映射文件存放位置
mybatis.mapper-locations=classpath:mapper/*.xml
#声明实体类所包的包名
mybatis.type-aliases-package=com.nowcoder.community.entity
#如果插入的表以自增列为主键，则允许 JDBC 支持自动生成主键，并可将自动生成的主键返回
mybatis.configuration.useGeneratedKeys=true
#让下划线的命名方式和驼峰命名匹配
mybatis.configuration.mapUnderscoreToCamelCase=true
~~~

```
//maybatis的注解相当于@Repository
@Mapper
```

什么是实体类

实体类是概念性的数据密集类-他们的主要目的是存储数据并提供对这些数据的访问。
在很多情况下，实体类是持久的，这意味着数据是长久存在的，并需要存储于文件或者数据库中。
面向对象的静态建模和频繁用于逻辑数据库设计的实体关系建模的主要不同是，尽管两种方法都对类、类属性、类间关系建模，
但面向对象的静态建模还允许规定类的操作。

1. 新建一个实体类

~~~java
package com.nowcoder.community.entity;

import java.util.Date;

public class User {

    private int id;
    private String username;
    private String password;
    private String salt;
    private String email;
    private int type;
    private int status;
    private String activationCode;
    private String headerUrl;
    private Date createTime;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public String getSalt() {
        return salt;
    }

    public void setSalt(String salt) {
        this.salt = salt;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public int getType() {
        return type;
    }

    public void setType(int type) {
        this.type = type;
    }

    public int getStatus() {
        return status;
    }

    public void setStatus(int status) {
        this.status = status;
    }

    public String getActivationCode() {
        return activationCode;
    }

    public void setActivationCode(String activationCode) {
        this.activationCode = activationCode;
    }

    public String getHeaderUrl() {
        return headerUrl;
    }

    public void setHeaderUrl(String headerUrl) {
        this.headerUrl = headerUrl;
    }

    public Date getCreateTime() {
        return createTime;
    }

    public void setCreateTime(Date createTime) {
        this.createTime = createTime;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", salt='" + salt + '\'' +
                ", email='" + email + '\'' +
                ", type=" + type +
                ", status=" + status +
                ", activationCode='" + activationCode + '\'' +
                ", headerUrl='" + headerUrl + '\'' +
                ", createTime=" + createTime +
                '}';
    }

}

~~~

2. 生成dao接口，只需要接口

~~~java
package com.nowcoder.community.dao;

import com.nowcoder.community.entity.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper {

    User selectById(int id);

    User selectByName(String username);

    User selectByEmail(String email);

    int insertUser(User user);

    int updateStatus(int id, int status);

    int updateHeader(int id, String headerUrl);

    int updatePassword(int id, String password);

}

~~~

3. 写xml文件

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.nowcoder.community.dao.UserMapper">

    <sql id="insertFields">
        username, password, salt, email, type, status, activation_code, header_url, create_time
    </sql>

    <sql id="selectFields">
        id, username, password, salt, email, type, status, activation_code, header_url, create_time
    </sql>

    <select id="selectById" resultType="User">
        select <include refid="selectFields"></include>
        from user
        where id = #{id}
    </select>

    <select id="selectByName" resultType="User">
        select <include refid="selectFields"></include>
        from user
        where username = #{username}
    </select>

    <select id="selectByEmail" resultType="User">
        select <include refid="selectFields"></include>
        from user
        where email = #{email}
    </select>

    <insert id="insertUser" parameterType="User" keyProperty="id">
        insert into user (<include refid="insertFields"></include>)
        values(#{username}, #{password}, #{salt}, #{email}, #{type}, #{status}, #{activationCode}, #{headerUrl}, #{createTime})
    </insert>

    <update id="updateStatus">
        update user set status = #{status} where id = #{id}
    </update>

    <update id="updateHeader">
        update user set header_url = #{headerUrl} where id = #{id}
    </update>

    <update id="updatePassword">
        update user set password = #{password} where id = #{id}
    </update>

</mapper>
~~~

4. 测试

~~~java
package com.nowcoder.community;

import com.nowcoder.community.dao.DiscussPostMapper;
import com.nowcoder.community.dao.UserMapper;
import com.nowcoder.community.entity.DiscussPost;
import com.nowcoder.community.entity.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Date;
import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = CommunityApplication.class)
public class MapperTests {

    @Autowired
    private UserMapper userMapper;

    @Autowired
    private DiscussPostMapper discussPostMapper;
//查
    @Test
    public void testSelectUser() {
        User user = userMapper.selectById(101);
        System.out.println(user);

        user = userMapper.selectByName("liubei");
        System.out.println(user);

        user = userMapper.selectByEmail("nowcoder101@sina.com");
        System.out.println(user);
    }
//增
    @Test
    public void testInsertUser() {
        User user = new User();
        user.setUsername("test");
        user.setPassword("123456");
        user.setSalt("abc");
        user.setEmail("test@qq.com");
        user.setHeaderUrl("http://www.nowcoder.com/101.png");
        user.setCreateTime(new Date());

        int rows = userMapper.insertUser(user);
        System.out.println(rows);
        System.out.println(user.getId());
    }
//改
    @Test
    public void updateUser() {
        int rows = userMapper.updateStatus(150, 1);
        System.out.println(rows);

        rows = userMapper.updateHeader(150, "http://www.nowcoder.com/102.png");
        System.out.println(rows);

        rows = userMapper.updatePassword(150, "hello");
        System.out.println(rows);
    }

    @Test
    public void testSelectPosts() {
        List<DiscussPost> list = discussPostMapper.selectDiscussPosts(149, 0, 10);
        for(DiscussPost post : list) {
            System.out.println(post);
        }

        int rows = discussPostMapper.selectDiscussPostRows(149);
        System.out.println(rows);
    }

}
~~~

5.设置日志级别

#logging.level.com.nowcoder.community=debug

**Mapper中的方法和DAO接口方法是怎么绑定到一起的?**

mybatis通过JDK的动态代理方式，在启动加载配置文件时，根据配置mapper的xml去生成Dao的实现。

session.getMapper()使用了代理，当调用一次此方法，都会产生一个代理class的instance,看看这个代理class的实现.

#### 16.git

#### 17.项目调试技巧

1. **常用状态码**

**302 Found**：重定向 https://blog.csdn.net/liangxy2014/article/details/78964928

Http 302对应生活中的真实例子，可以类比手机所对应的呼叫转移功能，这样打进A手机的电话，均转移到B手机接听。

- 比如一个portal页面，换了新的域名，但是老的域名地址还有很多用户在使用，这样可以对老域名配置302跳转到新域名地址，保证服务的延续。
- 另外对于一些客户端预埋的Url链接，免不了老版本地址失效与更改，将老地址配置302跳转到新地址，这样就能够全面兼容所有客户端版本。 

举个栗子：对302有一个感性全面认知，在浏览器输入taobao.com，进行抓包，如下图，访问taobao.com后，服务器吐回状态码302 Found，然后Locatioin字段标识浏览器应该跳转到了www.taobao.com，从而打开了淘宝网站。

对应于不同域名指向同一个地址的情况，大多是这么处理的。 

**404 Not Found**：
请求失败，期望得到的资源在服务器上未被发现。往往是**路径错误**

**500 Internal Server Error**：
服务器不知道怎么去处理，这种情况下应该检查服务器程序是否写错

2. **常用调试快捷键**

Step Over (F8)：步过，一行一行地往下走，如果这一行上有方法不会进入方法。

Step Into (F7)：步入，如果当前行有方法，可以进入方法内部，一般用于进入自定义方法内，不会进入官方类库的方法，如第25行的put方法

Step Out (Shift + F8)：步出，从步入的方法内退出到方法调用处，此时方法已执行完毕，只是还没有完成赋值。

Run to Cursor (Alt + F9)：运行到光标处，你可以将光标定位到你需要查看的那一行，然后使用这个功能，代码会运行至光标行，而不需要打断点。

3. **日志**

就是为了看错误的，因为开发过后页面是看2113不到错误的，最多有5261个系统繁忙，资源找不到的错误页面(这些都是经过美工处理后的4102页面，像原始的404,500这些错误都是一些英文字母，这种东1653西在成熟的页面时不内允许出现的)，既然看不到错误，就知道能在后台日志文件里打印容出来看了，然后再修改

将日志存为文件，在配置文件里加上：

```
#logging.file=d:/work/data/nowcoder/community.log
1
```

调整日志输出级别：

```
logging.level.com.nowcoder.community=debug
```

#### 18. 日志类Logger的使用

https://blog.csdn.net/qq_37866486/article/details/90699339?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase

一.使用Logger的步骤
 1.引入Logger和Logger工厂类

2.声明logger

3.记录日志

二.简单示例
//1. 引入slf4j接口的Logger和LoggerFactory
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class UserService {
  //2. 声明一个Logger，这个是static的方式，我比较习惯这么写。
  private final static Logger logger = LoggerFactory.getLogger(UserService.class);

  public boolean verifyLoginInfo(String userName, String password) {
    //3. log it，输出的log信息将会是："Start to verify User [Justfly]
    logger.info("Start to verify User [{}]", userName);
    return false;
  }
}
    这里使用的是静态的logger对象，因为这样更符合语义，可以节省cpu节省内存，不支持注入

三.方法 public void info(String msg); 输出msg信息

public void info(String format, Object arg); logger.info("开始导入配置文件[{}]","/somePath/config.properties");

public void info(String format, Object arg1, Object arg2); logger.info("开始从配置文件[{}]中读取配置项[{}]的值","/somePath/config.properties","maxSize");

public void info(String msg, Throwable t);logger.info("读取配置文件时出现异常",new FileNotFoundException("File not exists"));记录异常信息

**另外：**

引入包import org.apache.log4j.Logger;

protected static Logger LOG=Logger.getLogger(myclass.class);

若是去掉static,那每个类对象就返回一个Logger类，增加了开销

getLogger(" ")中的字符串写什么都行，只是打印日志的时候会显示出来



protected static Logger LOG=Logger.getLogger(myclass.class);

和protected static Logger LOG=Logger.getLogger(this.getClass());的区别

this表示当前对象的引用

getClass()是java.lang.Object中的方法，他返回一个对象的运行时类

获得一个Logger对象，将这个Logger监视this.getClass这个运行时类

这个运行时类里面你可能创建了log,info()等

那这些语句就会根据你预先定义的Logger级别来输出你的日志

Logger.getLogger(this.getClass())这样写，好处是只需要在基类中写一行代码就可以了

子类可以直接使用，这也是复用的原则



**总结**：this.getClass()是获取当前正在运行的类

myclass.class和myclass.getClass()的区别

public final native Class getClass();

xx.class是程序运行时就加载到jvm中的

xx.getClass()是程序运行时动态加载的

native表示该方法的实现是通过jvm之外的代码来实现

.class是一个静态常量

.getClass()是一个普通的成员函数

所以，

类名.class

类创建的对象.getClass()

因此，一个是通过类找到class对象，一个是通过类的实例对象找到class对象

myclass.class字段是编译器在编译myclass类时，自动加上去的，这个class是静态的、不可变的

myclass.getClass()是对象级别的，需要由myclass的对象进行调用

编译器自说自话的给类或对象加上额外的字段

除了class之外，还有数组的length也是这样

protected Logger logger=LoggerFactory.getLogger(class)

参数中可以使用两种方法

LoggerFactory.getLogger ( getClass( ) )

这个class中的logger可以被每个子类共用，而不需要每个子类都写一句LoggerFactory.getLogger(someclass.class)

因为getClass是一个可复写的方法，提供了灵活性

所以能用getClass()就用，不能用的时候就class

因为如果子类继承了父类中logger对象是采用getClass()来获取类时

此时返回的是子类的类名

logger.getClass().getName() 通过对象获取类名

Class.forName("包.类名称")也是一种访问class的方法

#### 18. 浅谈Java中try catch 的用法

[https](https://blog.csdn.net/wanghuiwei888/article/details/78818203)：//blog.csdn.net/wanghuiwei888/article/details/78818203

我们编译运行程序出错的时候，编译器就会抛出异常。抛出异常要比终止程序灵活许多，这是因为的Java提供了一个“捕获”异常的的处理器（处理器）对异常情况进行处理。如果没有提供处理器机制，程序就会终止，并在控制台上打印一条信息，给出异常的类型.L 

比如：使用了NULL引用或者是数组越界等。

**异常有两种类型**：**未检查异常和已检查异常**

对于已检查异常，处理器器将会检查是否提供了处理器。然而有许多的异常，如：访问null引用，都属于未检查异常。编译器不会查看是否为这些错误提供了处理器。毕竟，应该用严谨的态度来对待写代码，依次避免这些错误的发生，而不是将精力花在编写异常处理器上。

废话少说，show coder：  
 / * try catch：自己处理异常

  * try { 
    *可能出现异常的代码
    *} catch（异常类名A e）{ 
    *如果出现了异常类A类型的异常，那么执行该代码
    *} ...（catch可以有多个）
  * finally { 
    *最终肯定必须要执行的代码（例如释放资源的代码）
    *}
    *代码执行的顺序：
  * 1.try内的代码从出现异常的那一行开始，中断执行
  * 2.执行对应的catch块内的代码
  * 3.继续执行try catch结构之后的代码
    *注意点：
  * 1.如果catch内的异常类存在子父类的关系，那么子类应该在前，父类在后
  * 2。如果最后中有返回语句，那么最后返回的结果肯定以最终中的返回值为准
  * 3。如果最后语句中有回报，那么没有被处理的异常将会被吞掉
    *重写的注意点：
  * 1.儿子不能比父亲的本事大
  * 2.儿子要比父亲开放
  * 3.儿子不能比父亲惹更大的麻烦（子类的异常的类型不能是父类的异常的父类型）
    *异常类Api：
  * 1。的getMessage（）：获取异常描述信息字符串
  * 2。的toString（）：返回异常类的包路径和类
    名和异常描述信息字符串  * 3。的printStackTrace（）：除了打印的toString的信息外，还要打印堆栈信息

~~~java
package Bird;
 
import java.io.FileNotFoundException;
import java.io.FileReader;
 
//
public class TestYc {
	public static void main(String[] args) {
		try{
			FileReader fr = new FileReader("c:/abc.txt");
					} catch (FileNotFoundException e) {
						//打印输出异常
						e.printStackTrace();
					}		
	
	Mother mother  = new Mother();
	mother.bbb();
	//1.編譯時異常
	//讀取該路徑"c:/abc.txt"下的文件
	/* try {
		 FileReader fr = new FileReader("c:/abc.txt");
	 }catch(FileNotFoundException e) {
		//打印输出异常
			e.printStackTrace();
	 }*/
	int [] arr = new int[] {1,2,3};
	System.out.println(arr[2]);
	}
}
 
class Mother {
 
	private Boy b = null;
 
	// 构造器
	public Mother() {
 
		b = new Boy();
 
	}
 
	public void bbb() {
		// TODO Auto-generated method stub
		//调用带有异常的方法
		try {
			b.aaa();
		}catch(FileNotFoundException e) {
			e.printStackTrace();
			
		}
		
	}
}
 
class Boy {
	// throws 把异常抛给上层的调用者
	public  void aaa() throws FileNotFoundException{
		FileReader fr = new FileReader("c:/abc.txt");
	}
}
~~~

#### 18. spring中的异常处理

#### 19.Context

https://www.cnblogs.com/jingmengxintang/p/7889311.html

1、Context 概念

 Context是个抽象类，通过类的结构可以看到：Activity、Service、Application都是Context的子类；

从Android系统的角度来理解：Context是一个场景，描述的是一个应用程序环境的信息，即上下文，代表与操作系统的交互的一种过程。

从程序的角度上来理解：Context是个抽象类，而Activity、Service、Application等都是该类的一个实现。

#### [20.model.addattribute()的作用](https://www.cnblogs.com/yingyigongzi/p/9295696.html)

1.往前台传数据，可以传对象，可以传List，通过el表达式 ${}可以获取到，

类似于request.setAttribute("sts",sts)效果一样。

2.@ModelAttribute("model")  注解

#### 21. HTTP专题

参考链接：[link1](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)[link2](https://www.ietf.org)

##### [1]    HTTP是什么？

**超文本传输协议（HTTP）**是一个用于传输超媒体文档（例如 HTML）的[应用层](https://en.wikipedia.org/wiki/Application_Layer)协议。它是为 Web 浏览器与 Web 服务器之间的通信而设计的，但也可以用于其他目的。HTTP 遵循经典的[客户端-服务端模型](https://en.wikipedia.org/wiki/Client–server_model)，客户端打开一个连接以发出请求，然后等待它收到服务器端响应。HTTP 是[无状态协议](http://en.wikipedia.org/wiki/Stateless_protocol)，这意味着服务器不会在两个请求之间保留任何数据（状态）。该协议虽然通常基于 TCP/IP 层，但可以在任何可靠的[传输层](https://zh.wikipedia.org/wiki/传输层)上使用；也就是说，不像 UDP，它是一个不会静默丢失消息的协议。[RUDP](https://en.wikipedia.org/wiki/Reliable_User_Datagram_Protocol)——作为 UDP 的可靠化升级版本——是一种合适的替代选择。

##### [2]    HTTP概述

<img src="https://mdn.mozillademos.org/files/13673/HTTP%20&amp;%20layers.png" alt="HTTP as an application layer protocol, on top of TCP (transport layer) and IP (network layer) and below the presentation layer." style="zoom: 25%;" />

**HTTP是一种能够获取如 HTML 这样的网络资源的 [protocol](https://developer.mozilla.org/en-US/docs/Glossary/protocol)(通讯协议)**。它是在 Web 上进行数据交换的基础，是一种 client-server 协议，也就是说，请求通常是由像浏览器这样的接受方发起的。一个完整的Web文档通常是由不同的子文档拼接而成的，像是文本、布局描述、图片、视频、脚本等等。

##### [3]    HTTP 的基本性质

1. **HTTP 是简单的**。虽然下一代HTTP/2协议将HTTP消息封装到了帧（frames）中，HTTP大体上还是被设计得简单易读。HTTP报文能够被人读懂，还允许简单测试，降低了门槛，对新人很友好。

2. **HTTP 是可扩展的**。在 HTTP/1.0 中出现的 [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) 让协议扩展变得非常容易。只要服务端和客户端就新 headers 达成语义一致，新功能就可以被轻松加入进来。

3. **HTTP 是无状态，有会话的**。HTTP是无状态的：在同一个连接中，两个执行成功的请求之间是没有关系的。这就带来了一个问题，用户没有办法在同一个网站中进行连续的交互，比如在一个电商网站里，用户把某个商品加入到购物车，切换一个页面后再次添加了商品，这两次添加商品的请求之间没有关联，浏览器无法知道用户最终选择了哪些商品。而使用HTTP的头部扩展，HTTP Cookies就可以解决这个问题。把Cookies添加到头部中，创建一个会话让每次请求都能共享相同的上下文信息，达成相同的状态。**注意，HTTP本质是无状态的，使用Cookies可以创建有状态的会话**。

4. **HTTP 和连接**。一个连接是由传输层来控制的，这从根本上不属于HTTP的范围。HTTP并不需要其底层的传输层协议是面向连接的，只需要它是可靠的，或不丢失消息的（至少返回错误）。在互联网中，有两个最常用的传输层协议：TCP是可靠的，而UDP不是。因此，HTTP依赖于面向连接的TCP进行消息传递，但连接并不是必须的。

   在客户端（通常指浏览器）与服务器能够交互（客户端发起请求，服务器返回响应）之前，必须在这两者间建立一个 TCP 链接，打开一个 TCP 连接需要多次往返交换消息（因此耗时）。HTTP/1.0 默认为每一对 HTTP 请求/响应都打开一个单独的 TCP 连接。当需要连续发起多个请求时，这种模式比多个请求共享同一个 TCP 链接更低效。

   为了减轻这些缺陷，HTTP/1.1引入了流水线（被证明难以实现）和持久连接的概念：底层的TCP连接可以通过[`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection)头部来被部分控制。HTTP/2则发展得更远，通过在一个连接复用消息的方式来让这个连接始终保持为暖连接。 

   为了更好的适合HTTP，设计一种更好传输协议的进程一直在进行。Google就研发了一种以UDP为基础，能提供更可靠更高效的传输协议[QUIC](https://en.wikipedia.org/wiki/QUIC)。

##### [4]    HTTP 报文

HTTP/1.1以及更早的HTTP协议报文都是语义可读的。在HTTP/2中，这些报文被嵌入到了一个新的二进制结构，帧。帧允许实现很多优化，比如报文头部的压缩和复用。即使只有原始HTTP报文的一部分以HTTP/2发送出来，每条报文的语义依旧不变，客户端会重组原始HTTP/1.1请求。因此用HTTP/1.1格式来理解HTTP/2报文仍旧有效。

有两种HTTP报文的类型，**请求与响应**，每种都有其特定的格式。

1. **请求**

   <img src="X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200511111226252.png" alt="image-20200511111226252" style="zoom:50%;" />

   请求由以下元素组成：

   - 一个HTTP的[method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)，经常是由一个动词像[`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET), [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 或者一个名词像[`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)，[`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD)来定义客户端的动作行为。通常客户端的操作都是获取资源（GET方法）或者发送[HTML form](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms)表单值（POST方法），虽然在一些情况下也会有其他操作。
   - 要获取的资源的路径，通常是上下文中就很明显的元素资源的URL，它没有[protocol](https://developer.mozilla.org/en-US/docs/Glossary/protocol)（`http://`），[domain](https://developer.mozilla.org/en-US/docs/Glossary/domain)（`developer.mozilla.org`），或是TCP的[port](https://developer.mozilla.org/en-US/docs/Glossary/port)（HTTP一般在80端口）。
   - HTTP协议版本号。
   - 为服务端表达其他信息的可选头部[headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)。
   - 对于一些像POST这样的方法，报文的body就包含了发送的资源，这与响应报文的body类似。

2. **响应**

   <img src="X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200511111332621.png" alt="image-20200511111332621" style="zoom:50%;" />

响应报文包含了下面的元素：

- HTTP协议版本号。
- 一个状态码（[status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)），来告知对应请求执行成功或失败，以及失败的原因。
- 一个状态信息，这个信息是非权威的状态码描述信息，可以由服务端自行设定。
- HTTP [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)，与请求头部类似。
- 可选项，比起请求报文，响应报文中更常见地包含获取的资源body。

##### [5]   HTTP 请求方法

HTTP 定义了一组**请求方法**, 以表明要对给定资源执行的操作。指示针对给定资源要执行的期望动作. 虽然他们也可以是名词, 但这些请求方法有时被称为HTTP动词. 

```
GET
```

GET方法请求一个指定资源的表示形式. 使用GET的请求应该只被用于获取数据.

```
HEAD
```

HEAD方法请求一个与GET请求的响应相同的响应，但没有响应体.

```
POST
```

POST方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用. 

```
PUT
```

PUT方法用请求有效载荷替换目标资源的所有当前表示。

```
DELETE
```

DELETE方法删除指定的资源。

```
CONNECT
```

CONNECT方法建立一个到由目标资源标识的服务器的隧道。

```
OPTIONS
```

OPTIONS方法用于描述目标资源的通信选项。

```
TRACE
```

TRACE方法沿着到目标资源的路径执行一个消息环回测试。

```
PATCH
```

PATCH方法用于对资源应用部分修改。

##### [6]   HTTP 响应代码

HTTP 响应状态代码指示特定 [HTTP](https://developer.mozilla.org/zh-cn/HTTP) 请求是否已成功完成。响应分为五类：信息响应(`100`–`199`)，成功响应(`200`–`299`)，重定向(`300`–`399`)，客户端错误(`400`–`499`)和服务器错误 (`500`–`599`)。状态代码由 [section 10 of RFC 2616](https://tools.ietf.org/html/rfc2616#section-10)定义。

###### 信息响应

- [`100 Continue`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/100)

  这个临时响应表明，迄今为止的所有内容都是可行的，客户端应该继续请求，如果已经完成，则忽略它。

- [`101 Switching Protocol`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/101)

  该代码是响应客户端的 [`Upgrade`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Upgrade) 标头发送的，并且指示服务器也正在切换的协议。

- [`102 Processing`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/102) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  此代码表示服务器已收到并正在处理该请求，但没有响应可用。

- [`103 Early Hints`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/103) 

  此状态代码主要用于与[`Link`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Link) 链接头一起使用，以允许用户代理在服务器仍在准备响应时开始预加载资源。

###### 成功响应

- [`200 OK`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200)

  请求成功。成功的含义取决于HTTP方法：GET：资源已被提取并在消息正文中传输。HEAD：实体标头位于消息正文中。POST：描述动作结果的资源在消息体中传输。TRACE：消息正文包含服务器收到的请求消息

- [`201 Created`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/201)

  该请求已成功，并因此创建了一个新的资源。这通常是在POST请求，或是某些PUT请求之后返回的响应。

- [`202 Accepted`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/202)

  请求已经接收到，但还未响应，没有结果。意味着不会有一个异步的响应去表明当前请求的结果，预期另外的进程和服务去处理请求，或者批处理。

- [`203 Non-Authoritative Information`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/203)

  服务器已成功处理了请求，但返回的实体头部元信息不是在原始服务器上有效的确定集合，而是来自本地或者第三方的拷贝。当前的信息可能是原始版本的子集或者超集。例如，包含资源的元数据可能导致原始服务器知道元信息的超集。使用此状态码不是必须的，而且只有在响应不使用此状态码便会返回200 OK的情况下才是合适的。

- [`204 No Content`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/204)

  服务器成功处理了请求，但不需要返回任何实体内容，并且希望返回更新了的元信息。响应可能通过实体头部的形式，返回新的或更新后的元信息。如果存在这些头部信息，则应当与所请求的变量相呼应。如果客户端是浏览器的话，那么用户浏览器应保留发送了该请求的页面，而不产生任何文档视图上的变化，即使按照规范新的或更新后的元信息应当被应用到用户浏览器活动视图中的文档。由于204响应被禁止包含任何消息体，因此它始终以消息头后的第一个空行结尾。

- [`205 Reset Content`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/205)

  服务器成功处理了请求，且没有返回任何内容。但是与204响应不同，返回此状态码的响应要求请求者重置文档视图。该响应主要是被用于接受用户输入后，立即重置表单，以便用户能够轻松地开始另一次输入。与204响应一样，该响应也被禁止包含任何消息体，且以消息头后的第一个空行结束。

- [`206 Partial Content`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/206)

  服务器已经成功处理了部分 GET 请求。类似于 FlashGet 或者迅雷这类的 HTTP 下载工具都是使用此类响应实现断点续传或者将一个大文档分解为多个下载段同时下载。该请求必须包含 Range 头信息来指示客户端希望得到的内容范围，并且可能包含 If-Range 来作为请求条件。

- [`207 Multi-Status`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/207) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  由WebDAV(RFC 2518)扩展的状态码，代表之后的消息体将是一个XML消息，并且可能依照之前子请求数量的不同，包含一系列独立的响应代码。

- [`208 Already Reported`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/208) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  在 DAV 里面使用: propstat 响应元素以避免重复枚举多个绑定的内部成员到同一个集合。

- [`226 IM Used`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/226) ([HTTP Delta encoding](https://tools.ietf.org/html/rfc3229))

  服务器已经完成了对资源的 GET 请求，并且响应是对当前实例应用的一个或多个实例操作结果的表示。

###### 重定向

- [`300 Multiple Choice`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/300)

  被请求的资源有一系列可供选择的回馈信息，每个都有自己特定的地址和浏览器驱动的商议信息。用户或浏览器能够自行选择一个首选的地址进行重定向。

- [`301 Moved Permanently`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/301)

  被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个 URI 之一。如果可能，拥有链接编辑功能的客户端应当自动把请求的地址修改为从服务器反馈回来的地址。除非额外指定，否则这个响应也是可缓存的。

- [`302 Found`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/302)

  请求的资源现在临时从不同的 URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。

- [`303 See Other`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/303)

  对应当前请求的响应可以在另一个 URI 上被找到，而且客户端应当采用 GET 的方式访问那个资源。这个方法的存在主要是为了允许由脚本激活的POST请求输出重定向到一个新的资源。

- [`304 Not Modified`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/304)

  如果客户端发送了一个带条件的 GET 请求且该请求已被允许，而文档的内容（自上次访问以来或者根据请求的条件）并没有改变，则服务器应当返回这个状态码。304 响应禁止包含消息体，因此始终以消息头后的第一个空行结尾。

- `305 Use Proxy` 

  被请求的资源必须通过指定的代理才能被访问。Location 域中将给出指定的代理所在的 URI 信息，接收者需要重复发送一个单独的请求，通过这个代理才能访问相应资源。只有原始服务器才能建立305响应。

- `306 unused`

  在最新版的规范中，306 状态码已经不再被使用。

- [`307 Temporary Redirect`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/307)

  请求的资源现在临时从不同的URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。只有在Cache-Control或Expires中进行了指定的情况下，这个响应才是可缓存的。

- [`308 Permanent Redirect`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/308)

  这意味着资源现在永久位于由 `Location:` HTTP Response 标头指定的另一个 URI。 这与 `301 Moved Permanently HTTP` 响应代码具有相同的语义，但用户代理不能更改所使用的 HTTP 方法：如果在第一个请求中使用 `POST`，则必须在第二个请求中使用 `POST`。

###### 客户端响应

- [`400 Bad Request`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/400)

  1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。

  2、请求参数有误。

- [`401 Unauthorized`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/401)

  当前请求需要用户验证。该响应必须包含一个适用于被请求资源的 WWW-Authenticate 信息头用以询问用户信息。客户端可以重复提交一个包含恰当的 Authorization 头信息的请求。如果当前请求已经包含了 Authorization 证书，那么401响应代表着服务器验证已经拒绝了那些证书。如果401响应包含了与前一个响应相同的身份验证询问，且浏览器已经至少尝试了一次验证，那么浏览器应当向用户展示响应中包含的实体信息，因为这个实体信息中可能包含了相关诊断信息。

- `402 Payment Required`

  此响应码保留以便将来使用，创造此响应码的最初目的是用于数字支付系统，然而现在并未使用。

- [`403 Forbidden`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/403)

  服务器已经理解请求，但是拒绝执行它。与 401 响应不同的是，身份验证并不能提供任何帮助，而且这个请求也不应该被重复提交。如果这不是一个 HEAD 请求，而且服务器希望能够讲清楚为何请求不能被执行，那么就应该在实体内描述拒绝的原因。当然服务器也可以返回一个 404 响应，假如它不希望让客户端获得任何信息。

- [`404 Not Found`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/404)

  请求失败，请求所希望得到的资源未被在服务器上发现。没有信息能够告诉用户这个状况到底是暂时的还是永久的。假如服务器知道情况的话，应当使用410状态码来告知旧资源因为某些内部的配置机制问题，已经永久的不可用，而且没有任何可以跳转的地址。404这个状态码被广泛应用于当服务器不想揭示到底为何请求被拒绝或者没有其他适合的响应可用的情况下。

- [`405 Method Not Allowed`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/405)

  请求行中指定的请求方法不能被用于请求相应的资源。该响应必须返回一个Allow 头信息用以表示出当前资源能够接受的请求方法的列表。 鉴于 PUT，DELETE 方法会对服务器上的资源进行写操作，因而绝大部分的网页服务器都不支持或者在默认配置下不允许上述请求方法，对于此类请求均会返回405错误。

- [`406 Not Acceptable`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/406)

  请求的资源的内容特性无法满足请求头中的条件，因而无法生成响应实体。

- [`407 Proxy Authentication Required`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/407)

  与401响应类似，只不过客户端必须在代理服务器上进行身份验证。代理服务器必须返回一个 Proxy-Authenticate 用以进行身份询问。客户端可以返回一个 Proxy-Authorization 信息头用以验证。

- [`408 Request Timeout`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/408)

  请求超时。客户端没有在服务器预备等待的时间内完成一个请求的发送。客户端可以随时再次提交这一请求而无需进行任何更改。

- [`409 Conflict`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/409)

  由于和被请求的资源的当前状态之间存在冲突，请求无法完成。这个代码只允许用在这样的情况下才能被使用：用户被认为能够解决冲突，并且会重新提交新的请求。该响应应当包含足够的信息以便用户发现冲突的源头。

- [`410 Gone`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/410)

  被请求的资源在服务器上已经不再可用，而且没有任何已知的转发地址。这样的状况应当被认为是永久性的。如果可能，拥有链接编辑功能的客户端应当在获得用户许可后删除所有指向这个地址的引用。如果服务器不知道或者无法确定这个状况是否是永久的，那么就应该使用 404 状态码。除非额外说明，否则这个响应是可缓存的。

- [`411 Length Required`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/411)

  服务器拒绝在没有定义 `Content-Length` 头的情况下接受请求。在添加了表明请求消息体长度的有效 `Content-Length` 头之后，客户端可以再次提交该请求。

- [`412 Precondition Failed`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/412)

  服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。这个状态码允许客户端在获取资源时在请求的元信息（请求头字段数据）中设置先决条件，以此避免该请求方法被应用到其希望的内容以外的资源上。

- [`413 Payload Too Large`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/413)

  服务器拒绝处理当前请求，因为该请求提交的实体数据大小超过了服务器愿意或者能够处理的范围。此种情况下，服务器可以关闭连接以免客户端继续发送此请求。如果这个状况是临时的，服务器应当返回一个 `Retry-After` 的响应头，以告知客户端可以在多少时间以后重新尝试。

- [`414 URI Too Long`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/414)

  请求的URI 长度超过了服务器能够解释的长度，因此服务器拒绝对该请求提供服务。这比较少见，通常的情况包括：本应使用POST方法的表单提交变成了GET方法，导致查询字符串（Query String）过长。

- [`415 Unsupported Media Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/415)

  对于当前请求的方法和所请求的资源，请求中提交的实体并不是服务器中所支持的格式，因此请求被拒绝。

- [`416 Range Not Satisfiable`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/416)

  如果请求中包含了 Range 请求头，并且 Range 中指定的任何数据范围都与当前资源的可用范围不重合，同时请求中又没有定义 If-Range 请求头，那么服务器就应当返回416状态码。

- [`417 Expectation Failed`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/417)

  此响应代码意味着服务器无法满足 [`Expect`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expect) 请求标头字段指示的期望值。

- [`418 I'm a teapot`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/418)

  服务器拒绝尝试用 `“茶壶冲泡咖啡”`。

- [`421 Misdirected Request`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/421)

  该请求针对的是无法产生响应的服务器。 这可以由服务器发送，该服务器未配置为针对包含在请求 URI 中的方案和权限的组合产生响应。

- [`422 Unprocessable Entity`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/422) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  请求格式良好，但由于语义错误而无法遵循。

- [`423 Locked`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/423) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  正在访问的资源被锁定。

- [`424 Failed Dependency`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/424) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  由于先前的请求失败，所以此次请求失败。

- [`425 Too Early`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/425)

  服务器不愿意冒着风险去处理可能重播的请求。

- [`426 Upgrade Required`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/426)

  服务器拒绝使用当前协议执行请求，但可能在客户机升级到其他协议后愿意这样做。 服务器在 426 响应中发送 [`Upgrade`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Upgrade) 头以指示所需的协议。

- [`428 Precondition Required`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/428)

  原始服务器要求该请求是有条件的。 旨在防止“丢失更新”问题，即客户端获取资源状态，修改该状态并将其返回服务器，同时第三方修改服务器上的状态，从而导致冲突。

- [`429 Too Many Requests`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/429)

  用户在给定的时间内发送了太多请求（“限制请求速率”）。

- [`431 Request Header Fields Too Large`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/431)

  服务器不愿意处理请求，因为它的 请求头字段太大（ Request Header Fields Too Large）。 请求可以在减小请求头字段的大小后重新提交。

- [`451 Unavailable For Legal Reasons`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/451)

  用户请求非法资源，例如：由政府审查的网页。

###### 服务端响应

- [`500 Internal Server Error`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/500)

  服务器遇到了不知道如何处理的情况。

- [`501 Not Implemented`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/501)

  此请求方法不被服务器支持且无法被处理。只有`GET`和`HEAD`是要求服务器支持的，它们必定不会返回此错误代码。

- [`502 Bad Gateway`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/502)

  此错误响应表明服务器作为网关需要得到一个处理这个请求的响应，但是得到一个错误的响应。

- [`503 Service Unavailable`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/503)

  服务器没有准备好处理请求。 常见原因是服务器因维护或重载而停机。 请注意，与此响应一起，应发送解释问题的用户友好页面。 这个响应应该用于临时条件和 `Retry-After`：如果可能的话，HTTP头应该包含恢复服务之前的估计时间。 网站管理员还必须注意与此响应一起发送的与缓存相关的标头，因为这些临时条件响应通常不应被缓存。

- [`504 Gateway Timeout`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/504)

  当服务器作为网关，不能及时得到响应时返回此错误代码。

- [`505 HTTP Version Not Supported`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/505)

  服务器不支持请求中所使用的HTTP协议版本。

- [`506 Variant Also Negotiates`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/506)

  服务器有一个内部配置错误：对请求的透明内容协商导致循环引用。

- [`507 Insufficient Storage`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/507)

  服务器有内部配置错误：所选的变体资源被配置为参与透明内容协商本身，因此不是协商过程中的适当端点。

- [`508 Loop Detected`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/508) ([WebDAV](https://developer.mozilla.org/en-US/docs/Glossary/WebDAV))

  服务器在处理请求时检测到无限循环。

- [`510 Not Extended`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/510)

  客户端需要对请求进一步扩展，服务器才能实现它。服务器会回复客户端发出扩展请求所需的所有信息。

- [`511 Network Authentication Required`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/511)

  511 状态码指示客户端需要进行身份验证才能获得网络访问权限。

##### [7]   谈下 HTTP 1.0 和 1.1、2.0的主要变化？

1. HTTP1.0 经过多年发展，在 1.1 提出了改进。首先是提出了**长连接**，HTTP 可以在一次 TCP 连接中不断发送请求。
2. HTTP2.0 支持**多路复用**，同一个连接可以并发处理多个请求，方法是把 HTTP数据包拆为多个帧，并发有序的发送，根据序号在另一端进行重组，而不需要一个个 HTTP请求顺序到达；

##### [8]   讲一下HTTP和HTTPS协议的区别？

* **HTTP数据时明文传输，HTTPS是密文传输**；

* HTTPS一般是要需要区证书，是收费的；

* HTTP默认使用80端口，HTTPS默认使用443端口。

##### [9]  HTTP中的Get，Post，Put具体指什么？ 

**答：它们都是HTTP中的请求方法：**

1. GET(获得)方法：对这个资源的查操作。
2. PUT(改)和POST（更新）都有更改指定URI的语义。

>【拓展问题！！！！！！！！！！！！！！！！！！！】
>
>1. **Put和Post 请求有什么区别？**
>
>>**Put请求：如果两个请求相同，后一个请求会把第一个请求覆盖掉**。（所以PUT用来改资源）
>>**Post请求：后一个请求不会把第一个请求覆盖掉**。（所以Post用来增资源）
>
>2. **GET和Post 请求有什么区别？**
>
>>1. Get一般用来从服务器上**查询信息**；Post一般用来**插入信息**；对于服务器讲：get是安全(不更改信息)、幂等(作用1次和n次效果相同); post不安全、不幂等;  ，get是安全(不更改信息)、幂等(作用1次和n次效果相同); post不安全、不幂等;   
>>
>>2. 对于客户端来讲：Get方法将参数直接拼接在了URL后边，**明文显示**，可以通过浏览器地址栏直接访问；Post请求用于提交表单，**数据不是明文的**，安全性更高；
>>3. Get请求**有长度限制**，Post请求**没有长度限制**。
>
>https://www.zhihu.com/question/28586791?sort=created

#### 22. HttpServletResponse

Web服务器收到客户端的http请求，会针对每一次请求，分别创建一个用于代表请求的request对象、和代表响应的response对象。获取网页提交过来的数据，只需要找request对象就行了。要向网页输出数据，只需要找response对象。

一，HttpServletResponse对象介绍

![img](https://upload-images.jianshu.io/upload_images/5824016-59d786bc8f7eda6e.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/1048/format/webp)

https://www.jianshu.com/p/8bc6b82403c5

#### 23. [springboot之使用redistemplate优雅地操作redis](https://www.cnblogs.com/superfj/p/9232482.html)

https://www.cnblogs.com/superfj/p/9232482.html

1. 连接池自动管理，提供了一个高度封装的“RedisTemplate”类
2. 针对jedis客户端中大量api进行了归类封装,将同一类型操作封装为operation接口

ValueOperations：简单K-V操作 

SetOperations：set类型数据操作

 ZSetOperations：zset类型数据操作

 HashOperations：针对map类型的数据操作

 ListOperations：针对list类型的数据操作

3. 提供了对key的“bound”(绑定)便捷化操作API，可以通过bound封装指定的key，然后进行一系列的操作而无须“显式”的再次指定Key，即BoundKeyOperations：

   BoundValueOperations 

   BoundSetOperations 

   BoundListOperations 

   BoundSetOperations 

   BoundHashOperations

   4. 将事务操作封装，有容器控制。

   5. 针对数据的“**序列化/反序列化**”，提供了多种可选择策略(RedisSerializer) JdkSerializationRedisSerializer：POJO对象的存取场景，使用JDK本身序列化机制，将pojo类通过ObjectInputStream/ObjectOutputStream进行序列化操作，最终redis-server中将存储字节序列。是目前最常用的序列化策略。

      StringRedisSerializer：Key或者value为字符串的场景，根据指定的charset对数据的字节序列编码成string，是“new String(bytes, charset)”和“string.getBytes(charset)”的直接封装。是最轻量级和高效的策略。

       JacksonJsonRedisSerializer：jackson-json工具提供了javabean与json之间的转换能力，可以将pojo实例序列化成json格式存储在redis中，也可以将json格式的数据转换成pojo实例。因为jackson工具在序列化和反序列化时，需要明确指定Class类型，因此此策略封装起来稍微复杂。【需要jackson-mapper-asl工具支持】

       OxmSerializer：提供了将javabean与xml之间的转换能力，目前可用的三方支持包括jaxb，apache-xmlbeans；redis存储的数据将是xml工具。不过使用此策略，编程将会有些难度，而且效率最低；不建议使用。【需要spring-oxm模块的支持】

   #### 24. kafka和zeekpoor

   https://www.cnblogs.com/qingyunzong/category/1212387.html

   #### 25. kafkaTemplate

## 第二章 开发社区首页

分步实现：

* 开发社区首页
* 显示前10个帖子

<img src="X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200530085731962.png" alt="image-20200530085731962" style="zoom: 67%;" />

1. 使用数据库建表

~~~sql
CREATE TABLE `discuss_post` (
  `id` int(11) NOT NULL AUTO_INCREMENT,'自增长的主键;'
  `user_id` varchar(45) DEFAULT NULL,'发布的用户;'
  `title` varchar(100) DEFAULT NULL,'帖子的标题;'
  `content` text,'帖子的内容;'
  `type` int(11) DEFAULT NULL COMMENT '0-普通; 1-置顶;',
  `status` int(11) DEFAULT NULL COMMENT '0-正常; 1-精华; 2-拉黑;',
  `create_time` timestamp NULL DEFAULT NULL,'帖子发布的时间;'
  `comment_count` int(11) DEFAULT NULL,'帖子评论的数量;'
  `score` double DEFAULT NULL,'帖子的分数;'
  PRIMARY KEY (`id`),'id为主键;'
  KEY `index_user_id` (`user_id`)
) ENGINE=InnoDB AUTO_INCREMENT=281 DEFAULT CHARSET=utf8
~~~

2. 开发dao数据访问层

   * 需要使用驼峰式命名方式

   **创建entity实体类：**

~~~java
package com.nowcoder.community.entity;

import java.util.Date;

public class DiscussPost {

    private int id;
    private int userId;
    private String title;
    private String content;
    private int type;
    private int status;
    private Date createTime;
    private int commentCount;
    private double score;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getUserId() {
        return userId;
    }

    public void setUserId(int userId) {
        this.userId = userId;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getContent() {
        return content;
    }

    public void setContent(String content) {
        this.content = content;
    }

    public int getType() {
        return type;
    }

    public void setType(int type) {
        this.type = type;
    }

    public int getStatus() {
        return status;
    }

    public void setStatus(int status) {
        this.status = status;
    }

    public Date getCreateTime() {
        return createTime;
    }

    public void setCreateTime(Date createTime) {
        this.createTime = createTime;
    }

    public int getCommentCount() {
        return commentCount;
    }

    public void setCommentCount(int commentCount) {
        this.commentCount = commentCount;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }

    @Override
    public String toString() {
        return "DiscussPost{" +
                "id=" + id +
                ", userId=" + userId +
                ", title='" + title + '\'' +
                ", content='" + content + '\'' +
                ", type=" + type +
                ", status=" + status +
                ", createTime=" + createTime +
                ", commentCount=" + commentCount +
                ", score=" + score +
                '}';
    }
}
~~~

**创建entity实体类：**

在dao创建访问数据层的接口DiscussPostMapper

~~~java
package com.nowcoder.community.dao;

import com.nowcoder.community.entity.DiscussPost;
import org.apache.ibatis.annotations.Mapper;
import org.apache.ibatis.annotations.Param;

import java.util.List;

@Mapper
public interface DiscussPostMapper {
    // DiscussPost就是帖子相关的实体类
    // userId是用户的id，如果userId等于0，则不会传入数据。若不等于0，则会传入。
    //offset每一页起始行号，每一页最多显示的数据
    List<DiscussPost> selectDiscussPosts(int userId, int offset, int limit);//查询帖子

    // @Param注解用于给参数取别名,
    // 如果只有一个参数,并且在<if>里使用,则必须加别名.
    int selectDiscussPostRows(@Param("userId") int userId);//查询并统计帖子的数量
}

~~~

3. 实现dao中的接口
   * DiscussPost不是java自带的类型需要声明。
   * user_id = #{userId}；前面为字段，后面为方法的参数
   * order by type desc, create_time desc，帖子的类型倒序，时间倒序
   * limit #{offset}, #{limit} 分页

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.nowcoder.community.dao.DiscussPostMapper">

    <sql id="selectFields">
        id, user_id, title, content, type, status, create_time, comment_count, score
    </sql>

    <select id="selectDiscussPosts" resultType="DiscussPost">
        select <include refid="selectFields"></include>
        from discuss_post
        where status != 2
        <if test="userId!=0">
            and user_id = #{userId}
        </if>
        order by type desc, create_time desc
        limit #{offset}, #{limit}
    </select>

    <select id="selectDiscussPostRows" resultType="int">
        select count(id)-
        from discuss_post
        where status != 2
        <if test="userId!=0">
            and user_id = #{userId}
        </if>
    </select>

</mapper>
~~~

test

~~~java
    @Test
    public void testSelectPosts() {
        List<DiscussPost> list = discussPostMapper.selectDiscussPosts(149, 0, 10);
        for(DiscussPost post : list) {
            System.out.println(post);
        }

        int rows = discussPostMapper.selectDiscussPostRows(149);
        System.out.println(rows);
    }

}
~~~

3. 开发service层调用dao层

service层 https://blog.csdn.net/qq_36654606/article/details/86618708

Service，就是 Servlet 和 Dao 层之间缓冲的层。通过这一层来进行解耦，使得 Dao 层内的变化不会直接影响到 Servlet 层。

**解耦**：也就是在逻辑上不同的层之间，某一层的改变尽量不要去影响到其他层。
而Servlet层和Dao层都是由实际意义代码操作的层，而且两层有直接的调用关系。如果 Dao 层代码修改了，会直接影响到 Servlet 层。为了尽量的减少这两层之间的耦合，加了这么一个层
核心词2
**便于扩展**：这是个很常见的词了。也就是说一份代码在初次写的时候，一手的开发者对代码的理解是相对比较充分的，但是，功能可能不是非常完善的。那么在之后修改代码的某一部分功能的时候，也是需要尽量这个功能对外仅仅提供一个调用，功能内部的代码如何实现不需要被其他部分了解。那么 Service 层作为一个中间站，就比较好的解决了这个问题。

~~~java
package com.nowcoder.community.service;

import com.nowcoder.community.dao.DiscussPostMapper;
import com.nowcoder.community.entity.DiscussPost;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class DiscussPostService {

    @Autowired
    private DiscussPostMapper discussPostMapper;

    public List<DiscussPost> findDiscussPosts(int userId, int offset, int limit) {
        return discussPostMapper.selectDiscussPosts(userId, offset, limit);
    }

    public int findDiscussPostRows(int userId) {
        return discussPostMapper.selectDiscussPostRows(userId);
    }

}

~~~

~~~java
package com.nowcoder.community.service;

import com.nowcoder.community.dao.UserMapper;
import com.nowcoder.community.entity.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;
   //由于帖子内要显示用户名称，所以通过id查询user的数据
    public User findUserById(int id) {
        return userMapper.selectById(id);
    }

}
~~~

4. 开发Controller层

~~~java
package com.nowcoder.community.controller;

import com.nowcoder.community.entity.DiscussPost;
import com.nowcoder.community.entity.Page;
import com.nowcoder.community.entity.User;
import com.nowcoder.community.service.DiscussPostService;
import com.nowcoder.community.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

@Controller
public class HomeController {

    @Autowired
    private DiscussPostService discussPostService;

    @Autowired
    private UserService userService;


    @RequestMapping(path = "/index", method = RequestMethod.GET)
    public String getIndexPage(Model model, Page page) {
        // 方法调用钱,SpringMVC会自动实例化Model和Page,并将Page注入Model.
        // 所以,在thymeleaf中可以直接访问Page对象中的数据.
        page.setRows(discussPostService.findDiscussPostRows(0));
        page.setPath("/index");

        List<DiscussPost> list = discussPostService.findDiscussPosts(0, page.getOffset(), page.getLimit());
        List<Map<String, Object>> discussPosts = new ArrayList<>();
        if (list != null) {
            for (DiscussPost post : list) {
                Map<String, Object> map = new HashMap<>();
                map.put("post", post);
                User user = userService.findUserById(post.getUserId());
                map.put("user", user);
                discussPosts.add(map);
            }
        }
        model.addAttribute("discussPosts", discussPosts);
        return "/index";
    }

}
~~~

~~~java
package com.nowcoder.community.entity;

/**
 * 封装分页相关的信息.
 */
public class Page {

    // 当前页码
    private int current = 1;
    // 显示上限
    private int limit = 10;
    // 数据总数(用于计算总页数)
    private int rows;
    // 查询路径(用于复用分页链接)
    private String path;

    public int getCurrent() {
        return current;
    }

    public void setCurrent(int current) {
        if (current >= 1) {
            this.current = current;
        }
    }

    public int getLimit() {
        return limit;
    }

    public void setLimit(int limit) {
        if (limit >= 1 && limit <= 100) {
            this.limit = limit;
        }
    }

    public int getRows() {
        return rows;
    }

    public void setRows(int rows) {
        if (rows >= 0) {
            this.rows = rows;
        }
    }

    public String getPath() {
        return path;
    }

    public void setPath(String path) {
        this.path = path;
    }

    /**
     * 获取当前页的起始行
     *
     * @return
     */
    public int getOffset() {
        // current * limit - limit
        return (current - 1) * limit;
    }

    /**
     * 获取总页数
     *
     * @return
     */
    public int getTotal() {
        // rows / limit [+1]
        if (rows % limit == 0) {
            return rows / limit;
        } else {
            return rows / limit + 1;
        }
    }

    /**
     * 获取起始页码
     *
     * @return
     */
    public int getFrom() {
        int from = current - 2;
        return from < 1 ? 1 : from;
    }

    /**
     * 获取结束页码
     *
     * @return
     */
    public int getTo() {
        int to = current + 2;
        int total = getTotal();
        return to > total ? total : to;
    }

}

~~~

5. html文件

~~~java
<!doctype html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
	<link rel="icon" href="https://static.nowcoder.com/images/logo_87_87.png"/>
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" crossorigin="anonymous">
	<link rel="stylesheet" th:href="@{/css/global.css}" />
	<title>牛客网-首页</title>
</head>
<body>	
	<div class="nk-container">
		<!-- 头部 -->
		<header class="bg-dark sticky-top">
			<div class="container">
				<!-- 导航 -->
				<nav class="navbar navbar-expand-lg navbar-dark">
					<!-- logo -->
					<a class="navbar-brand" href="#"></a>
					<button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
						<span class="navbar-toggler-icon"></span>
					</button>
					<!-- 功能 -->
					<div class="collapse navbar-collapse" id="navbarSupportedContent">
						<ul class="navbar-nav mr-auto">
							<li class="nav-item ml-3 btn-group-vertical">
								<a class="nav-link" href="index.html">首页</a>
							</li>
							<li class="nav-item ml-3 btn-group-vertical">
								<a class="nav-link position-relative" href="site/letter.html">消息<span class="badge badge-danger">12</span></a>
							</li>
							<li class="nav-item ml-3 btn-group-vertical">
								<a class="nav-link" href="site/register.html">注册</a>
							</li>
							<li class="nav-item ml-3 btn-group-vertical">
								<a class="nav-link" href="site/login.html">登录</a>
							</li>
							<li class="nav-item ml-3 btn-group-vertical dropdown">
								<a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
									<img src="http://images.nowcoder.com/head/1t.png" class="rounded-circle" style="width:30px;"/>
								</a>
								<div class="dropdown-menu" aria-labelledby="navbarDropdown">
									<a class="dropdown-item text-center" href="site/profile.html">个人主页</a>
									<a class="dropdown-item text-center" href="site/setting.html">账号设置</a>
									<a class="dropdown-item text-center" href="site/login.html">退出登录</a>
									<div class="dropdown-divider"></div>
									<span class="dropdown-item text-center text-secondary">nowcoder</span>
								</div>
							</li>
						</ul>
						<!-- 搜索 -->
						<form class="form-inline my-2 my-lg-0" action="site/search.html">
							<input class="form-control mr-sm-2" type="search" aria-label="Search" />
							<button class="btn btn-outline-light my-2 my-sm-0" type="submit">搜索</button>
						</form>
					</div>
				</nav>
			</div>
		</header>

		<!-- 内容 -->
		<div class="main">
			<div class="container">
				<div class="position-relative">
					<!-- 筛选条件 -->
					<ul class="nav nav-tabs mb-3">
						<li class="nav-item">
							<a class="nav-link active" href="#">最新</a>
						</li>
						<li class="nav-item">
							<a class="nav-link" href="#">最热</a>
						</li>
					</ul>
					<button type="button" class="btn btn-primary btn-sm position-absolute rt-0" data-toggle="modal" data-target="#publishModal">我要发布</button>
				</div>
				<!-- 弹出框 -->
				<div class="modal fade" id="publishModal" tabindex="-1" role="dialog" aria-labelledby="publishModalLabel" aria-hidden="true">
					<div class="modal-dialog modal-lg" role="document">
						<div class="modal-content">
							<div class="modal-header">
								<h5 class="modal-title" id="publishModalLabel">新帖发布</h5>
								<button type="button" class="close" data-dismiss="modal" aria-label="Close">
									<span aria-hidden="true">&times;</span>
								</button>
							</div>
							<div class="modal-body">
								<form>
									<div class="form-group">
										<label for="recipient-name" class="col-form-label">标题：</label>
										<input type="text" class="form-control" id="recipient-name">
									</div>
									<div class="form-group">
										<label for="message-text" class="col-form-label">正文：</label>
										<textarea class="form-control" id="message-text" rows="15"></textarea>
									</div>
								</form>
							</div>
							<div class="modal-footer">
								<button type="button" class="btn btn-secondary" data-dismiss="modal">取消</button>
								<button type="button" class="btn btn-primary" id="publishBtn">发布</button>
							</div>
						</div>
					</div>
				</div>
				<!-- 提示框 -->
				<div class="modal fade" id="hintModal" tabindex="-1" role="dialog" aria-labelledby="hintModalLabel" aria-hidden="true">
					<div class="modal-dialog modal-lg" role="document">
						<div class="modal-content">
							<div class="modal-header">
								<h5 class="modal-title" id="hintModalLabel">提示</h5>
							</div>
							<div class="modal-body" id="hintBody">
								发布完毕!
							</div>
						</div>
					</div>
				</div>
				
				<!-- 帖子列表 -->
				<ul class="list-unstyled">
					<li class="media pb-3 pt-3 mb-3 border-bottom" th:each="map:${discussPosts}">
						<a href="site/profile.html">
							<img th:src="${map.user.headerUrl}" class="mr-4 rounded-circle" alt="用户头像" style="width:50px;height:50px;">
						</a>
						<div class="media-body">
							<h6 class="mt-0 mb-3">
								<a href="#" th:utext="${map.post.title}">备战春招，面试刷题跟他复习，一个月全搞定！</a>
								<span class="badge badge-secondary bg-primary" th:if="${map.post.type==1}">置顶</span>
								<span class="badge badge-secondary bg-danger" th:if="${map.post.status==1}">精华</span>
							</h6>
							<div class="text-muted font-size-12">
								<u class="mr-3" th:utext="${map.user.username}">寒江雪</u> 发布于 <b th:text="${#dates.format(map.post.createTime,'yyyy-MM-dd HH:mm:ss')}">2019-04-15 15:32:18</b>
								<ul class="d-inline float-right">
									<li class="d-inline ml-2">赞 11</li>
									<li class="d-inline ml-2">|</li>
									<li class="d-inline ml-2">回帖 7</li>
								</ul>
							</div>
						</div>						
					</li>
				</ul>
				<!-- 分页 -->
				<nav class="mt-5" th:if="${page.rows>0}">
					<ul class="pagination justify-content-center">
						<li class="page-item">
							<a class="page-link" th:href="@{${page.path}(current=1)}">首页</a>
						</li>
						<li th:class="|page-item ${page.current==1?'disabled':''}|">
							<a class="page-link" th:href="@{${page.path}(current=${page.current-1})}">上一页</a></li>
						<li th:class="|page-item ${i==page.current?'active':''}|" th:each="i:${#numbers.sequence(page.from,page.to)}">
							<a class="page-link" href="#" th:text="${i}">1</a>
						</li>
						<li th:class="|page-item ${page.current==page.total?'disabled':''}|">
							<a class="page-link" th:href="@{${page.path}(current=${page.current+1})}">下一页</a>
						</li>
						<li class="page-item">
							<a class="page-link" th:href="@{${page.path}(current=${page.total})}">末页</a>
						</li>
					</ul>
				</nav>
			</div>
		</div>

		<!-- 尾部 -->
		<footer class="bg-dark">
			<div class="container">
				<div class="row">
					<!-- 二维码 -->
					<div class="col-4 qrcode">
						<img src="https://uploadfiles.nowcoder.com/app/app_download.png" class="img-thumbnail" style="width:136px;" />
					</div>
					<!-- 公司信息 -->
					<div class="col-8 detail-info">
						<div class="row">
							<div class="col">
								<ul class="nav">
									<li class="nav-item">
										<a class="nav-link text-light" href="#">关于我们</a>
									</li>
									<li class="nav-item">
										<a class="nav-link text-light" href="#">加入我们</a>
									</li>
									<li class="nav-item">
										<a class="nav-link text-light" href="#">意见反馈</a>
									</li>
									<li class="nav-item">
										<a class="nav-link text-light" href="#">企业服务</a>
									</li>
									<li class="nav-item">
										<a class="nav-link text-light" href="#">联系我们</a>
									</li>
									<li class="nav-item">
										<a class="nav-link text-light" href="#">免责声明</a>
									</li>
									<li class="nav-item">
										<a class="nav-link text-light" href="#">友情链接</a>
									</li>
								</ul>
							</div>
						</div>
						<div class="row">
							<div class="col">
								<ul class="nav btn-group-vertical company-info">
									<li class="nav-item text-white-50">
										公司地址：北京市朝阳区大屯路东金泉时代3-2708北京牛客科技有限公司
									</li>
									<li class="nav-item text-white-50">
										联系方式：010-60728802(电话)&nbsp;&nbsp;&nbsp;&nbsp;admin@nowcoder.com
									</li>
									<li class="nav-item text-white-50">
										牛客科技©2018 All rights reserved
									</li>
									<li class="nav-item text-white-50">
										京ICP备14055008号-4 &nbsp;&nbsp;&nbsp;&nbsp;
										<img src="http://static.nowcoder.com/company/images/res/ghs.png" style="width:18px;" />
										京公网安备 11010502036488号
									</li>
								</ul>
							</div>
						</div>
					</div>
				</div>
			</div>
		</footer>
	</div>

	<script src="https://code.jquery.com/jquery-3.3.1.min.js" crossorigin="anonymous"></script>
	<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" crossorigin="anonymous"></script>
	<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" crossorigin="anonymous"></script>
	<script th:src="@{/js/global.js}"></script>
	<script th:src="@{js/index.js}"></script>
</body>
</html>
~~~

~~~java
package com.nowcoder.community.entity;

/**
 * 封装分页相关的信息.
 */
public class Page {

    // 当前页码
    private int current = 1;
    // 显示上限
    private int limit = 10;
    // 数据总数(用于计算总页数)
    private int rows;
    // 查询路径(用于复用分页链接)
    private String path;

    public int getCurrent() {
        return current;
    }

    public void setCurrent(int current) {
        if (current >= 1) {
            this.current = current;
        }
    }

    public int getLimit() {
        return limit;
    }

    public void setLimit(int limit) {
        if (limit >= 1 && limit <= 100) {
            this.limit = limit;
        }
    }

    public int getRows() {
        return rows;
    }

    public void setRows(int rows) {
        if (rows >= 0) {
            this.rows = rows;
        }
    }

    public String getPath() {
        return path;
    }

    public void setPath(String path) {
        this.path = path;
    }

    /**
     * 获取当前页的起始行
     *
     * @return
     */
    public int getOffset() {
        // current * limit - limit
        return (current - 1) * limit;
    }

    /**
     * 获取总页数
     *
     * @return
     */
    public int getTotal() {
        // rows / limit [+1]
        if (rows % limit == 0) {
            return rows / limit;
        } else {
            return rows / limit + 1;
        }
    }

    /**
     * 获取起始页码
     *
     * @return
     */
    public int getFrom() {
        int from = current - 2;
        return from < 1 ? 1 : from;
    }

    /**
     * 获取结束页码
     *
     * @return
     */
    public int getTo() {
        int to = current + 2;
        int total = getTotal();
        return to > total ? total : to;
    }

}
~~~

## 第三章 开发社区登录模块

#### 1. 发送邮件

https://www.icode9.com/content-4-653512.html

> 邮箱设置：
>
> ——启用客户端SMTP服务
>
> Spring Email：
>
> ——导入jar包
>
> ——邮箱参数配置
>
> ——使用javaMailSend发送邮件
>
> 模板引擎：
>
> ——使用Thymeleaf发送HTML邮件

1、邮箱设置
–启动客户端SMTP服务，获取授权码

2、Spring Email
–导入jar包,在pom.xml文件里加入：

~~~java
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-mail -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-mail</artifactId>
    <version>2.2.5.RELEASE</version>
</dependency>
~~~

–邮箱参数配置
在application.properties里加上：

~~~java
#MailProperties
#域名    
spring.mail.host=smtp.qq.com
#端口   
spring.mail.port=465
#邮箱的账号 
spring.mail.username=1206512593@qq.com
#邮箱的授权码,如果出现535错误，记住看这里
spring.mail.password=vwidchrhujsbgjgd
#邮箱的协议
spring.mail.protocal=smtps
#采用smtp安全连接
spring.mail.properties.mail.smtp.ssl.enable=true 
~~~

–使用JavaMailSender发送邮件

JavaMailSender类：

~~~java
public interface JavaMailSender extends MailSender {
    MimeMessage createMimeMessage();
    MimeMessage createMimeMessage(InputStream var1) throws MailException;

    void send(MimeMessage var1) throws MailException;
    void send(MimeMessage... var1) throws MailException;
    void send(MimeMessagePreparator var1) throws MailException;
    void send(MimeMessagePreparator... var1) throws MailException;

}
~~~


可以看到 这个类主要作用是创建一个MimeMessage对象（表示邮件主体），然后通过send（）将这个对象发送出去

利用Spring提供的MimeMessageHelper可以很快的将邮件主体构建好



~~~java
/*
编写MailSender方法实现发邮件功能：
send方法，实现发邮件的逻辑，不需要返回值，不报错就表示成功
 @param to         发送目标
 @param subject    邮件标题
      @param content    邮件内容
          */

@Component
public class MailClient {
//记录日志
    private static final Logger logger = LoggerFactory.getLogger(MailClient.class);
    //注入JavaMailSender接口
    @Autowired
    private JavaMailSender mailSender;
    //把application.properties内的spring.mail.username注入到value
    @Value("${spring.mail.username}")
    private String from;
   //发送目标，发送标题，发送内容
    public void sendMail(String to, String subject, String content) {
        try {
            //通过createMimeMessage()对象构建一个模版，还需要往里面填充内容
            MimeMessage message = mailSender.createMimeMessage();
            MimeMessageHelper helper = new MimeMessageHelper(message);
            //message
            //发件人
            helper.setFrom(from);
             //收件人
            helper.setTo(to);
            //主题
            helper.setSubject(subject);
             //内容
            helper.setText(content, true);//true表示允许支持Html文本
            //发送邮件
            mailSender.send(helper.getMimeMessage());
        } catch (MessagingException e) {
            logger.error("发送邮件失败:" + e.getMessage());
        }
    }

}
~~~

测试类：

    @Test
        public void testTextMail(){
            mailClient.sendMail("904759374@qq.com","Test","welcome");
        }

3. 模版引擎

   –使用Thymeleaf发送HTML邮件,里面有动态参数：

<p>欢迎你, <span style="color:red;" th:text="${username}"></span>!</p>

在templates/resoueces/mail文件夹下新建HTML模版文件

4. 测试方法详情：

   @RunWith(SpringRunner.class)
   @SpringBootTest
   @ContextConfiguration(classes = CommunityApplication.class)
   public class MailTests {

       @Autowired
       private MailClient mailClient;

   //然后在测试类注入模版引擎
       @Autowired
       private TemplateEngine templateEngine;

       @Test
       public void testTextMail() {
           mailClient.sendMail("1206512593@qq.com", "TEST", "Welcome.");
       }
       
       @Test
       public void testHtmlMail() {
       //Context对象
           Context context = new Context();
           context.setVariable("username", "sunday");
       
           String content = templateEngine.process("/mail/demo", context);
           System.out.println(content);
       
           mailClient.sendMail("1206512593@qq.com", "HTML", content);
       }

   }

#### 2. 注册功能

> 访问注册页面：
>
> ——点击链接，打开注册页面
>
> 提交注册数据：
>
> ——通过表单提交数据
>
> ——服务端验证账号是否已存在，邮箱是否已注册
>
> ——服务端发送激活邮件
>
> 激活注册账号：
>
> ——点击邮件中的链接，访问服务端的激活服务。

**访问注册页面**
点击顶部区域内的链接 打开注册页面

~~~java
    @RequestMapping(path = "/register", method = RequestMethod.GET)
    public String getRegisterPage() {
        return "/site/register";
    }
~~~

**生成随机字符串**

~~~java
public class CommunityUtil {
    // 生成随机字符串
    public static String generateUUID() {
        return UUID.randomUUID().toString().replaceAll("-", "");
    }
    // MD5加密
    // hello -> abc123def456    直接MD5容易被解密
    // hello + 3e4a8 -> abc123def456abc    //增加随机字符串，字符串越长，破解难度几何增长
    public static String md5(String key) {
        if (key==null||key.length()==0) {
            return null;
        }
        //spring自带加密方法
        return DigestUtils.md5DigestAsHex(key.getBytes());
    }
}
~~~

**提交注册数据**
通过表单提交数据

服务端验证账号是否已经存在，邮箱是否已经注册

~~~java
    public Map<String, Object> register(User user) {
        Map<String, Object> map = new HashMap<>();

        // 空值处理,直接抛出异常
        if (user == null) {
            throw new IllegalArgumentException("参数不能为空!");
        }
         // 空值处理,这里不是程序上的异常，所以不抛异常
        if (StringUtils.isBlank(user.getUsername())) {
            map.put("usernameMsg", "账号不能为空!");
            return map;
        }
        if (StringUtils.isBlank(user.getPassword())) {
            map.put("passwordMsg", "密码不能为空!");
            return map;
        }
        if (StringUtils.isBlank(user.getEmail())) {
            map.put("emailMsg", "邮箱不能为空!");
            return map;
        }

        // 验证账号
        User u = userMapper.selectByName(user.getUsername());
        if (u != null) {
            map.put("usernameMsg", "该账号已存在!");
            return map;
        }

        // 验证邮箱
        u = userMapper.selectByEmail(user.getEmail());
        if (u != null) {
            map.put("emailMsg", "该邮箱已被注册!");
            return map;
        }

        // 注册用户
        user.setSalt(CommunityUtil.generateUUID().substring(0, 5));
        user.setPassword(CommunityUtil.md5(user.getPassword() + user.getSalt()));
        user.setType(0);
        user.setStatus(0);
        user.setActivationCode(CommunityUtil.generateUUID());
        user.setHeaderUrl(String.format("http://images.nowcoder.com/head/%dt.png", new Random().nextInt(1000)));
        user.setCreateTime(new Date());
        //插入user信息
        userMapper.insertUser(user);
        // 激活邮件
        Context context = new Context();
        context.setVariable("email", user.getEmail());
        // http://localhost:8080/community/activation/101/code
        String url = domain + contextPath + "/activation/" + user.getId() + "/" + user.getActivationCode();
        context.setVariable("url", url);
        String content = templateEngine.process("/mail/activation", context);
        mailClient.sendMail(user.getEmail(), "激活账号", content);

        return map;
    }
~~~

服务端发送激活邮件

~~~java
package com.nowcoder.community.util;

public interface CommunityConstant {

    /**
     * 激活成功
     */
    int ACTIVATION_SUCCESS = 0;

    /**
     * 重复激活
     */
    int ACTIVATION_REPEAT = 1;

    /**
     * 激活失败
     */
    int ACTIVATION_FAILURE = 2;

    /**
     * 默认状态的登录凭证的超时时间
     */
    int DEFAULT_EXPIRED_SECONDS = 3600 * 12;

    /**
     * 记住状态的登录凭证超时时间
     */
    int REMEMBER_EXPIRED_SECONDS = 3600 * 24 * 100;

}
------------------------------------------------------------------------------------------------------------
       
    public int activation(int userId, String code) {
        User user = userMapper.selectById(userId);
        if (user.getStatus() == 1) {
            return ACTIVATION_REPEAT;
        } else if (user.getActivationCode().equals(code)) {
            userMapper.updateStatus(userId, 1);
            return ACTIVATION_SUCCESS;
        } else {
            return ACTIVATION_FAILURE;
        }
    }

~~~

~~~java
  @RequestMapping(path = "/register", method = RequestMethod.POST)
    public String register(Model model, User user) {
        Map<String, Object> map = userService.register(user);
        if (map == null || map.isEmpty()) {
            model.addAttribute("msg", "注册成功,我们已经向您的邮箱发送了一封激活邮件,请尽快激活!");
            model.addAttribute("target", "/index");
            return "/site/operate-result";
        } else {
            model.addAttribute("usernameMsg", map.get("usernameMsg"));
            model.addAttribute("passwordMsg", map.get("passwordMsg"));
            model.addAttribute("emailMsg", map.get("emailMsg"));
            return "/site/register";
        }
    }
~~~

**激活注册账号**
点击邮件中的链接，访问服务端的激活服务

 ~~~java
    // http://localhost:8080/community/activation/101/code
    @RequestMapping(path = "/activation/{userId}/{code}", method = RequestMethod.GET)
    public String activation(Model model, @PathVariable("userId") int userId, @PathVariable("code") String code) {
        //判断验证码正确不
        int result = userService.activation(userId, code);
        if (result == ACTIVATION_SUCCESS) {
            //想访问的模板传参数
            model.addAttribute("msg", "激活成功,您的账号已经可以正常使用了!");
            model.addAttribute("target", "/login");
        } else if (result == ACTIVATION_REPEAT) {
            model.addAttribute("msg", "无效操作,该账号已经激活过了!");
            model.addAttribute("target", "/index");
        } else {
            model.addAttribute("msg", "激活失败,您提供的激活码不正确!");
            model.addAttribute("target", "/index");
        }
        return "/site/operate-result";
    }

 ~~~

#### 3. 会话管理

>HTTP基本性质：
>
>——HTTP是简单的、可扩展的、无状态的、有会话的
>
>Cookie：
>
>——是服务器发送到浏览器，并保存在浏览器端的一块数据。
>
>——浏览器下次访问该服务器时，会自动携带该块数据，并将其发送服务器
>
>Session
>
>——是JavaEE的标准，用于在服务器端记录客户端信息。
>
>——数据存放在服务器端更加安全，但是也会增加服务端的内存压力。

 **cookie示例**

```
// cookie示例

@RequestMapping(path = "/cookie/set", method = RequestMethod.GET)
@ResponseBody
public String setCookie(HttpServletResponse response) {
    // 创建cookie
    Cookie cookie = new Cookie("code", CommunityUtil.generateUUID());
    // 设置cookie生效的范围
    cookie.setPath("/community/alpha");
    // 设置cookie的生存时间
    cookie.setMaxAge(60 * 10);
    // 发送cookie
    response.addCookie(cookie);

    return "set cookie";
}
    @RequestMapping(path = "/cookie/get", method = RequestMethod.GET)
    @ResponseBody
    public String getCookie(@CookieValue("code") String code) {
        System.out.println(code);
        return "get cookie";
    }
    // session示例

    @RequestMapping(path = "/session/set", method = RequestMethod.GET)
    @ResponseBody
    public String setSession(HttpSession session) {
        session.setAttribute("id", 1);
        session.setAttribute("name", "Test");
        return "set session";
    }

    @RequestMapping(path = "/session/get", method = RequestMethod.GET)
    @ResponseBody
    public String getSession(HttpSession session) {
        System.out.println(session.getAttribute("id"));
        System.out.println(session.getAttribute("name"));
        return "get session";
    }
```

####  4. 生成验证码

>kaptcha：
>
>——导入jar包（pom添加依赖）
>
>——编写Kaptcha配置类
>
>——生成随机字符、生成图片

~~~java
<!-- https://mvnrepository.com/artifact/com.github.penggle/kaptcha -->
<dependency>
    <groupId>com.github.penggle</groupId>
    <artifactId>kaptcha</artifactId>
    <version>2.3.2</version>
</dependency>
~~~

**编写Kaptcha配置类**
因为是一个单独的小工具SpringBoot没有单独对其做整合，所以要编写配置类注入SpringBoot

~~~java
@Configuration
public class KaptchaConfig {

    @Bean
    public Producer kaptchaProducer(){
        //配置文件
        Properties properties = new Properties();
        //图片长宽
        properties.setProperty("kaptcha.image.width","100");
        properties.setProperty("kaptcha.image.height","40");
        //字体设置
        properties.setProperty("kaptcha.textproducer.font.size","32");
        properties.setProperty("kaptcha.textproducer.font.color","0,0,0");
        //字符规格、生成几个字符、生成噪音设置
        properties.setProperty("kaptcha.textproducer.char.string","123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ");
        properties.setProperty("kaptcha.textproducer.char.length","4");
        properties.setProperty("kaptcha.textproducer.noise.impl","com.google.code.kaptcha.impl.NoNoise");

        DefaultKaptcha kaptcha = new DefaultKaptcha();
        Config config = new Config(properties);
        kaptcha.setConfig(config);
        return kaptcha;
    }

}
~~~

#### 5. 登录、退出功能

>访问登录页面
>
>登录：
>
>——验证账号、密码、验证码
>
>——成功，生成登录凭证，发送给客户端
>
>——失败，跳回登录页
>
>退出：
>
>——将登录凭证修改为失效状态
>
>——跳转首页

**登陆和退出功能**
通过LoginTicket表保存用户的状态，核心字段为ticket；

登陆成功服务器将ticket字段发送给客户端，客户端访问时，将ticket字段发送给服务器，然后服务器根据这个字段查询用户信息（id、用户状态、过期时间等）

访问登陆页面：点击登录按钮，访问登陆页面

**登陆**
验证账号、密码、验证码
成功时，生成登陆凭证，发放给客户端
失败时，跳转回登陆页面

**建表：**

~~~sql
CREATE TABLE `login_ticket` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` int(11) NOT NULL,
  `ticket` varchar(45) NOT NULL,
  `status` int(11) DEFAULT '0' COMMENT '0-有效; 1-无效;',
  `expired` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `index_ticket` (`ticket`(20))
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8
~~~

**Dao层的登陆函数：**

~~~java
package com.nowcoder.community.dao;

import com.nowcoder.community.entity.LoginTicket;
import org.apache.ibatis.annotations.*;

@Mapper
public interface LoginTicketMapper {
//插入登录状态
    @Insert({
            "insert into login_ticket(user_id,ticket,status,expired) ",
            "values(#{userId},#{ticket},#{status},#{expired})"
    })
    @Options(useGeneratedKeys = true, keyProperty = "id")
    int insertLoginTicket(LoginTicket loginTicket);
//插入登录状态
    @Select({
            "select id,user_id,ticket,status,expired ",
            "from login_ticket where ticket=#{ticket}"
    })
    LoginTicket selectByTicket(String ticket);

    @Update({
            "<script>",
            "update login_ticket set status=#{status} where ticket=#{ticket} ",
            "<if test=\"ticket!=null\"> ",
            "and 1=1 ",
            "</if>",
            "</script>"
    })
    int updateStatus(String ticket, int status);

}
~~~

**Service层的登陆函数：**
**Controller层传递三个参数：名字、密码、有效期（根据用户是否选择“记住密码判断”）**
1、首先进行空值判断，如果错误将结果存入Map
2、对信息合法性进行验证：用户名不存在、密码错误、账号未激活，如果错误将结果存入Map
3、生成LoginTicket对象调用Dao存入数据库
4、将ticket存入Map返回给客户端保存

```java
   public Map<String,Object> login(String username,String password,int expiredSeconds){
        //map存的是执行结果信息
        Map<String,Object> map = new HashMap<>();
   //空值处理
    if(StringUtils.isBlank(username)){
        map.put("usernameMsg","账号不能为空！");
        return map;
    }
    if(StringUtils.isBlank(password)){
        map.put("passwordMsg","密码不能为空！");
        return map;
    }
    //对合法性进行验证
    //验证账号：
    User user = userMapper.selectByName(username);
    if(user == null){
        map.put("usernameMsg","该账号不存在！");
        return map;
    }

    //判断是否激活账号
    if(user.getStatus() == 0){
        map.put("usernameMsg","该账号未被激活！");
        return map;
    }
    //判断密码是否正确
    password = CommunityUtil.md5(password+user.getSalt()); //加密密码
    if(user.getPassword()!=password){
        map.put("passwordMsg","密码输入错误！");
        return map;
    }
    //生成登陆凭证
    LoginTicket loginTicket = new LoginTicket();
    loginTicket.setUserId(user.getId());
    loginTicket.setTicket(CommunityUtil.generateUUID());
    loginTicket.setStatus(0);
    loginTicket.setExpired(new Date(System.currentTimeMillis()+expiredSeconds*1000));
    loginTicketMapper.insertLoginTicket(loginTicket);

    //将ticket返回给客户端保存
    map.put("ticket",loginTicket.getTicket());

    return map;
}
```

**Controller层的Login函数**
1、在表现层就第一步就进行验证码验证。session里面存了发送给客户端的验证码，直接取，然后与客户端传来的验证码做判断。不正确直接跳转到登陆页面。
2、调用业务层，获取执行结果
3、如果Map中返回的字段有ticket，则将其值初始化cookie，设置cookie属性，将cookie加入response对象并返回给客户端

4、如果失败则在model里面加入错误信息，跳转到登陆页面

```java
   @RequestMapping(path = "/login" ,method = RequestMethod.POST)
    //code:验证码 remember：记住我
    //session：存放验证码，将生成的验证码取出来
    //HttpServletResponse:使用户保存cookie
    //参数username password code都是从request中取值的
    public String login(String username, String password, String code, boolean rememberme,
                        Model model, HttpSession session, HttpServletResponse response){
        //判断验证码
        String kaptcha = (String)session.getAttribute("kaptcha");
   //客户端、服务器任一方存的验证码为空、验证码不相等（表现层可先判断，不用交给业务层处理）
    if(StringUtils.isBlank(kaptcha)||StringUtils.isBlank(code)||!kaptcha.equalsIgnoreCase(code)){
        model.addAttribute("codeMsg","验证码不正确");
        return "/site/login";
    }

    //检查账号密码
    int expiredSeconds = rememberme ? REMENBER_EXPIRED_SECONDS : DEFAULT_EXPIRED_SECONDS; //根据是否勾选记住账号设置有效期
    Map<String,Object> map = userService.login(username,password,expiredSeconds);//调用业务层返回结果map
    //存在ticket字段则登陆成功
    if(map.containsKey("ticket")){
        //生成cookie，将ticket压入返回给客户端
        Cookie cookie = new Cookie("ticket",map.get("ticket").toString());
        cookie.setPath(contextPath); //cookie设置为整个项目都有效
        cookie.setMaxAge(expiredSeconds);
        response.addCookie(cookie);//将cookie传入response对象
        return "redirect:/index"; //登陆成功 重定向到首页
    }else{
        //登陆不成功，返回错误信息
        model.addAttribute("usernameMsg",map.get("usernameMsg"));
        model.addAttribute("passwordMsg",map.get("passwordMsg"));
        return "/site/login"; //失败，回到登陆页面
    }
}
```

**退出**
将登陆凭证修改为失效状态：Logout函数直接调用updateStatus方法更新Ticket对象的状态码为1

~~~java
public void logout(String ticket){
        loginTicketMapper.updateStatus(ticket,1);
    }
然后跳转至网站首页
~~~

每一次页面跳转都是一次request

#### 6. 显示登录信息

 >拦截器示例：
 >
 >——定义拦截器，实现HandlerInterceptor
 >
 >——配置拦截器，为它指定拦截、排除的路径
 >
 >拦截器应用：
 >
 >——在请求开始时查询登录用户
 >
 >——在本次请求中持有用户数据
 >
 >——在模板视图上显示用户数据
 >
 >——在请求结束时清理用户数据

**在每个页面头部都要显示用户头像，点击某些按钮就会显示用户名字**
**如果用户没有登陆，页面最上方显示的是登陆按钮。如果没有登陆，显示的是头像、消息等按钮，根据登陆与否需要调整页面内容。**

如果每个页面都调用这些相同的方法来显示用户信息，则耦合度高m例如每个方法里面都要写请求用户的方法。利用Spring拦截器来解决问题。能够拦截浏览器访问请求，在请求的开始和结束部分插入，从而批量解决多个请求共有的业务。能以很低的耦合度解决共有的问题。

**拦截器示例**
定义拦截器，注入Spring容器，实现HandlerInterceptor
-主要有三个方法：**请求之前执行、之后执行、模版引擎执行后执行**

```java
@Component
public class AlphaIntercepter implements HandlerInterceptor {
private static final Logger logger = LoggerFactory.getLogger(AlphaIntercepter.class);

//在Controller处理请求之前执行，如果返回false则Controller不实行
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    logger.debug("Prehandle: "+handler.toString());
    return true;
}

//调用完Controller之后执行
//ModelAndView：可能需要向模版中装入一些数据
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    logger.debug("postHandle: " + handler.toString());
}

//在模版引擎执行完之后执行
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    logger.debug("afterCompletion: " + handler.toString());
}
```

配置拦截器，告诉Spring应该拦截哪些请求，新建一个配置类

```java
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
//注入拦截器
@Autowired
private AlphaIntercepter alphaIntercepter;

//添加拦截器
@Override
public void addInterceptors(InterceptorRegistry registry) {
    //将拦截器加入rehister对象
    registry.addInterceptor(alphaIntercepter)
    .excludePathPatterns("/**/*.css","/**/*.js","/**/*.png","/**/*.jpg","/**/*.jpeg")//访问静态资源不需要拦截
    .addPathPatterns("/register","/login"); //拦截这些方法
}
```

登陆后返回如下结果：

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200311155615695.png)

说明拦截器拦截的是login方法

**拦截器应用**
1、在请求开始时查询登陆用户：
每次请求的渲染过程

![æ¯æ¬¡è¯·æ±çæ¸²æè¿ç¨](https://img-blog.csdnimg.cn/20200311161025627.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

定义一个Util类处理每次请求得到Cookie中的Ticket值：

~~~java
public class CookieUtil {
    public static String getValue(HttpServletRequest request, String name){
        if(request == null || name == null){
            throw new IllegalArgumentException("参数为空！");
        }
        Cookie[] cookies = request.getCookies();
        if(cookies!=null){
            for(Cookie cookie : cookies){
                //找到数据 返回Cookie值
                if(cookie.getName().equalsIgnoreCase(name)){
                    return cookie.getValue();
                }
            }
        }
        return null;
    }
}
~~~

-在本次请求中持有用户数据（将用户数据存到内存里方便以后使用）
需要将User数据放到内存中，便于这次请求后续操作使用。如果放到session中，会极大的增加服务器的压力。由于服务器是多线程环境，如果简单的将User信息存入一个容器中，很有可能产生冲突。此时用到了线程私有的ThreadLocal：

持有用户的信息，用于代替session对象

~~~java
/**
 * 持有用户的信息，用于代替session对象
 */
@Component
public class HostHolder {
    //通过ThreadLocal进行线程隔离
    //底层的set方法是为每个线程创建一个Map对象 get也是线程获取Map
    private ThreadLocal<User> users = new ThreadLocal<>();

    public void setUser(User user){
        users.set(user);
    }

    public User getUser(){
        return users.get();
    }

    /**
     * 清理 也是获取当前线程的ThreadLocalMap，然后清掉数据
     */
    public void clear(){
        users.remove();
    }
}
~~~

在模版视图上显示用户数据
在请求结束时清理用户数据
**定义拦截器类：**

```java
public class LoginTicketInterceptor implements HandlerInterceptor {
@Autowired
UserService userService;

@Autowired
private HostHolder hostHolder;

/**
 * 请求开始时通过ticket获取User信息，将信息存入ThreadLocal
 * @param request
 * @param response
 * @param handler
 * @return
 * @throws Exception
 */
@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    //从cookie中尝试获取ticket：
    String ticket = CookieUtil.getValue(request,"ticket");

    if(ticket!=null){
        //查询凭证
        LoginTicket loginTicket = userService.findLoginTicket(ticket);
        //检查凭证是否过期,是否还有效
        if(loginTicket!=null&&loginTicket.getStatus()==0&&loginTicket.getExpired().after(new Date())){
            //根据凭证查询用户信息
            User user = userService.findUserById(loginTicket.getUserId());
            //在本次请求中持有用户（存用户信息方便本次请求以后使用）
            //因为请求不中断，线程一直存活，ThreadLocal一直在
            hostHolder.setUser(user);
        }
    }
    return true;
}

/**
 * Controller方法执行完之后 将User信息填充到model
 * @param request
 * @param response
 * @param handler
 * @param modelAndView
 * @throws Exception
 */
@Override
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    User user = hostHolder.getUser();
    //用户信息传入model
    if(user != null && modelAndView != null){
        modelAndView.addObject("loginUser",user);
    }
}

/**
 * 渲染之后，清空ThreadLocal里面的User信息
 * @param request
 * @param response
 * @param handler
 * @param ex
 * @throws Exception
 */
@Override
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    hostHolder.clear();
}
    }
```

#### 7. 账号设置

>上传文件：
>
>——请求：必须是POST请求
>
>——表单：enctype = “multipart/from-data”
>
>——**Spring MVC：通过MultipartFile处理上传文件**
>
>开发步骤：
>
>——访问账号设置页面
>
>——上传头像
>
>——获取头像

**上传文件**

> -请求：必须是Post请求
> -表单：enctype=“multipart/form-data”
> -Spring MVC：通过MultipartFile处理上传文件 故只在表现层保存文件，业务层只用更新路径
>
> 代码逻辑：读取文件后缀后，将其与随机字符串拼接，表示服务器本地存入的文件名。然后进行存储，更新数据库里面的Header字段。

```java
    @RequestMapping(path = "/upload",method = RequestMethod.POST)
    //MultipartFile:MVC提供专有的文件
    public String uploadHeader(MultipartFile headerImage, Model model){
        if(headerImage == null){
            model.addAttribute("error","尚未传入图片");
            return "/site/setting";
        }
    //读取文件后缀
    String filename = headerImage.getOriginalFilename();
    String suffix = filename.substring(filename.lastIndexOf("."));//最后一个点往后截取字符串
    //取不到后缀
    if(StringUtils.isBlank(suffix)){
        model.addAttribute("error","文件的格式不正确");
        return "/site/setting";
    }
    //更新用户名，存储的路径
    filename = CommunityUtil.generateUUID()+suffix;
    //确定文件存放路径
    File dest = new File(uploadPath+"/"+filename);
    try {
        //存储文件
        headerImage.transferTo(dest);
    } catch (IOException e) {
        logger.error("上传文件失败 "+e.getMessage());
        throw new RuntimeException("上传文件失败，服务器发生异常",e);
    }
    
    // 更新当前用户头像的路径
    // http://localhost:8080/community/user/header/xxx.png
    User user = hostHolder.getUser();
    String headerUrl = domain + contextPath + "/user/header/" + filename; //允许外界访问的web路径
    userService.updateHeader(user.getId(),headerUrl);
    
    return "redirect:index";
}
```

获得头像：

```java
     @RequestMapping(path = "/header/{fileName}",method = RequestMethod.GET)
    public void getHeader(@PathVariable("fileName") String fileName, HttpServletResponse response){ //路径中获取到fileName，并将其作为参数
        //服务器存放路径
        fileName = uploadPath+"/"+fileName;
        //文件后缀
        String suffix = fileName.substring(fileName.lastIndexOf("."));
        //响应图片
        response.setContentType("image/"+suffix);
        try (
                OutputStream os = response.getOutputStream();
                FileInputStream fis = new FileInputStream(fileName);
                ){  
       byte[] buffer = new byte[1024];
        int b=0;
        while((b = fis.read(buffer))!= -1){
            os.write(buffer,0,b);
        }
    } catch (IOException e) {
        logger.error(" 读取头像失败 "+e.getMessage());
        throw new RuntimeException(" 上传文件失败，服务器发生异常",e);
    }

}
```

#### 8. 检查登录状态

>使用拦截器：
>
>——在方法前标注自定义注解
>
>——拦截所有请求，只处理带有该注解的方法
>
>自定义注解：
>
>——常用元注解：@Target、@Retention、@Document、@Inherited
>
>——读取注解：Method.getDeclaredAnnotations(),  Method.getAnnotation(Class<T> annotationClass)

如果用户没登陆，直接访问设置用户的URL，这样应该被拒绝访问，用户端需要有个判断机制。

使用拦截器：
**在方法前标注自定义注解**
**拦截所有请求，只处理带有该注解的方法**

自定义注解
常用的元注解：
**@Target:定义自定义注解可以作用在那个类型上**
**@Retention：声明自定义注解有效的时间（运行时有效/编译时有效…）**
**@Document：生成文档时要不要带上注解**
**@Inherited：用于继承 ，指定子类要不要继承父类的注解**

~~~java
/**
 * 是否需要登陆
 */
@Target(ElementType.METHOD) //用在方法上
@Retention(RetentionPolicy.RUNTIME) //运行时有效
public @interface LoginRequired {
    
}
~~~


如何读取注解（通过反射）：
Method.getDeclareAnnotations()
Method.getAnnotation(Class annotationClass)

**定义拦截器：**
在拦截器里定义，只有访问的方法加上了注解才进行检查是否用户登陆，别的都继续进行。这样可以避免在Config里面增加过多的拦截配置

```java
@Component
public class LoginRequiredInterceptor implements HandlerInterceptor {
@Autowired
private HostHolder hostHolder;

@Override
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    //如果拦截到的是一个方法
    if(handler instanceof HandlerMethod){
        HandlerMethod handlerMethod = (HandlerMethod) handler;
        Method method = handlerMethod.getMethod();
        //读到注解，则需要登陆才能访问
        LoginRequired loginRequired = method.getAnnotation(LoginRequired.class);
        //访问该方法但是用户没登陆的情况
        if(loginRequired != null && hostHolder.getUser() == null){
            //重定向到登陆页面
            response.sendRedirect(request.getContextPath()+"/login");
            return false;
        }
    }
    return true;
}
```

在Config文件里面讲拦截器加入registor对象

~~~java
registry.addInterceptor(loginRequiredInterceptor)
                .excludePathPatterns("/**/*.css","/**/*.js","/**/*.png","/**/*.jpg","/**/*.jpeg");//访问静态资源不需要拦截
//这里不需要使用add方法了，所有请求都拦截，拦截器里面定义没加注解直接放行，加了注解的额外判断
~~~

## 第四章：社区核心功能

#### 1. 过滤敏感词

> 前缀树：
>
> ——名称：Trie、字典树、查找树
>
> ——特点：查找效率高，消耗内存大
>
> ——应用：字符串检索、词频统计、字符串排序等
>
> 敏感词过滤器：
>
> ——定义前缀树
>
> ——根据敏感词，初始化前缀树
>
> ——编写过滤敏感词的方法

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200312170738466.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

 **敏感词搜索逻辑**：

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200312171436824.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

假设用户输入的字符串是：xwabfabcff
敏感词为：abc、bf、be

**1、首先根据敏感词构造前缀树，在构成敏感词的节点上标记表示是敏感词**
前缀树的特点：
1、每个节点只包含一个字符，根节点为空
2、从根节点到某一个节点连接起来对应一个字符串
3、每个节点下的子节点都不相同

**2、需要三个指针：**
初始化：第一个指针指向前缀树根节点，第二个指针和第三个指针指向字符串第一个字符
遍历的结果存到StringBuilder里，如果是敏感词，则全部打上*，最后得到的是已经过滤好敏感词的字符串。

需要两个指针的原因：两个指针之间的字符串为待查询子串，如果是敏感词则过滤。两个指针一开始一起往后面移动。第一个指针标记的是敏感词起始的位置，如果能匹配，则第二个指针开始往后遍历判断敏感词，树上的指针一起开始往后走。

**定义SensitiveFilter类：**
静态变量：替换符所有敏感词都以这个来替换
定义一个空的根节点，前缀树节点是一个内部类，节点有一个boolean变量指的是从根节点到该节点的字符串是否为敏感字符串，还有一个Map<Charactor,TrieNode>存的是key为字符，Value为对应的子节点的Map。

~~~java
//替换符
    private static final String REPLACEMENT = "***";

    //根节点
    private TrieNode rootNode = new TrieNode();

    //内部类：前缀树
    private class TrieNode{


        //关键词结束标志
        public Boolean isKeywordEnd = false;

        //子节点 key是下级节点字符 value是下级节点地址
        public Map<Character,TrieNode> subNodes = new HashMap<>();

        public Boolean getKeywordEnd() {
            return isKeywordEnd;
        }

        public void setKeywordEnd(Boolean keywordEnd) {
            isKeywordEnd = keywordEnd;
        }

        //添加子节点
        public void addSubNode(Character c, TrieNode node){
            subNodes.put(c,node);
        }

        //获取子节点方法
        public TrieNode getSubNode(Character key){
            return subNodes.get(key);
        }
    }
~~~

**编写Init函数，在Spring注入该类，调用该类构造函数之后，根据txt文件构造敏感词前缀树**：
@PostConstruct表示该方法在构造函数之后执行

~~~java
@PostConstruct
    public void init(){
        try(
                //得到敏感词文件的字节流
                InputStream is = this.getClass().getClassLoader().getResourceAsStream("sensitivewords.txt");
                //改造为缓冲流
                BufferedReader reader = new BufferedReader(new InputStreamReader(is));
                ) {
                System.out.println("加载l");
                String keyword;
                //readline()方法读取一行一个敏感词
                while((keyword = reader.readLine())!=null){
                    //敏感词添加到前缀树对象
                    this.addKeyWord(keyword);
                }
        } catch (Exception e) {
           logger.error("加载敏感词信息失败： "+e.getMessage());
        }
    }
~~~

**addKeyWord函数，将敏感词插入到前缀树**：

~~~java
//敏感词添加到前缀树中
    private void addKeyWord(String keyword){
        TrieNode tempNode = rootNode;
        for(int i = 0;i<keyword.length();i++){
            if(tempNode.subNodes.containsKey(keyword.charAt(i))){
                tempNode = tempNode.subNodes.get(keyword.charAt(i));
                if(i==keyword.length()-1){
                    tempNode.isKeywordEnd = true;
                }
            }else{
                TrieNode temp = new TrieNode();
                if(i==keyword.length()-1)temp.isKeywordEnd=true;
                tempNode.addSubNode(keyword.charAt(i),temp);
                tempNode=temp;
            }
        }
    }
~~~

用户有可能在敏感词中插入特殊字符来逃避检测，定义一个isSymbol函数
标准为Ascii码+东亚文字：

~~~java
/**
     * 返回当前字符是否为普通字符（防止在敏感词中插入特殊字符干扰）
     *  0x2E80 ~ c > 0x9FFF 为东亚文字，需要排除
     * @param c
     * @return 返回是true，则为特殊字符
     */
    public boolean isSymbol(Character c){
        return !CharUtils.isAsciiAlphanumeric(c) && ( c < 0x2E80 || c > 0x9FFF );
    }
~~~

定义一个过滤函数，处理待处理字符串，返回过滤后的字符串：
如果当前字符是一个特殊字符：
1、position节点指向根节点，表示特殊符号不是在敏感词判断过程包含范围内，直接加入StringBuilder 敏感词起始指针前移一位；
2、position节点不指向根节点，直接跳过该符号 position++；

当前字符不是特殊字符：
1、如果前缀树往下找不到对应的节点，证明从begin～position不是敏感词，此时：
将begin处的字符加入StringBuilder
begin++；
position = begin；
树指针移到根节点准备下一次判断

2、如果当前树指针指向的节点标记为true：
敏感词找到，直接将***字符加入StringBuilder
position++
begin=position；
树指针移到根节点准备下一次判断

3、如果当前树指针指向的节点标记为false：
position++，begin不变 继续搜索以begin下标开始的敏感词

结束判断（原代码存在错误）：
以position==length（）为结束判断条件
position走到这个位置的两种情况：
1、最后一段子字符串为敏感词，直接替换后++；
2、还没在前缀树搜索阶段的情况下，begin和position同时走到最后。

代码：

~~~java
public String filter(String text){
        if(StringUtils.isBlank(text))return null;

        //指针1：指向树
        TrieNode tempNode = rootNode;

        //指针2：
        int begin = 0;

        //指针3：
        int position = 0;

        //最终结果
        StringBuilder sb = new StringBuilder();

        while(position < text.length()){
            char c = text.charAt(position);

            //跳过特殊符号
            if(isSymbol(c)){
                //若指针1处于根节点，
                if(tempNode == rootNode){
                    sb.append(c);
                    begin++;
                }
                //无论符号在开头或中间，指针3都向下走一步
                position++;
                continue;
            }

            //检查下级节点
            tempNode = tempNode.getSubNode(c);
            if(tempNode == null){
                //以begin为开头的字符串不是敏感词
                sb.append(text.charAt(begin));
                //进入下一个位置
                position = ++begin;
                //重新指向根节点
                tempNode = rootNode;
            }else if(tempNode.isKeywordEnd){
                //发现敏感词，替换begin～position区间的字符串
                sb.append(REPLACEMENT);
                //进入下一个位置
                begin = ++position;
                //重新指向根节点
                tempNode = rootNode;
            }else{
                // 检查下一个字符
                position++;
            }
        }
        //将最后一批字符计入结果
        sb.append(text.substring(begin));
        return sb.toString();
    }
~~~

**原代码的缺点：不能以position的位置来判断循环过程是否结束！**
原因1：假设封杀台独艺人周子瑜来大陆是敏感词
周子瑜也是敏感词
那么如果输入字符串是封杀台独艺人周子瑜来，遍历过程中position直接到最后，结束循环，没有过滤敏感词周子瑜

原因2、如果一个更长的串是敏感词，从起点开始的一个子串也是敏感词，则只会根据boolean值为true将子串替换，不可取。需定义一个变量存储找到最大敏感词的最大下标。

**修改后的算法成功过滤了子串：**

~~~java
public String filter(String text){
        if(StringUtils.isBlank(text))return null;

        //指针1：指向树
        TrieNode tempNode = rootNode;

        //指针2：
        int begin = 0;

        //指针3：
        int position ;

        //最终结果
        StringBuilder sb = new StringBuilder();
        
        while(begin<text.length()){

            tempNode=rootNode;

            //为特殊字符，直接放行
            if(isSymbol(text.charAt(begin))){
                begin++;
                sb.append(text.charAt(begin));
                continue;
            }

            //前缀树第一个字符是否包含begin处所存的字符
            tempNode = tempNode.getSubNode(text.charAt(begin));

            //不包含，当前字符直接begin++
            if(tempNode == null){
                sb.append(text.charAt(begin));
                begin++;

                //进行前缀树搜索的情况：
            }else{
                //标记最大敏感词字串
                int max = -1;
                //重新移到根节点，因为有第一个字符就是单个敏感字的情况
                tempNode=rootNode;
                //前缀树搜索
                for(position = begin;position<text.length();position++){
                    tempNode = tempNode.getSubNode(text.charAt(position));
                    if(tempNode == null)break;
                    if(tempNode.isKeywordEnd==true)max=Math.max(position,max);
                }
                //找到敏感词的情况
                if(max>=0){
                    sb.append(REPLACEMENT);
                    begin = max+1;
                }else{
                    sb.append(text.charAt(begin));
                    begin++;
                }
            }
        }
        return sb.toString();
    }
~~~

#### 2. 发布帖子

>AJAX
>
>——Asynchronous JavaScript and XML
>
>——异步的JavaScript与XML，不是一门新技术
>
>——使用AJAX，网页能够增量更新呈现在页面上，而不需要刷新整个页面
>
>——虽然X代表XML，但目前JSON的使用比XML更加普遍
>
>示例：
>
>——使用jQuery发送AJAX请求。
>
>实践：
>
>——采用AJAX请求，实现发布帖子的功能

**功能描述**

点击帖子，进入到相应的帖子界面

**实现流程**

**Dao层**

创建方法：

 ~~~java
/**
     * 根据主键查找帖子
     * @param id
     * @return
     */
    DiscussPost selectDiscussPostById(int id);
 ~~~

Mapper文件里增加配置SQL语句：

~~~sql
<select id="selectDiscussPostById" resultType="DiscussPost">
        select <include refid="selectFields"></include>
        from discuss_post
        where id = #{id}
</select>
~~~

**Service层：**

~~~java
public DiscussPost findDiscussPostById(int id){
        return discussPostMapper.selectDiscussPostById(id);
 }
~~~

**Controller层**

因为Post表里面只查到了发帖人的userid，但是在页面上还需要显示一部分发帖人的详细信息。此时有两种方法进行查询：

1、直接修改SQL语句为关联查询
优点：只需进行一次查询，效率高
缺点：存在冗余，如果别的不需要用户信息的方法也使用这个SQL进行查询，则会多出来许多无用的信息

2、分别查两次（采用方法）
优点缺点与第一个方法相反 ，可以用Redis解决效率问题，将影响性能的查询数据缓存到缓存里面

~~~java
@RequestMapping(path = "/detail/{discussPostId}" ,method = RequestMethod.POST)
    public String getDiscussPost(@PathVariable("discussPostId") int discussPostId, Model model){
        //查到帖子
        DiscussPost post = discussPostService.findDiscussPostById(discussPostId);
        model.addAttribute("post",post);
        //查询作者
        User user = userService.findUserById(post.getUserId());
        model.addAttribute("user",user);
        
        //查询帖子回复（待实现）
        
        return "/site/discuss-detail";
    }
~~~

发布帖子

**使用jQuery发送异步请求的示例**
1、导入FastJson包：

~~~java
<!-- https://mvnrepository.com/artifact/com.alibaba/fastjson -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.66</version>
</dependency>
~~~

2.编写工具类将一些数据封装成JSON对象，然后转换成JSON格式的String。

~~~java
/**
     * 将一些信息封装成JSON对象，转换成字符串
     * @param code 状态码
     * @param msg  提示信息
     * @param map  业务数据
     * @return
     */
    public static String getJSONString(int code, String msg, Map<String,Object> map){
        //将数据装入Json对象
        JSONObject json = new JSONObject();
        json.put("code",code);
        json.put("msg",msg);
        if(map!=null){
            for(String key : map.keySet()){
                json.put(key,map.get(key));
            }
        }
        //转换成String
        return json.toJSONString();
    }

    public static String getJSONString(int code, String msg){
        return getJSONString(code,msg,null);
    }

    public static String getJSONString(int code){
        return getJSONString(code,null,null);
    }
~~~

**测试示例**
服务端：

~~~java
//ajax示例
    @RequestMapping(path = "/ajax" ,method = RequestMethod.POST)
    @ResponseBody
    public String testAjax(String name,int age){
        System.out.println(name);
        System.out.println(age);
        return CommunityUtil.getJSONString(0,"操作成功");
    }
~~~

前端：

```<script>
<script>
    function send() {
        $.post(
            "/community/alpha/ajax",
            {"name":"张三","age":23},
            function(data) {
                console.log(typeof(data));
                console.log(data);

                data = $.parseJSON(data);
                console.log(typeof(data));
                console.log(data.code);
                console.log(data.msg);
            }
        );
    }
</script>
```

结果：后台打印出前端传过来的信息
前端控制台成功取到String类型，转换为Object并取到对应的值

采用Ajax请求 实现发布帖子的功能

Dao层

~~~java
//插入数据
    int insertDiscussPost(DiscussPost discussPost);
~~~

~~~JAVA
<insert id="insertDiscussPost" parameterType="DiscussPost">
        insert into discuss_post(<include refid="insertFields"></include>include>)
        values(#{userId},#{title},#{content},#{type},#{status},#{createTime},#{commentCount},#{score})
</insert>
~~~

Service层

1、帖子和标题中可能包含HTML标签相关代码，以后返回给前端显示时，可能出现问题，需要进行转译处理
2、对帖子和标题过滤敏感词

~~~java
/**
     * 发表帖子
     * @param discussPost
     * @return
     */
    public int addDiscussPost(DiscussPost discussPost){
        if(discussPost == null){
            throw new IllegalArgumentException("参数不能为空");
        }

        //discussPost的 title和content属性 需要做敏感词过滤
        //转义HTML标记（如果文字部分包含<script>之类的HTML标签，可能会对页面存在影响，需要对其进行处理）
        discussPost.setTitle(HtmlUtils.htmlEscape(discussPost.getTitle()));
        discussPost.setContent(HtmlUtils.htmlEscape(discussPost.getContent()));
        //过滤
        discussPost.setTitle(sensitiveFilter.filter(discussPost.getTitle()));
        discussPost.setContent(sensitiveFilter.filter(discussPost.getContent()));
        
        return discussPostMapper.insertDiscussPost(discussPost);
    }
~~~

Controller层

~~~java
@Autowired
    private HostHolder hostHolder;

    /**
     * 异步的 
     * @param title
     * @param content
     * @return
     */
    @RequestMapping(path = "/add" ,method = RequestMethod.POST)
    @ResponseBody
    //前端只发过来文本信息
    public String addDiscussPost(String title,String content){
        //取到用户信息
        User user = hostHolder.getUser();
        if(user == null){
            return CommunityUtil.getJSONString(403,"你还没有登陆");
        }

        //创建对象，设置文本、时间、创建人信息等字段 
        DiscussPost post = new DiscussPost();
        post.setUserId(user.getId());
        post.setTitle(title);
        post.setContent(content);
        post.setCreateTime(new Date());
        discussPostService.addDiscussPost(post);
        
        //报错的情况将来统一处理
        return CommunityUtil.getJSONString(0,"发布成功");
    }

~~~

#### 数据库事务管理（理论）

事务的概念
没有去做事务的隔离会怎么样？
每一个浏览器在访问服务器的时候，服务器就会创建一个线程，去处理浏览器的请求。在一个请求中可能访问数据库，对其进行修改。

如果多个线程同时访问同一个网站的某一个功能，多个事务同时访问一份数据的情况就会出现，不做隔离就出现问题。

常见的异常：
1、第一类丢失更新：
某一个事务回滚，导致另外一个事务更新的数据丢失（事务交叉，事务1回滚将事务2的结果覆盖）
![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200313235547451.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

2、第二类丢失更新
某一个事务回滚导致另外一个事务已更新的数据丢失（事务1覆盖了事务2的结果）

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314091853500.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

3、脏读
某一个事务读取了另外一个事务未提交的数据

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314092028856.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

4、幻读
某一个事务，对同一个表前后查询到的行数不一致（幻读是查询多条数据不一致，脏读是查询一条数据不一致，如下图所示，很短的时间内查询到的行数不一致）

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314094519388.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

5、不可重复读
某一个事务，对同一个数据前后读取的结果不一致（在根短的时间间隔之内对同一份数据读取情况不同）

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314093838690.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

**常见的隔离级别（由低到高）**：
Read Uncommitted：读取未提交的数据
Read Committed：读取已提交的数据
Repeatable Read：可重复读
Serializable ：串行化（加锁，性能下降快）

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314095041337.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

实际开发中通常选择中间的一些级别

**实现机制（悲观锁、乐观锁）**

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314100456110.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

Spring中的事务机制

Demo：

~~~java
@Transactional(isolation = Isolation.READ_COMMITTED, propagation = Propagation.REQUIRED)
    public Object save1() {
        // 新增用户
        User user = new User();
        user.setUsername("alpha");
        user.setSalt(CommunityUtil.generateUUID().substring(0, 5));
        user.setPassword(CommunityUtil.md5("123" + user.getSalt()));
        user.setEmail("alpha@qq.com");
        user.setHeaderUrl("http://image.nowcoder.com/head/99t.png");
        user.setCreateTime(new Date());
        userMapper.insertUser(user);

        // 新增帖子
        DiscussPost post = new DiscussPost();
        post.setUserId(user.getId());
        post.setTitle("Hello");
        post.setContent("新人报道!");
        post.setCreateTime(new Date());
        discussPostMapper.insertDiscussPost(post);

        Integer.valueOf("abc");

        return "ok";
    }
~~~

声明事务：
@Transactional(isolation = Isolation.READ_COMMITTED, propagation = Propagation.REQUIRED)
isolation = Isolation.READ_COMMITTED：手动设置隔离级别
propagation：事务的传播机制 ：业务方法A可能调用业务方法B，两个方法都可能加上注解管理事务

常用的传播机制：

~~~java
REQUIRED: 支持当前事务(外部事务),如果不存在则创建新事务.（如果外部事务存在支持外部事务，否则创建新事务）
REQUIRES_NEW: 创建一个新事务,并且暂停当前事务(外部事务).（无视外部事务创建新事务）
NESTED: 如果当前存在事务(外部事务),则嵌套在该事务中执行(独立的提交和回滚),否则就会REQUIRED一样.
~~~

#### 3. 帖子详情

>DiscussPostMapper
>
>——在帖子标题上增加访问详情页面的链接
>
>discuss-detail.html
>
>——处理静态资源的访问路径
>
>——复用index.html的header区域
>
>——显示标题、作者、发布时间、帖子正文等内容

#### 4.显示评论

>数据层
>
>——根据实体查询一页评论数据。
>
>——根据实体查询评论的数量。
>
>业务层
>
>——处理查询评论的业务。  
>
>——处理查询评论数量的业务。
>
>表现层
>
>——显示帖子详情数据时，同时显示该帖子所有的评论数据

Dao

 ~~~java
@Mapper
public interface CommentMapper {
    /**
     * 根据实体来查询评论 根据帖子/评论/...来查询
     * @param entityType
     * @param entityId
     * @param limit      行数限制
     * @return
     */
    List<CommentMapper> selectCommentsByEntity(int entityType ,int entityId,int limit);

    /**
     * 根据实体查询评论数量
     * @param entityType
     * @param entityId
     * @return
     */
    int selectCountByEntity(int entityType ,int entityId);
}
 ~~~

Service

~~~java
@Service
public class CommentService {
    
    @Autowired
    private CommentMapper commentMapper;
    
    public List<Comment> findCommentsByEntity(int entityType,int entityId,int offset,int limit){
        return commentMapper.selectCommentsByEntity(entityType,entityId,offset,limit);
    }
    
    public int findCommentCount(int entityType,int entityId){
        return commentMapper.selectCountByEntity(entityType,entityId);
    }
}
~~~

Controller

在CommunityConstant.java中定义实体类型常量：

~~~java
/**
     * 实体类型：帖子
     */
    int ENTITY_TYPE_POST = 1;

    /**
     * 实体类型：评论
     */
    int ENTITY_TYPE_COMMENT = 2;
~~~

~~~java
/**
     *查询帖子详情
     *
     * Page参数整理接收分页的条件
     * 如果在参数里存在实体类型，那么MVC会把实体类型存到Model里，页面上通过Model获取Page
     */
    @RequestMapping(path = "/detail/{discussPostId}" ,method = RequestMethod.GET)
    public String getDiscussPost(@PathVariable("discussPostId") int discussPostId, Model model, Page page){
        //查到帖子
        DiscussPost post = discussPostService.findDiscussPostById(discussPostId);
        model.addAttribute("post",post);
        //查询作者
        User user = userService.findUserById(post.getUserId());
        model.addAttribute("user",user);

        //评论分页信息
        page.setLimit(5); // 每页5条评论
        //设置路径
        page.setPath("/discuss/detail/"+discussPostId);
        //总的回复数
        page.setRows(post.getCommentCount());

        //定义评论：帖子的所有评论  回复：每条评论的留言

        //得到帖子的所有评论
        List<Comment> commentList = commentService.findCommentsByEntity(ENTITY_TYPE_POST,post.getId(),page.getOffset(),page.getLimit());

        //找到每条评论对应的user信息 Vo:ViewObject显示的对象
        //一个帖子的所有评论Vo：
        List<Map<String,Object>> commentVoList = new ArrayList<>();

        if(commentVoList != null){
            //对于每条评论
            for(Comment comment:commentList){
                //一个评论的所有跟帖Vo：
                Map<String,Object> commentVo = new HashMap<>();
                //显示作者信息、评论内容、跟帖内容到页面：

                //评论
                commentVo.put("comment",comment);
                //作者信息
                commentVo.put("user",userService.findUserById(comment.getUserId()));

                //查询跟帖Comment列表
                List<Comment> replyList = commentService.findCommentsByEntity(ENTITY_TYPE_COMMENT,comment.getId(),0,Integer.MAX_VALUE);

                //跟帖的VO列表
                List<Map<String,Object>> replyVoList = new ArrayList<>();
                if(replyList != null){
                    for(Comment reply : replyList){
                        Map<String,Object> replyVo = new HashMap<>();
                        //回复
                        replyVo.put("reply",reply);
                        //作者
                        replyVo.put("user",userService.findUserById(reply.getUserId()));
                        //回复的目标
                        User target = reply.getTargetId() == 0 ? null :userService.findUserById(reply.getTargetId());
                        replyVo.put("target",target);

                        replyVoList.add(replyVo);
                    }
                }
                commentVo.put("replys",replyVoList);

                //回复数量
                int replyCount = commentService.findCommentCount(ENTITY_TYPE_COMMENT,comment.getId());
                commentVo.put("replyCount",replyCount);

                commentVoList.add(commentVo);
            }
        }

        model.addAttribute("comments",commentVoList);

        return "/site/discuss-detail";
    }
~~~

#### 5. 添加评论

>数据层：
>
>——增加评论数据。
>
>——修改帖子的评论数量。
>
>业务层：
>
>——处理添加评论的业务：先增加评论、再更新帖子的评论数量。
>
>表现层 ：
>
>——处理添加评论数据的请求。
>
>——设置添加评论的表单。

**Comment表的内容**：



![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200314165413601.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

逻辑：
1、根据传入的参数：帖子ID ， 查询到帖子实体和发帖人信息，传入model
2、对Page对象设置分页信息（每页评论数、查询路径、总回复数）
3、查询到帖子所有的评论，存入List
4、遍历这个List，对于每条评论：
    1、将评论和作者信息加入commentVo
    2、通过Targetid查到跟帖评论列表，创建replyVoList对于每个跟帖回复：
            1、创建replyVo
            2、加入文本信息
            3、查到用户信息、加入
            4、加入回复的用户信息（页面上会显示：回复xxx）
            5、将ReplyVo加入List
    3、将跟帖信息加入commentVo
    4、将回复数量加入commentVo
    5、将commentVo加入commentVoList
添加评论

DAO

**增加评论数据**

~~~java
/**
     * 插入评论
     * @param comment
     * @return
     */
    int insertComment(Comment comment);
~~~

~~~java
<insert id="insertComment" parameterType="Comment">
        insert into comment(<include refid="insertFields"></include>)
        values(#{userId},#{entityType},#{entityId},#{targetId},#{content},#{status},#{createTime})
</insert>
~~~

**修改帖子的评论数量**
增加评论以后就要更新帖子表对应条目的评论数量字段（这个数据是冗余的，为了查询方便）

~~~java
/**
     * 更新评论数
     * @param id
     * @param commentcount
     * @return
     */
    int updateCommentCount(int id,int commentcount);
~~~

~~~java
<update id="updateCommentCount">
        update discuss_post set comment_count = #{commentCount} where id = #{id}
</update>
~~~

Service
处理添加评论的业务
先添加评论、再更新帖子的评论数量:包含两步操作，更新两个表，所以要声明事务：
1、将Comment对象的文本内容进行转义和去敏感词，插入评论表
2、根据Comment对象的两个字段：回复主体id和回复主体内容，查询评论数量。在更新被回复对象的CommentCount字段

~~~java
@Transactional(isolation = Isolation.READ_COMMITTED,propagation = Propagation.REQUIRED)
    public int addComment(Comment comment){
        if(comment == null){
            throw new IllegalArgumentException("参数不能为空");
        }
        //过滤评论敏感词、HTML转义、插入评论表
        comment.setContent(HtmlUtils.htmlEscape(comment.getContent()));
        comment.setContent(sensitiveFilter.filter(comment.getContent()));
        int rows = commentMapper.insertComment(comment);

        //更新帖子评论数量
        if(comment.getEntityType()==ENTITY_TYPE_POST){
            //根据被评论实体类型、ID查询评论数
            int count = commentMapper.selectCountByEntity(comment.getEntityType(),comment.getEntityId());
            discussPostService.updateCommentCount(comment.getEntityId(),count);
        }
        return rows;
    }
~~~

Controller

处理添加评论数据的请求
//这里需要通过URI获取被回复帖子ID，用于评论以后重定向回帖子页面

~~~java
@RequestMapping(path = "/add/{discussPostId}",method = RequestMethod.POST)
    public String addMethod(@PathVariable("discussPostId") int discussPostId, Comment comment){
        //设置评论人id、状态、日期
        comment.setUserId(hostHolder.getUser().getId());
        comment.setStatus(0);
        comment.setCreateTime(new Date());
        //插入评论
        commentService.addComment(comment);
        
        //重定向到被评论帖子详情页面
        return "redirect:/discuss/detail/"+discussPostId;
    }
~~~

#### 6. 私信列表

>私信列表
>
>——查询当前用户的会话列表，   每个会话只显示一条最新的私信。
>
>——支持分页显示。
>
>私信详情
>
>——查询某个会话所包含的私信。
>
>——支持分页显示。

 功能

**私信列表**
1、查询当前用户的会话列表，每个回话列表只显示一条最新的私信（类似QQ）
2、支持分页显示
**私信详情**
1、查询某个会话所包含的私信
2、支持分页

私信表结构



![img](https://img-blog.csdnimg.cn/20200315102342850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

Conversation Id（冗余数据）：标识某一个会话，便于查找。利用from_id和to_id也可以做到。以会话为条件进行查询的时候更加方便
格式：111_112 form id和 to_id小的在前大的在后，表示一个会话。如果直接以form id和 to_id来查询，则SQL语句条件会复杂。

实体类

~~~java
private int id;
    private int fromId;
    private int toId;
    private String conversationId;
    private String content;
    private int status;
    private Date createTime;
~~~

dao

~~~java
@Mapper
public interface MessageMapper {

    //查询当前用户会话列表，针对每个回话只返回一条最新的私信
    List<Message> selectConversations(int userid,int offset,int limit);

    //查询当前用户会话数量
    int selectConversationCount(int userId);

    //查询某个回话所包含的私信列表
    List<Message> selectLetters(String conversationId,int offset,int limit);

    //查询某个回话所包含的私信数量
    int selectLetterCount(String conversationId);

    //查询未读私信数量
    int selectLetterUnreadCount(int userId,String conversationId);
    
}
~~~

Service

~~~java
@Service
public class MessageService {
    
    @Autowired
    private MessageMapper messageMapper;
    
    public List<Message> findConversations(int userId, int offset, int limit){
        return messageMapper.selectConversations(userId,offset,limit);
    }
    
    public int findConversationCount(int userId){
        return messageMapper.selectConversationCount(userId);
    }
    
    public List<Message> findLetters(String conversationId,int offset,int limit){
        return messageMapper.selectLetters(conversationId,offset,limit);
    }
    
    public int findLetterCount(String conversationId){
        return messageMapper.selectLetterCount(conversationId);
    }
    
    public int findLetterUnreadCount(int userId,String conversationId){
        return messageMapper.selectLetterUnreadCount(userId,conversationId);
    }
    
}
~~~

**Controller**

**私信列表**：需要将两种类型的信息装入Model：
1、查找到每个会话最新的信息，根据这个Message的会话Id找到会话总的信息、未读信息、发送人信息，加入Map，再加入List
2、查找总的未读消息数量

~~~java
//私信列表
    @RequestMapping(path = "/letter/list" ,method = RequestMethod.GET)
    public String getLetterList(Model model, Page page){
        User user = hostHolder.getUser();
        //设置分页信息
        page.setLimit(5);
        page.setPath("/letter/list");
        page.setRows(messageService.findConversationCount(user.getId()));
        //会话列表（每一个会话的最新私信集合）
        List<Message> conversationList = messageService.findConversations(user.getId(),page.getOffset(),page.getLimit());
        //查询到的会话列表VO结果
        List<Map<String,Object>> conversations = new ArrayList<>();
        if(conversationList!=null){
            //对于会话列表的每个Message
            for(Message message:conversationList){
                Map<String,Object> map = new HashMap<>();
                //显示会话信息
                map.put("conversation",message);
                //显示总的信息
                map.put("letterCount",messageService.findLetterCount(message.getConversationId()));
                //显示未读信息
                map.put("unreadCount",messageService.findLetterUnreadCount(user.getId(),message.getConversationId()));
                //最新一条消息是发给谁的
                int targetId = user.getId()==message.getFromId() ? message.getToId():message.getFromId();
                //根据发送人查找他的信息
                map.put("target",userService.findUserById(targetId));
                //将当前会话Vo加入Vo列表
                conversations.add(map);

            }
        }
        model.addAttribute("conversations",conversations);

        //查询未读消息数量（整个用户所有的未读消息，显示在最上面）
        int letterUnreadCount = messageService.findLetterUnreadCount(user.getId(),null);
        model.addAttribute("letterUnreadCount",letterUnreadCount);

        return "/site/letter";
    }
~~~

#### 7. 发送私信

>发送私信
>
>——采用异步的方式发送私信。
>
>——发送成功后刷新私信列表。
>
>设置已读
>
>——访问私信详情时，   将显示的私信设置为已读状态。

 Dao层

~~~java
//新增消息
int insertMessage(Message message);
    
//修改消息的状态（进入页面该页面所有消息批量变成已读）
int updateStatus(List<Integer>ids,int status);
~~~

Service层

~~~java
过滤敏感词、插入数据
public int addMessage(Message message){
        message.setContent(HtmlUtils.htmlEscape(message.getContent()));
        message.setContent(sensitiveFilter.filter(message.getContent()));
        return messageMapper.insertMessage(message);
    }

//读取数据 批量更改message状态
public int readMessage(List<Integer> ids){
        return messageMapper.updateStatus(ids,1);
}
~~~

Controller层

~~~java
@RequestMapping(path = "/letter/send",method = RequestMethod.POST)
    @ResponseBody
    public String sendLetter(String toName,String content){
        //通过对方用户名查找到对方数据
        User target = userService.findUserByName(toName);
        if(target == null)return CommunityUtil.getJSONString(1,"目标用户不存在");
        
        Message message = new Message();
        message.setFromId(hostHolder.getUser().getId());
        message.setToId(target.getId());
        if(message.getFromId()<message.getToId()){
            message.setConversationId(message.getFromId()+"_"+message.getToId());
        }else{
            message.setConversationId(message.getToId()+"_"+message.getFromId());
        }
        message.setContent(content);
        message.setCreateTime(new Date());
        
        messageService.addMessage(message);
        
        return CommunityUtil.getJSONString(0); //0表示成功
    }
~~~

#### 8. 统一处理异常

>@ControllerAdvice
>
>——用于修饰类，表示该类是Controller的全局配置类。
>
>——在此类中，可以对Controller进行如下三种全局配置：异常处理方案、绑定数据方案、绑定参数方案。
>
>@ExceptionHandler
>
>——用于修饰方法，该方法会在Controller出现异常后被调用，用于处理捕获到的异常。
>
>@ModelAttribute
>
>——用于修饰方法，该方法会在Controller方法执行前被调用，用于为Model对象绑定参数。
>
>@DataBinder
>
>——用于修饰方法，该方法会在Controller方法执行前被调用，用于绑定参数的转换器。

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200315215251294.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

随着模块越来越多，可能要对某个模块所有的方法记录日志。

方法1:将记录日志的方法在函数最前面调用
弊端：记录日志不属于业务需求，属于系统需求。业务方法里面耦合系统需求，如果有一天系统需求发生变化，加入某一天需要将所有的记录日志方法改到处理业务方法后面，则每个业务方法都需要改变。

解决方法：AOP，将改方法独立出去。

添加切面类，设置前置方法处理所有Service方法：

~~~java
@Component
@Aspect
public class ServiceLogAspect {

    private static final Logger logger = LoggerFactory.getLogger(ServiceLogAspect.class);

    //所有的service方法
    @Pointcut("execution(* com.nowcoder.community.service.*.*(..))")
    public void pointcut(){

    }

    @Before("pointcut()")
    public void before(JoinPoint joinPoint){ //连接点参数
        //日志格式：用户[xxx],在时间[y]访问了[com.nowcoder.community.service.xxx()]
        ServletRequestAttributes attributes = (ServletRequestAttributes) RequestContextHolder.getRequestAttributes();
        HttpServletRequest request = attributes.getRequest();
        //获取用户ip
        String ip = request.getRemoteHost();
        //格式化时间
        String now =new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
        //通过JoinPoint对象获取 类和方法名
        String target = joinPoint.getSignature().getDeclaringTypeName() + "."+joinPoint.getSignature().getName();

        logger.info(String.format("用户[%s],在[%s],访问了[%s]",ip,now,target));
    }
}
~~~



## 第五章：Redis，一站式高性能存储方案

#### 1. Redis入门

Redis是一款基于键值对的NoSQL数据库，它的值支持多种数据结构：字符串(strings)、哈希(hashes)、列表(lists)、集合(sets)、有序集合(sorted sets)等。

Redis将所有的数据都存放在内存中，所以它的读写性能十分惊人。 同时，Redis还可以将内存中的数据以快照或日志的形式保存到硬盘上，以保证数据的安全性。

Redis典型的应用场景包括：缓存、排行榜、计数器、社交网络、消息队列等。

1. 连接redis

   ![image-20200613170942596](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613170942596.png)

2. 内置16个库，使用select 切换

   ![image-20200613171203598](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613171203598.png)

3. 如何在库里操作**字符串**类型，存，取，加1，减1

   ![image-20200613171542306](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613171542306.png)

4. 如何在库里操作**哈希**类型

   ![image-20200613171842976](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613171842976.png)

5. 如何在库里操作**列表**类型，列表是一个横向的容器，可以从左边取，也可以从右边取。（可以模拟栈和队列）

   ![image-20200613172720439](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613172720439.png)

6. 如何在库里操作**集合**类型，可以随机弹出一个元素-抽奖

   ![image-20200613173345601](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613173345601.png)

7. 如何在库里操作**有序集合**类型，可以按照分数排序

   ![image-20200613180130983](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613180130983.png)

   

   8. 全局命令

      ![image-20200613181027596](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200613181027596.png)

      

#### 2. Spring整合Redis

>引入依赖
>
>——spring-boot-starter-data-redis
>
>配置Redis
>
>——配置数据库参数
>
>——编写配置类，构造RedisTemplate
>
>访问Redis
>
>——redisTemplate.opsForValue()
>
>——redisTemplate.opsForHash()
>
>——redisTemplate.opsForList()
>
>——redisTemplate.opsForSet()
>
>——redisTemplate.opsForZSet()

1. 引入依赖，spring-boot和spring整合的包可以不写版本号，

~~~xml
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
		</dependency>
~~~

父pom声明了很多子包的版本，**默认版本可以保证互相兼容**

~~~xml
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.1.5.RELEASE</version>
		<relativePath/> <!-- lookup parent from repository -->
	</parent>
~~~

2. 配置参数

~~~properties
# RedisProperties
//使用16个数据库的哪一个库
spring.redis.database=11
//ip
spring.redis.host=localhost
//端口
spring.redis.port=6379
~~~

3. 编写配置类

~~~java
package com.nowcoder.community.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.RedisSerializer;

@Configuration
public class RedisConfig {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(factory);

        // 设置key的序列化方式
        template.setKeySerializer(RedisSerializer.string());
        // 设置value的序列化方式
        template.setValueSerializer(RedisSerializer.json());
        // 设置hash的key的序列化方式
        template.setHashKeySerializer(RedisSerializer.string());
        // 设置hash的value的序列化方式
        template.setHashValueSerializer(RedisSerializer.json());

        template.afterPropertiesSet();
        return template;
    }

~~~

4. 测试

~~~java
package com.nowcoder.community;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.dao.DataAccessException;
import org.springframework.data.redis.core.BoundValueOperations;
import org.springframework.data.redis.core.RedisOperations;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.SessionCallback;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.concurrent.TimeUnit;

@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = CommunityApplication.class)
public class RedisTests {

    @Autowired
    private RedisTemplate redisTemplate;
//访问string
    @Test
    public void testStrings() {
        String redisKey = "test:count";

        redisTemplate.opsForValue().set(redisKey, 1);

        System.out.println(redisTemplate.opsForValue().get(redisKey));
        System.out.println(redisTemplate.opsForValue().increment(redisKey));
        System.out.println(redisTemplate.opsForValue().decrement(redisKey));
    }
//访问hash
    @Test
    public void testHashes() {
        String redisKey = "test:user";

        redisTemplate.opsForHash().put(redisKey, "id", 1);
        redisTemplate.opsForHash().put(redisKey, "username", "zhangsan");

        System.out.println(redisTemplate.opsForHash().get(redisKey, "id"));
        System.out.println(redisTemplate.opsForHash().get(redisKey, "username"));
    }
//
    @Test
    public void testLists() {
        String redisKey = "test:ids";

        redisTemplate.opsForList().leftPush(redisKey, 101);
        redisTemplate.opsForList().leftPush(redisKey, 102);
        redisTemplate.opsForList().leftPush(redisKey, 103);

        System.out.println(redisTemplate.opsForList().size(redisKey));
        System.out.println(redisTemplate.opsForList().index(redisKey, 0));
        System.out.println(redisTemplate.opsForList().range(redisKey, 0, 2));

        System.out.println(redisTemplate.opsForList().leftPop(redisKey));
        System.out.println(redisTemplate.opsForList().leftPop(redisKey));
        System.out.println(redisTemplate.opsForList().leftPop(redisKey));
    }

    @Test
    public void testSets() {
        String redisKey = "test:teachers";

        redisTemplate.opsForSet().add(redisKey, "刘备", "关羽", "张飞", "赵云", "诸葛亮");

        System.out.println(redisTemplate.opsForSet().size(redisKey));
        System.out.println(redisTemplate.opsForSet().pop(redisKey));
        System.out.println(redisTemplate.opsForSet().members(redisKey));
    }

    @Test
    public void testSortedSets() {
        String redisKey = "test:students";

        redisTemplate.opsForZSet().add(redisKey, "唐僧", 80);
        redisTemplate.opsForZSet().add(redisKey, "悟空", 90);
        redisTemplate.opsForZSet().add(redisKey, "八戒", 50);
        redisTemplate.opsForZSet().add(redisKey, "沙僧", 70);
        redisTemplate.opsForZSet().add(redisKey, "白龙马", 60);

        System.out.println(redisTemplate.opsForZSet().zCard(redisKey));
        System.out.println(redisTemplate.opsForZSet().score(redisKey, "八戒"));
        System.out.println(redisTemplate.opsForZSet().reverseRank(redisKey, "八戒"));
        System.out.println(redisTemplate.opsForZSet().reverseRange(redisKey, 0, 2));
    }

    @Test
    public void testKeys() {
        redisTemplate.delete("test:user");

        System.out.println(redisTemplate.hasKey("test:user"));

        redisTemplate.expire("test:students", 10, TimeUnit.SECONDS);
    }

    // 多次访问同一个key
    @Test
    public void testBoundOperations() {
        String redisKey = "test:count";
        BoundValueOperations operations = redisTemplate.boundValueOps(redisKey);
        operations.increment();
        operations.increment();
        operations.increment();
        operations.increment();
        operations.increment();
        System.out.println(operations.get());
    }

    // 编程式事务
    @Test
    public void testTransactional() {
        Object obj = redisTemplate.execute(new SessionCallback() {
            @Override
            public Object execute(RedisOperations operations) throws DataAccessException {
                String redisKey = "test:tx";

                operations.multi();

                operations.opsForSet().add(redisKey, "zhangsan");
                operations.opsForSet().add(redisKey, "lisi");
                operations.opsForSet().add(redisKey, "wangwu");

                System.out.println(operations.opsForSet().members(redisKey));

                return operations.exec();
            }
        });
        System.out.println(obj);
    }

}
~~~

#### 3.  点赞

>点赞 ：
>
>——支持对帖子、评论点赞。
>
>——第1次点赞，第2次取消点赞。
>
>首页点赞数量 ：
>
>——统计帖子的点赞数量。
>
>详情页点赞数量 ：
>
>——统计点赞数量。
>
>——显示点赞状态。

**点赞功能的实现**

点赞功能需要考虑性能的问题：很多人同时给一个帖子点赞
因此把**点赞数据存到Redis里**会提升性能

首先写一个生成RedisKey的工具：RedisKeyUtil**点赞使用Set集合存储数据**：可以看到是谁点的赞，集合中装的是UserId。

~~~java
package com.nowcoder.community.util;

public class RedisKeyUtil {

    private static final String SPLIT = ":";
    private static final String PREFIX_ENTITY_LIKE = "like:entity";
    private static final String PREFIX_USER_LIKE = "like:user";
    private static final String PREFIX_FOLLOWEE = "followee";
    private static final String PREFIX_FOLLOWER = "follower";
    private static final String PREFIX_KAPTCHA = "kaptcha";
    private static final String PREFIX_TICKET = "ticket";
    private static final String PREFIX_USER = "user";

    // 某个实体的赞
    // like:entity:entityType:entityId -> set(userId)
    public static String getEntityLikeKey(int entityType, int entityId) {
        return PREFIX_ENTITY_LIKE + SPLIT + entityType + SPLIT + entityId;
    }

    // 某个用户的赞
    // like:user:userId -> int
    public static String getUserLikeKey(int userId) {
        return PREFIX_USER_LIKE + SPLIT + userId;
    }

    // 某个用户关注的实体
    // followee:userId:entityType -> zset(entityId,now)
    public static String getFolloweeKey(int userId, int entityType) {
        return PREFIX_FOLLOWEE + SPLIT + userId + SPLIT + entityType;
    }

    // 某个实体拥有的粉丝
    // follower:entityType:entityId -> zset(userId,now)
    public static String getFollowerKey(int entityType, int entityId) {
        return PREFIX_FOLLOWER + SPLIT + entityType + SPLIT + entityId;
    }

    // 登录验证码
    public static String getKaptchaKey(String owner) {
        return PREFIX_KAPTCHA + SPLIT + owner;
    }

    // 登录的凭证
    public static String getTicketKey(String ticket) {
        return PREFIX_TICKET + SPLIT + ticket;
    }

    // 用户
    public static String getUserKey(int userId) {
        return PREFIX_USER + SPLIT + userId;
    }

}
~~~

~~~java
private static final String SPLIT = ":"; //单词分隔符
    private static final String PREFIX_ENTITY_LIKE = "like:entity"; //实体的赞的前缀

    /**
     * 得到存某一个实体的赞的Key
     * 格式：like:entity:entityType:entityId  ->  set（userId）
     * @param entityType 实体（帖子、回帖）类型
     * @param entityId   实体ID
     * @return
     */
    public static String getEntityLikeKey(int entityType,int entityId){
        return PREFIX_ENTITY_LIKE + entityType + SPLIT +entityId;
    }
~~~

Service层：**实现点赞功能**

找到该实体点赞的Set，点赞人Id作为Key。如果已经点过赞，则点一下取消。如果没有点过赞，则点一下生成赞。

~~~java
//点赞
    public void like(int userId,int entityType,int entityId){
        //得到Key
        String entityLikeKey = RedisKeyUtil.getEntityLikeKey(entityType,entityId);
        //判断是否重复点赞
        boolean isMember = redisTemplate.opsForSet().isMember(entityLikeKey,userId);
        if(isMember){
            //如果已经点过赞，则取消赞
            redisTemplate.opsForSet().remove(entityLikeKey,userId);
        }else{
            //没点过赞，加入赞
            redisTemplate.opsForSet().add(entityLikeKey,userId)
        }
    }
~~~

**查询点赞数量**：

~~~java
/查询实体点赞数量
    public long findEntityLikeCount(int entityType,int entityId){
        //得到Key
        String entityLikeKey = RedisKeyUtil.getEntityLikeKey(entityType,entityId);
        return redisTemplate.opsForSet().size(entityLikeKey);
    }
~~~

**查询点赞状态**：

~~~java
//查询某人对某实体的点赞状态
    //返回int值是为了点踩
    public int findEntityLikeStatus(int userId,int entityType,int entityId){
        //得到Key
        String entityLikeKey = RedisKeyUtil.getEntityLikeKey(entityType,entityId);
        return redisTemplate.opsForSet().isMember(entityLikeKey,userId)?1:0;
    }
~~~

**Controller**

~~~java
@RequestMapping(path = "/like",method = RequestMethod.POST)
    @ResponseBody
    public String like(int entityType,int entityId){
        User user = hostHolder.getUser();
        
        //点赞
        likeService.like(user.getId(),entityType,entityId);
        
        //数量
        long likeCount = likeService.findEntityLikeCount(entityType,entityId);
        
        //状态
        int likeStatus = likeService.findEntityLikeStatus(user.getId(),entityType,entityId);

        //封装返回结果
        Map<String,Object> map = new HashMap<>();
        map.put("likeCount",likeCount);
        map.put("likeStatus",likeStatus);
        
        return CommunityUtil.getJSONString(0,null,map);
    }
~~~

实现首页赞的数量的显示：
在HomeController里面查找每个帖子的详细信息里加入查找点赞数量的逻辑：

~~~java
//搜寻帖子对应的点赞数加入Map
long likeCount = likeService.findEntityLikeCount(ENTITY_TYPE_POST,post.getId());
map.put("likeCount",likeCount);
~~~

在帖子详情页面逻辑里面，在搜寻每个帖子、评论、回复的逻辑里，均加入点赞数量和点赞状态的获取：

~~~java
likeCount = likeService.findEntityLikeCount(ENTITY_TYPE_COMMENT,comment.getId());
                commentVo.put("likeCount",likeCount);
                //当前用户点赞状态(如果没登陆直接返回0)
                likeStatus = hostHolder.getUser()==null?0:likeService.findEntityLikeStatus(hostHolder.getUser().getId(), ENTITY_TYPE_COMMENT,comment.getId());
                commentVo.put("likeStatus",likeStatus);
~~~

#### 4. 收到的赞

>重构点赞功能：
>
>——以用户为key，记录点赞数量
>
>——increment(key)，decrement(key)
>
>开发个人主页
>
>——以用户为key，查询点赞数量

我收到的赞
如何统计每个人收到的赞的总数？
–将用户和点赞绑定，每次点赞的时候给相应的用户加一，存在Redis里
重构点赞功能：
1、在RedisKeyUtil里面增加User点赞数的PREFIX定义：

~~~java
private static final String PREFIX_USER_LIKE = "like:user"; //用户收到的赞
~~~

Key的构建方法：

~~~java
/**
     * 某个用户收到的赞的Key
     * 格式：like:user:userId  ->  set（userId）
     * @return
     */
    public static String getUserLikeKey(int userId){
        return PREFIX_ENTITY_LIKE  + SPLIT + userId;
    }
~~~

2、重构点赞方法：**加上Redis的事务处理**（同时进行对实体和事务的点赞和撤销）
要点：查询是否点过赞的操作要放在事务的外面
原因：Redis事务比较特殊，如果查询写在事务范围内，只是把命令放到队列里，没有立即执行。统一提交再执行。

```java
    //点赞
    public void like(int userId,int entityType,int entityId,int entityUserId){
    //重构：两次Redis插入要新建事务
    redisTemplate.execute(new SessionCallback() {
        @Override
        public Object execute(RedisOperations redisOperations) throws DataAccessException {
            //得到实体的Key
            String entityLikeKey = RedisKeyUtil.getEntityLikeKey(entityType,entityId);
            //得到用户的Key
            String userLikeKey = RedisKeyUtil.getUserLikeKey(entityUserId);
            //判断是否重复点赞
            boolean isMember = redisOperations.opsForSet().isMember(entityLikeKey,userId);

            //Redis事务比较特殊，如果查询写在事务范围内，只是把命令放到队列里，没有立即执行。统一提交再执行
            //所以查询需要放在事务外面
            redisOperations.multi(); //开启事务
            if(isMember){
                //如果已经点过赞，则取消赞
                redisOperations.opsForSet().remove(entityLikeKey,userId);
                //赞减1
                redisOperations.opsForValue().decrement(userLikeKey);

            }else{
                //没点过赞，加入赞
                redisOperations.opsForSet().add(entityLikeKey,userId);
                redisOperations.opsForValue().increment(userLikeKey);
            }
            return redisOperations.exec(); //执行事务
        }
    });
}
```


返回用户的赞的Service方法：

~~~java
//查询某个用户获得的赞
    public int findUserLikeCount(int UserId){
        //得到用户的Key
        String userLikeKey = RedisKeyUtil.getUserLikeKey(UserId);
        Integer count = (Integer)redisTemplate.opsForValue().get(userLikeKey);
        return count == null ? 0: count.intValue();
    }
~~~

Controller:点击个人主页
功能：通过一个人的头像，点击进入（自己/其他用户）主页。包含用户个人信息、被点赞数量

```java
 @RequestMapping(path = "/profile/{userId}",method = RequestMethod.GET)
    public String getProfilePage(@PathVariable("userId") int userId,Model model){
        User user = userService.findUserById(userId);
        if(user == null){
            throw new RuntimeException("该用户不存在");
        }
 //用户基本信息发送给页面
    model.addAttribute("user",user);

    //用户获赞数量
    int likeCount = likeService.findUserLikeCount(userId);
    model.addAttribute("likeCount",likeCount);

    return "/site/profile";
}
```

#### 5. 关注、取消关注

>需求
>
>——开发关注、取消关注功能。
>
>——统计用户的关注数、粉丝数。
>
>关键
>
>——若A关注了B，则A是B的Follower（粉丝），B是A的Followee（目标）。
>
>——关注的目标可以是用户、帖子、题目等，在实现时将这些目标抽象为实体。

**关注、取关、个人主页显示关注数**

**需要将关注与被关注的信息存在Redis里**
个人猜测：因为随手点赞关注这些内容发生频率高，存在Redis里面较为合适，避免访问数据库

**首先定义RedisKeyUtil包里面的Prefix和生成Key的方法**：

~~~java
private static final String PREFIX_FOLLOWEE = "followee";   //用户/实体的关注者
private static final String PREFIX_FOLLOWER = "follower";   //用户/实体的粉丝
~~~

~~~java
/**
     * 某个用户关注的实体
     * followee:userId:entityType -> Zset(entityId,now)  //zset满足排序，当前时间now作为分数排序（有按照时间罗列被关注者的需求）
     */
    public static String getFolloweeKey(int userId,int entityType){
        return PREFIX_FOLLOWEE+SPLIT+userId+SPLIT+entityType;
    }

    /**
     * 某个用户拥有的粉丝
     * follower：entityType：entityId -> zset(userId,now)
     */
    public static String getFollowerKey(int entityType,int entityId){
        return PREFIX_FOLLOWER+SPLIT+entityType+SPLIT+entityId;
    }
~~~

其中，在follow别人的时候需要同时把自己的信息加入别人的Follower中，并且在自己的Flowee中加入被关注人，这样便于查找。
**定义Service层的方法（关注、取关）**：
因为有两个Redis操作，所以要引入事务：

~~~java
/**
     * 关注
     * @param userId
     * @param entityType
     * @param entityId
     */
    public void follow(int userId,int entityType,int entityId){
        redisTemplate.execute(new SessionCallback() {

            @Override
            public Object execute(RedisOperations redisOperations) throws DataAccessException {
                String followeeKey = RedisKeyUtil.getFolloweeKey(userId,entityType);
                String followerKey = RedisKeyUtil.getFollowerKey(entityType,entityId);

                redisOperations.multi();

                redisOperations.opsForZSet().add(followeeKey, entityId, System.currentTimeMillis());
                redisOperations.opsForZSet().add(followerKey, userId, System.currentTimeMillis());

                return redisOperations.exec();
            }

        });
    }

    /**
     * 取关
     * @param userId
     * @param entityType
     * @param entityId
     */
    public void unfollow(int userId,int entityType,int entityId){
        redisTemplate.execute(new SessionCallback() {

            @Override
            public Object execute(RedisOperations redisOperations) throws DataAccessException {
                String followeeKey = RedisKeyUtil.getFolloweeKey(userId,entityType);
                String followerKey = RedisKeyUtil.getFollowerKey(entityType,entityId);

                redisOperations.multi();

                redisOperations.opsForZSet().remove(followeeKey, entityId);
                redisOperations.opsForZSet().remove(followerKey, userId);

                return redisOperations.exec();
            }
        });
    }
~~~

Service层查询关注数量、被关注数量、是否关注：

/**
     * 查询关注的实体数量
     * @param userId        用户ID
     * @param entityType    关注类型
     * @return
     */
    public long findFolloweeCount(int userId,int entityType){
        String followeeKey = RedisKeyUtil.getFolloweeKey(userId,entityType);
        return redisTemplate.opsForZSet().zCard(followeeKey);
    }

    /**
     * 查询当前实体粉丝数量
     * @param entityType
     * @param entityId
     * @return
     */
    public long findFollowerCount(int entityType,int entityId){
        String followerKey = RedisKeyUtil.getFollowerKey(entityType,entityId);
        return redisTemplate.opsForZSet().zCard(followerKey);
    }
    
    /**
     * 查询当前用户是否已经关注某实体
     * @param userId
     * @param entityType
     * @param entityId
     * @return
     */
    public boolean hasFollowed(int userId,int entityType,int entityId){
        String followeeKey = RedisKeyUtil.getFolloweeKey(userId,entityType);
        return redisTemplate.opsForZSet().score(followeeKey,entityId)!=null;
    }

Controller：

    @RequestMapping(path = "/follow",method = RequestMethod.POST)
        @ResponseBody
        public String follow(int entityType ,int entityId){
            User user = hostHolder.getUser();
            followService.follow(user.getId(),entityType,entityId);
            return CommunityUtil.getJSONString(0,"已经关注");
        }
    @RequestMapping(path = "/unfollow",method = RequestMethod.POST)
    @ResponseBody
    public String unfollow(int entityType ,int entityId){
        User user = hostHolder.getUser();
    
        followService.unfollow(user.getId(),entityType,entityId);
    
        return CommunityUtil.getJSONString(0,"已经取消关注");
    }

在用户界面显示关注人数、粉丝人数、是否已经关注当前人：

       //关注数量
            long followeeCount = followService.findFolloweeCount(userId,ENTITY_TYPE_USER);
            model.addAttribute("followeeCount",followeeCount);
       //粉丝数量
        long followerCount = followService.findFollowerCount(ENTITY_TYPE_USER,userId);
        model.addAttribute("followerCount",followerCount);
    
        //是否已经关注这个人
        boolean hasFollowed = false;
        if(hostHolder.getUser()!=null){
            hasFollowed = followService.hasFollowed(hostHolder.getUser().getId(),ENTITY_TYPE_USER,userId);
        }
        model.addAttribute("hasFollowed",hasFollowed);

**关注列表、取消列表**
service层操作Redis对关注人、被关注人进行查询：
1、需要分页，与对Mysql的查询不同
2、传给前端的数据是用户基本信息和关注时间
3、规定Zset通过插入时间进行排序，这里可以通过Score（）函数得到一个double类型的变量

```java
  /**
     * 查询某个用户关注的人
     * 需要将user对象和关注时间传给前端
     */
    public List<Map<String,Object>> findFollowees(int userId,int offset,int limit){
        //key
        String followeeKey = RedisKeyUtil.getFolloweeKey(userId,ENTITY_TYPE_USER);
        //查关注的人的数据：redis分页查询两个参数是起始下标和截止下标
        Set<Integer> targetIds = redisTemplate.opsForZSet().reverseRange(followeeKey,offset,offset+limit-1);
        if(targetIds == null){
            return null;
        }
List<Map<String,Object>> list = new ArrayList<>();
    for(Integer targetId:targetIds){
        Map<String,Object> map = new HashMap<>();
        User user = userService.findUserById(targetId);
        map.put("user",user);
        Double score = redisTemplate.opsForZSet().score(followeeKey,targetId);
        map.put("followTime",new Date(score.longValue()));
        list.add(map);
    }
    return list;
}

/**
 * 查询某个用户的粉丝
 */
public List<Map<String,Object>> findFollowers(int userId,int offset,int limit){
    //key
    String followerKey = RedisKeyUtil.getFolloweeKey(userId,ENTITY_TYPE_USER);
    Set<Integer> targetIds = redisTemplate.opsForZSet().reverseRange(followerKey,offset,offset+limit-1);
    
    if(targetIds==null){
        return null;
    }

    List<Map<String,Object>> list = new ArrayList<>();
    for(Integer targetId:targetIds){
        Map<String,Object> map = new HashMap<>();
        User user = userService.findUserById(targetId);
        map.put("user",user);
        Double score = redisTemplate.opsForZSet().score(followerKey,targetId);
        map.put("followTime",new Date(score.longValue()));
        list.add(map);
    }
    return list;
}
```

#### 6. 关注列表、粉丝列表

>业务层：
>
>——查询某个用户关注的人，支持分页。
>
>——查询某个用户的粉丝，支持分页。
>
>表现层 ：
>
>——处理“查询关注的人”、“查询粉丝”请求。
>
>——编写“查询关注的人”、“查询粉丝”模板。

Controller：
还是查找关注着和被关注着的列表：
1、网页需要当前用户部分信息（用户名等），先查询当前用户信息
2、设置分页条件
3、遍历关注者/被关注着的链表，因为页面上每个用户后面都要判断当前用户是否关注了他，有个按钮（
问题：为什么这个逻辑不放在Service层处理、关注者列表完全不需要查找，全部默认是true，这里是多余的操作（未解决）
）

```java
   /**
     * 查找关注者列表
     * @param userId
     * @param page
     * @param model
     * @return
     */
    @RequestMapping(path = "/followees/{userId}",method = RequestMethod.GET)
    public String getFollowees(@PathVariable("userId")int userId, Page page, Model model){
        //查找User的原因是页面需要User的部分信息（比如名字，页面需要显示：xxx的关注列表）
        User user = userService.findUserById(userId);
        if(user == null){
            throw new RuntimeException("该用户不存在");
        }
        model.addAttribute("user",user);
   //设置分页条件
    page.setLimit(5);
    page.setPath("/followees/"+userId);
    page.setRows((int)followService.findFolloweeCount(userId,ENTITY_TYPE_USER));

    List<Map<String,Object>> userList = followService.findFollowees(userId,page.getOffset(),page.getLimit());
    if(userList != null){
        for(Map<String,Object> map : userList){
            User u = (User)map.get("user");
            map.put("hasFollowed",hasFollowed(u.getId()));
        }
    }
    model.addAttribute("users",userList);
    return "/site/followee";
}
```

    /**
     * 判断当前用户是否关注某个用户
     * @param userId
     * @return
     */
    private boolean hasFollowed(int userId){
        if(hostHolder.getUser() == null){
            return false;
        }
        return followService.hasFollowed(hostHolder.getUser().getId(),ENTITY_TYPE_USER,userId);
    }

#### 7. 优化登录模块

>使用Redis存储验证码:
>
>——验证码需要频繁的访问与刷新，对性能要求较高。
>
>——验证码不需永久保存，通常在很短的时间后就会失效。
>
>——分布式部署时，存在Session共享的问题。
>
>使用Redis存储登录凭证：
>
>——处理每次请求时，都要查询用户的登录凭证，访问的频率非常高。
>
>使用Redis缓存用户信息：
>
>——处理每次请求时，都要根据凭证查询用户信息，访问的频率非常高。

**一、使用Redis存储验证码**
验证码需要频繁的访问和刷新，对性能要求较高，而且验证码不需要永久保存，很短时间就失效（Redis可以设置失效时间）分布式部署时，存在Session共享的问题
1、设置前缀：

~~~java
private static final String PREFIX_KAPTCHA = "kaptcha"; //验证码
~~~

2、设置获得Key的方法：
用户在输入验证码的时候还没有登陆，如何将这个用户与对应的验证码联系起来？

给用户发一个凭证存到cookie，通过这个方式将验证码和用户绑定：

~~~java
/**
     * 登陆验证码
     */
    public static String getKaptchaKey(String owner){
        return PREFIX_KAPTCHA+owner;
    }
~~~

修改生成验证码方法：
原来是将验证码存入Session。HttpSession对象通过程序调用销毁方法invalidate()，或者过期，或者服务端进程被终止才能销毁。HttpSession对象存在内存中。

```java
    @RequestMapping(path = "/kaptcha",method = RequestMethod.GET)
    public void getKaptcha(HttpServletResponse response/*, HttpSession session */){
        // 生成验证码
        String text = kaptchaProducer.createText();
        BufferedImage image = kaptchaProducer.createImage(text);
    // 将验证码存入session
    //session.setAttribute("kaptcha",text);
    
    //生成凭证，传入Cookie
    String kaptchaOwner = CommunityUtil.generateUUID();
    Cookie cookie = new Cookie("kaptchaOwner",kaptchaOwner);
    cookie.setMaxAge(60); //生存时间60s
    cookie.setPath(contextPath); //全路径有效
    response.addCookie(cookie); //添加cookie
    
    //将验证码存入Redis
    String redisKey = RedisKeyUtil.getKaptchaKey(kaptchaOwner); 
    redisTemplate.opsForValue().set(redisKey,text,60, TimeUnit.SECONDS);
            // 将图片输出给浏览器
    response.setContentType("image/png");
    try {
        // 使用response输出流输出
        OutputStream os = response.getOutputStream();
        ImageIO.write(image, "png", os);
    } catch (IOException e) {
        logger.error("响应验证码失败："+e.getMessage());
    }
}
```

修改Controller里面的Login方法：
1、参数增加从Cookie里面取的凭证：

~~~java
@CookieValue("kaptchaOwner") String kaptchaOwner
~~~


2、将取验证码的方法从Session里面取改为根据Cookie得到的key从Redis里面取：

        //判断验证码
        //String kaptcha = (String)session.getAttribute("kaptcha");
        
        String kaptcha = null;
        if(StringUtils.isBlank(kaptchaOwner)){
            //得到key
            String redisKey = RedisKeyUtil.getKaptchaKey(kaptchaOwner);
            kaptcha = (String)redisTemplate.opsForValue().get(redisKey);
        }

二、使用Redis存储登陆凭证
处理每次请求时，都要查询用户登陆凭证，访问频率很高
之前使用LoginTicket表来存储用户Ticket、ticket状态、过期时间来处理这些数据。通过Redis存可以废弃掉这个表以及对应的Dao的类。
只改状态而不是删除Ticket数据是因为以后某些功能可能需要查询用户最早什么时候登陆，一年登陆多少次之类的功能
1	、生成凭证和生成Key的方式

~~~java
private static final String PREFIX_TICKET = "ticket"; //验证码
~~~

~~~java
/**
     * 登陆凭证
     */
    public static String getTicketKey(String ticket){
        return PREFIX_TICKET+ticket;
    }
~~~

2、存Ticket：直接存到Redis里面，Redis自动将loginTicket对象序列化为String

~~~java
//存Ticket
      //loginTicketMapper.insertLoginTicket(loginTicket);
        String redisKey = RedisKeyUtil.getTicketKey(loginTicket.getTicket());
        redisTemplate.opsForValue().set(redisKey,loginTicket); //redis可以吧loginTicket对象序列化为字符串
~~~

3、修改状态：

```java
public void logout(String ticket){
//loginTicketMapper.updateStatus(ticket,1);
    String redisKey = RedisKeyUtil.getTicketKey(ticket);
    //取出字符串，强转为对象，修改状态，再存入
    redisTemplate.opsForValue().get(redisKey);
    LoginTicket loginTicket = (LoginTicket)redisTemplate.opsForValue().get(redisKey);
    loginTicket.setStatus(1);
    redisTemplate.opsForValue().set(redisKey,loginTicket)     
}
4、查找Ticket
```


​        

```java
public LoginTicket findLoginTicket(String ticket){
//return loginTicketMapper.selectByTicket(ticket);
    String redisKey = RedisKeyUtil.getTicketKey(ticket);
    LoginTicket loginTicket = (LoginTicket)redisTemplate.opsForValue().get(redisKey);
    return loginTicket;
    
}
```

三、使用Redis缓存用户信息
处理每次请求时，都要根据凭证查询用户信息，访问频率非常高
需要考虑的方面：

如果在缓存中找不到对应User数据，则从数据库取，再存入缓存
如果修改数据库中对应的User数据，则也要刷新缓存中的数据(采用直接删掉，下次访问再加载的方法（如果更新会出现并发的问题？？？）)
前缀和取Key：

~~~java
private static final String PREFIX_USER = "user"; //用户信息
~~~

~~~java
/**

    用户
     */
         public static String getTicketKey(int userId){
     return PREFIX_TICKET+SPLIT+userId;
         }
~~~

定义从缓存取、从数据库查并加载至缓存、清除缓存对应数据三个基础方法，再在别的方法里面调用，覆盖以前操作数据库的语句：

    //1、优先从缓存中取值
        private User getCache(int userId){
            String redisKey = RedisKeyUtil.getTicketKey(userId);
            return (User)redisTemplate.opsForValue().get(redisKey);
        }
    //2、取不到，往缓存存数据
    private User initCache(int userId){
        //查
        User user = userMapper.selectById(userId);
        //存缓存
        String redisKey = RedisKeyUtil.getTicketKey(userId);
        redisTemplate.opsForValue().set(redisKey,user,36000, TimeUnit.SECONDS);
        return  user;
    }
    
    //3、数据变更清缓存
    private void clearCache(int userId){
        String redisKey = RedisKeyUtil.getTicketKey(userId);
        redisTemplate.delete(redisKey);
    }

修改查询User函数：

```java
 public User findUserById(int id) {
 //return userMapper.selectById(id);
    
    //从缓存取
    User user = getCache(id);
    //如果没有，从数据库取加载值缓存
    if(user == null){
        user = initCache(id);
    }
    //返回
    return user;
}
```

修改更新头像操作
先更新数据库，再清缓存，避免先清理缓存数据库操作失败，反而把缓存清掉的情况。

~~~java
public int updateHeader(int userId,String headerUrl){
       // return userMapper.updateHeader(userId,headerUrl);
        //更新返回行数
        int rows = userMapper.updateHeader(userId,headerUrl);
        clearCache(userId);
        return rows;
    }
~~~

## 第六章：Kafka，构建TB级异步消息系统

#### 1. 阻塞队列

>BlockingQueue ：
>
>——解决线程通信的问题。
>
>——阻塞方法：put、take。
>
>生产者消费者模式：
>
>——生产者：产生数据的线程。
>
>——消费者：使用数据的线程。
>
>实现类 ：
>
>——ArrayBlockingQueue
>
>——LinkedBlockingQueue
>
>——PriorityBlockingQueue、SynchronousQueue、DelayQueue等。 

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/2020031820363281.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

**BlockingQueue**：解决线程通信的一个接口。如图所示，如果线程1和线程2想要通信，线程1将数据放到BlockingQueue，线程2从阻塞队列里面取。这就是操作系统里面的生产者、消费者模式。在生产者、消费者之间起到缓冲作用，避免浪费CPU资源。

不使用阻塞队列的弊端：
如果生产者生产速度快、消费者消费速度慢的话，很快就会达到性能瓶颈。线程2没处理完数据，线程1 依旧在不停的生产数据、没有被阻塞，则会占用CPU资源，造成浪费。
如果消费者消费的速度更快，生产者还没有给出数据的时候消费者没有被阻塞，则消费者会占用CPU资源。

使用阻塞队列：队列满，生产者线程被阻塞，不占用系统资源。队列空相反。

#### 2. Kafka入门

>Kafka简介:
>
>——Kafka是一个分布式的流媒体平台。
>
>——应用：消息系统、日志收集、用户行为追踪、流式处理。
>
>Kafka特点 ：
>
>——高吞吐量、消息持久化、高可靠性、高扩展性。
>
>Kafka术语：
>
>——Broker、Zookeeper - Topic、Partition、Offset - Leader Replica 、Follower Replica

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200318213618776.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

性能：

**消息持久化**：会把数据存到硬盘上，而不是简单的存到内存里。

**高吞吐量**：因为存到硬盘里，硬盘价格低、存数量大，是存储海量数据的前提。硬盘的顺序读取高于对内存的随机读取。所以对硬盘进行IO不一定一定性能低。Kafka对硬盘的读取都是顺序进行的，保证了效率。

**高可靠性**：在分布式环境，可以做集群部署，存在容错能力

**高扩展性**：简单的部署到新服务器上

术语：

**Broker**：指服务器
**Zookeeper**：一个独立的软件，用来管理集群
**Topic**：
点对点模式：一个数据被一个消费者读取
发布订阅模式：多个消费者关注一个位置（Topic存放消息的位置），可以同时/先后读到这个位置的数据。
**Partition**：分区，对主题位置的分区，可以同时向多个分区写数据，这样提高了并发能力
**OffSe**t：消息在分区内存的索引，可以根据索引读数据
**Replica**：备份，通过副本的形式对数据进行存（Leader：主副本 Follower：随从副本）

1. 启动zokeeper

![image-20200614093411448](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200614093411448.png)

2. 启动kafka

![image-20200614093753831](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200614093753831.png)

3. 创建一个主题

   p is not a recognized option：https://blog.csdn.net/qq_34291242/article/details/100033040c

4. Connection to node -1 (localhost/127.0.0.1:9092) could not be established. Broker may not be available.

https://blog.csdn.net/Rand_C/article/details/84928232?utm_medium=distribute.pc_relevant.none-task-blog-baidujs-1

>kafka-topics.bat --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test

解决：https://blog.csdn.net/w546097639/article/details/88578635?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-17.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-17.nonecase

#### 3. Spring整合Kafka

>引入依赖：
>
>—— spring-kafka
>
>配置Kafka：
>
>——配置server、consumer
>
>访问Kafka ：
>
>——生产者 
>
>   kafkaTemplate.send(topic, data);
>
>——消费者 
>
>   @KafkaListener(topics = {"test"})
>
>   public void handleMessage(ConsumerRecord record) {}

1. 引入依赖

~~~xml
<dependency>
<groupId>org.springframework.kafka</groupId>
<artifactId>spring-kafka</artifactId>
</dependency>
~~~

2. 编写配置文件

```properties
# KafkaProperties
#端口号
spring.kafka.bootstrap-servers=localhost:9092
#分组ID
spring.kafka.consumer.group-id=community-consumer-group
#是否自动提交=是
spring.kafka.consumer.enable-auto-commit=true
#自动提交的频率
spring.kafka.consumer.auto-commit-interval=3000
```

3. 测试

~~~java
package com.nowcoder.community;

import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.stereotype.Component;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = CommunityApplication.class)
public class KafkaTests {

    @Autowired
    private KafkaProducer kafkaProducer;

    @Test
    public void testKafka() {
        kafkaProducer.sendMessage("test", "你好");
        kafkaProducer.sendMessage("test", "在吗");

        try {
            Thread.sleep(1000 * 10);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
//主动-生产
@Component
class KafkaProducer {

    @Autowired
    private KafkaTemplate kafkaTemplate;
    
    public void sendMessage(String topic, String content) {
        kafkaTemplate.send(topic, content);
    }

}
//被动-消费（自动）
@Component
class KafkaConsumer {

    @KafkaListener(topics = {"test"})
    
    public void handleMessage(ConsumerRecord record) {
        System.out.println(record.value());
    }
    
}
~~~

#### 4. 发送系统通知

>触发事件 ：
>
>——评论后，发布通知
>
>——点赞后，发布通知
>
>——关注后，发布通知
>
>处理事件 
>
>——封装事件对象
>
>——开发事件的生产者
>
>——开发事件的消费者

**发送通知的操作非常频繁，为了保障性能，使用kafka消息队列，三类不同事相当于三种不同的主题，生产者直接将消息扔入队列中，不需要等待。**

**这也叫做异步。**

**事件驱动的方式**

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/2020031916351786.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

因为有点赞、评论、关注三个类型，故创建三个不同的topic。一旦事件发生，便将事件包装成消息放到消息队列中。
消费者进程读到消息，往表里存数据。

**封装事件对象：**

~~~java
@Getter
public class Event {

    //事件的主题
    private String topic;
    //触发事件的人
    private int userId;
    //变更实体类型
    private int entityType;
    //变更实体Id
    private int entityId;
    //实体的作者
    private int entityUserId;

    //不知道以后需要什么其他字段，如果有，则封装到Map里，增加可扩展性
    private Map<String,Object> data = new HashMap<>();

    public String getTopic() {
        return topic;
    }

    /**
     * 这样写便于以后event.setxx().setyy().setzz()...这样循环赋值
     * @param topic
     * @return
     */
    public Event setTopic(String topic) {
        this.topic = topic;
        return this;
    }


    public Event setUserId(int userId) {
        this.userId = userId;
        return this;
    }


    public Event setEntityType(int entityType) {
        this.entityType = entityType;
        return this;
    }


    public Event setEntityId(int entityId) {
        this.entityId = entityId;
        return this;
    }


    public Event setEntityUserId(int entityUserId) {
        this.entityUserId = entityUserId;
        return this;
    }


    public Event setData(String key,Object value) {
        this.data.put(key,value);
        return this;
    }
}
~~~

**定义通知类型常量**：

~~~java
/**
     * 主题：评论
     */
    String TOPIC_COMMENT = "comment";
    /**
     * 主题：点赞
     */
    String TOPIC_LIKE = "like";
    /**
     * 主题：关注
     */
    String TOPIC_FOLLOW = "follow";
~~~

**定义生产者**：
Event对象->JSON字符串存入指定主题

~~~java
@Component
public class EventProducer {
    @Autowired
    private KafkaTemplate kafkaTemplate;

    //处理事件
    public void fireEvent(Event event){
        //将事件发布到指定的主题,将消息实体转化为JSON字符串输入队列
        kafkaTemplate.send(event.getTopic(), JSONObject.toJSONString(event));
    }
}
~~~

**定义消费者**：

- 系统的通知和私信一样存在Message表里面
- 由于都是系统发送给人，所以ConversationId值为主题
- 将消息队列里面的消息取出，经过处理存在Message表里面

~~~java
@Component
public class EventConsumer implements CommunityConstant {

    private static final Logger logger = LoggerFactory.getLogger(EventConsumer.class);

    @Autowired
    private MessageService messageService;

    @KafkaListener(topics = {TOPIC_COMMENT,TOPIC_LIKE,TOPIC_FOLLOW})
    public void handleCommentMessage(ConsumerRecord record){
        if(record == null || record.value()==null){
            logger.error("消息内容为空");
            return;
        }

        //将取出来的消息转成字符串
        Event event = JSONObject.parseObject(record.value().toString(),Event.class);
        if(event == null){
            logger.error("消息格式错误");
            return;
        }

        //实例化通知Message对象
        Message message = new Message();
        message.setFromId(SYSTEM_USER_ID);
        message.setToId(event.getEntityUserId());
        message.setConversationId(event.getTopic());
        message.setCreateTime(new Date());

        //Content: 将操作人，实体类型，实体ID等其他信息存入Map
        Map<String,Object> content = new HashMap<>();
        content.put("userId",event.getUserId());
        content.put("entityType",event.getEntityType());
        content.put("entityId",event.getEntityId());
        //Entity里面有一个map存其他信息，遍历它并且将它存入content的map：
        if(event.getData().isEmpty()){
            for(Map.Entry<String,Object> entry: event.getData().entrySet()){
                content.put(entry.getKey(),entry.getValue());
            }
        }
        message.setContent(JSONObject.toJSONString(content));
        
        messageService.addMessage(message);
    }

}
~~~

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200319193844583.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

如上图所示，我们应该修改CommentController、LikeController、FollowController三个类：
在CommentController中添加触发事件（在将评论写入数据库之后）：

~~~java
//构造触发评论事件
        Event event = new Event()
                .setTopic(TOPIC_COMMENT)                 //触发事件：评论
                .setUserId(hostHolder.getUser().getId()) //触发人
                .setEntityType(comment.getEntityType())  //实体类型：帖子
                .setEntityId(comment.getEntityId())     //实体id
                .setData("postId",discussPostId);       //？

        if(comment.getEntityType() == ENTITY_TYPE_POST){ //评论类型是帖子，通过帖子找到发帖人Id
           DiscussPost target =  discussPostService.findDiscussPostById(comment.getEntityId());
           event.setEntityUserId(target.getUserId());
        }else if(comment.getEntityType() == ENTITY_TYPE_COMMENT){
            Comment target = commentService.findCommentById(comment.getEntityId());
            event.setEntityUserId(target.getUserId());
        }
        
        eventProducer.fireEvent(event);
~~~

这是并发/异步的，该线程将事件丢到消息队列中之后，就不管了，直接处理返回给用户的页面的响应消息的发布由消息队列处理，起到一个缓冲作用。可能略有延迟。处理效率高。

**触发点赞事件**：

~~~java
//触发点赞事件（点赞才加入消息队列，取消赞不行）
        if(likeStatus == 1){
            
            Event event = new Event()
                    .setTopic(TOPIC_LIKE)
                    .setUserId(hostHolder.getUser().getId())
                    .setEntityType(entityType)
                    .setEntityId(entityId)
                    .setEntityUserId(entityUserId)
                    .setData("postId",postId);  //帖子Id
            eventProducer.fireEvent(event);
        }
~~~

**触发关注事件**：

~~~java
//触发关注事件
        Event event = new Event()
                .setTopic(TOPIC_FOLLOW)
                .setUserId(hostHolder.getUser().getId())
                .setEntityType(entityType)
                .setEntityId(entityId)
                .setEntityUserId(entityId);

        eventProducer.fireEvent(event);
~~~

#### 5. 显示系统通知

>通知列表：
>
>——显示评论、点赞、关注三种类型的通知
>
>通知详情 ：
>
>——分页显示某一类主题所包含的通知
>
>未读消息 ：
>
>——在页面头部显示所有的未读消息数量

![å¨è¿éæå¥å¾çæè¿°](https://img-blog.csdnimg.cn/20200319225556828.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MzMxMTkz,size_16,color_FFFFFF,t_70)

**Dao层**：定义三个方法，通过用户Id和topic（存在ConversationId）来查询用户总消息数量、未读消息数量、以及每一部分显示最新的通知内容。

~~~java
//查询某个主题下最新通知
    Message selectLatestNotice(int UserId,String topic);
    
    //查询某个主题所包含的总通知数量
    int selectNoticeCount(int UserId,String topic);
    
    //查询某个主题所包含的未读通知数量
    int selectNoticeUnreadCount(int userId,String topic);
~~~

**Service层**：直接调Mapper方法

~~~java
public Message finalLateNotice(int userId,String topic){
        return messageMapper.selectLatestNotice(userId,topic);
    }
    public int findNoticeCount(int userId,String topic){
        return messageMapper.selectNoticeCount(userId,topic);
    }
    public int findNoticeUnreadCount(int userId,String topic){
        return messageMapper.selectNoticeUnreadCount(userId,topic);
    }
~~~

**Controller层**：

~~~java
/**
     * 通知
     * @param model
     * @return
     */
    @RequestMapping(path = "/notice/list", method = RequestMethod.GET)
    public String getNoticeList(Model model) {
        User user = hostHolder.getUser();

        // 查询评论类通知
        //找到最新一条XX类通知，将该通知实体放入VO、将通知内容转义后转换成实体存入VO、通过ID查找到被评论人信息、评论实体ID以及评论实体类型等信息放入VO
        Message message = messageService.findLatestNotice(user.getId(), TOPIC_COMMENT);
        Map<String, Object> messageVO = new HashMap<>();
        if (message != null) {
            messageVO.put("message", message);

            String content = HtmlUtils.htmlUnescape(message.getContent());
            Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);

            messageVO.put("user", userService.findUserById((Integer) data.get("userId")));
            messageVO.put("entityType", data.get("entityType"));
            messageVO.put("entityId", data.get("entityId"));
            messageVO.put("postId", data.get("postId"));
            //通知总数
            int count = messageService.findNoticeCount(user.getId(), TOPIC_COMMENT);
            messageVO.put("count", count);
            //未读消息数量
            int unread = messageService.findNoticeUnreadCount(user.getId(), TOPIC_COMMENT);
            messageVO.put("unread", unread);
        }
        //将VO放入Model
        model.addAttribute("commentNotice", messageVO);

        // 查询点赞类通知
        message = messageService.findLatestNotice(user.getId(), TOPIC_LIKE);
        messageVO = new HashMap<>();
        if (message != null) {
            messageVO.put("message", message);

            String content = HtmlUtils.htmlUnescape(message.getContent());
            Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);

            messageVO.put("user", userService.findUserById((Integer) data.get("userId")));
            messageVO.put("entityType", data.get("entityType"));
            messageVO.put("entityId", data.get("entityId"));
            messageVO.put("postId", data.get("postId"));

            int count = messageService.findNoticeCount(user.getId(), TOPIC_LIKE);
            messageVO.put("count", count);

            int unread = messageService.findNoticeUnreadCount(user.getId(), TOPIC_LIKE);
            messageVO.put("unread", unread);
        }
        model.addAttribute("likeNotice", messageVO);

        // 查询关注类通知
        message = messageService.findLatestNotice(user.getId(), TOPIC_FOLLOW);
        messageVO = new HashMap<>();
        if (message != null) {
            messageVO.put("message", message);

            String content = HtmlUtils.htmlUnescape(message.getContent());
            Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);

            messageVO.put("user", userService.findUserById((Integer) data.get("userId")));
            messageVO.put("entityType", data.get("entityType"));
            messageVO.put("entityId", data.get("entityId"));

            int count = messageService.findNoticeCount(user.getId(), TOPIC_FOLLOW);
            messageVO.put("count", count);

            int unread = messageService.findNoticeUnreadCount(user.getId(), TOPIC_FOLLOW);
            messageVO.put("unread", unread);
        }
        model.addAttribute("followNotice", messageVO);

        // 查询未读消息数量
        int letterUnreadCount = messageService.findLetterUnreadCount(user.getId(), null);
        model.addAttribute("letterUnreadCount", letterUnreadCount);
        int noticeUnreadCount = messageService.findNoticeUnreadCount(user.getId(), null);
        model.addAttribute("noticeUnreadCount", noticeUnreadCount);

        return "/site/notice";
    }
~~~

#### 6. 开发通知详情

Dao层：

~~~java
//查询某个主题包含的通知列表
    List<Message> selectNotices(int userId,String topic,int offSet,int limit);
~~~

~~~sql
<select id="selectNotices" resultType="Message">
        select <include refid="selectFields"></include>
        from message
        where status != 2
        and from_id = 1
        and to_id = #{userId}
        and conversation_id = #{topic}
        order by create_time desc
        limit #{offset}, #{limit}
    </select>
~~~

Service层

~~~java
public List<Message> findNotices(int userId,String topic,int offset,int limit){
        return messageMapper.selectNotices(userId,topic,offset,limit);
    }
~~~

Controller层

~~~java
/**
     * 通知详情
     * @param topic
     * @param page
     * @param model
     * @return
     */
    @RequestMapping(path = "/notice/detail/{topic}", method = RequestMethod.GET)
    public String getNoticeDetail(@PathVariable("topic") String topic, Page page, Model model) {
        User user = hostHolder.getUser();

        page.setLimit(5);
        page.setPath("/notice/detail/" + topic);
        page.setRows(messageService.findNoticeCount(user.getId(), topic));

        List<Message> noticeList = messageService.findNotices(user.getId(), topic, page.getOffset(), page.getLimit());
        List<Map<String, Object>> noticeVoList = new ArrayList<>();
        if (noticeList != null) {
            for (Message notice : noticeList) {
                Map<String, Object> map = new HashMap<>();
                // 通知
                map.put("notice", notice);
                // 内容
                String content = HtmlUtils.htmlUnescape(notice.getContent());
                Map<String, Object> data = JSONObject.parseObject(content, HashMap.class);
                map.put("user", userService.findUserById((Integer) data.get("userId")));
                map.put("entityType", data.get("entityType"));
                map.put("entityId", data.get("entityId"));
                map.put("postId", data.get("postId"));
                // 通知作者
                map.put("fromUser", userService.findUserById(notice.getFromId()));

                noticeVoList.add(map);
            }
        }
        model.addAttribute("notices", noticeVoList);

        // 设置已读
        List<Integer> ids = getLetterIds(noticeList);
        if (!ids.isEmpty()) {
            messageService.readMessage(ids);
        }

        return "/site/notice-detail";
    }
~~~

定义拦截器显示刷新通知页面时，页面上端要显示所有的通知数
查找所有未读消息和未读通知

```java
@Component
public class MessageInterceptor implements HandlerInterceptor {
@Autowired
private HostHolder hostHolder;

@Autowired
private MessageService messageService;

@Override
public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
    User user = hostHolder.getUser();
    if(user != null && modelAndView != null){
        int letterUnreadCount = messageService.findLetterUnreadCount(user.getId(),null);
        int noticeUnreadCount = messageService.findNoticeUnreadCount(user.getId(),null);
        modelAndView.addObject("allUnreadCount",letterUnreadCount+noticeUnreadCount);
    }
}
    }

```

## 第七章：Elasticsearch，分布式搜索引擎

#### 1. Elasticsearch入门

https://blog.csdn.net/niaonao/article/details/85995870

>Elasticsearch简介：
>
>——一个分布式的、Restful风格的搜索引擎。
>
>——支持对各种类型的数据的检索。
>
>——搜索速度快，可以提供实时的搜索服务。
>
>——便于水平扩展，每秒可以处理PB级海量数据。
>
>Elasticsearch术语 ：
>
>——索引、类型、文档、字段。
>
>——集群、节点、分片、副本。

1. 查看es健康状况

![image-20200615162231774](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200615162231774.png)

2. 查看节点

![image-20200615162626181](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200615162626181.png)

3. 查看索引

![image-20200615162708760](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200615162708760.png)

4. 创建删除索引

![image-20200615163109581](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200615163109581.png)

![image-20200615163925790](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200615163925790.png)

5. postman

![image-20200615174745771](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200615174745771.png)

#### 2. Spring整合Elasticsearch

>引入依赖 :
>
>——spring-boot-starter-data-elasticsearch
>
>配置Elasticsearch：
>
>—— cluster-name、cluster-nodes
>
>Spring Data Elasticsearch
>
>——ElasticsearchTemplate
>
>——ElasticsearchRepository

~~~java
package com.nowcoder.community;

import com.nowcoder.community.dao.DiscussPostMapper;
import com.nowcoder.community.dao.elasticsearch.DiscussPostRepository;
import com.nowcoder.community.entity.DiscussPost;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.elasticsearch.search.sort.SortBuilders;
import org.elasticsearch.search.sort.SortOrder;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.elasticsearch.core.ElasticsearchTemplate;
import org.springframework.data.elasticsearch.core.SearchResultMapper;
import org.springframework.data.elasticsearch.core.aggregation.AggregatedPage;
import org.springframework.data.elasticsearch.core.aggregation.impl.AggregatedPageImpl;
import org.springframework.data.elasticsearch.core.query.NativeSearchQueryBuilder;
import org.springframework.data.elasticsearch.core.query.SearchQuery;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

@RunWith(SpringRunner.class)
@SpringBootTest
@ContextConfiguration(classes = CommunityApplication.class)
public class ElasticsearchTests {

    @Autowired
    private DiscussPostMapper discussMapper;

    @Autowired
    private DiscussPostRepository discussRepository;

    @Autowired
    private ElasticsearchTemplate elasticTemplate;

    @Test
    public void testInsert() {
        discussRepository.save(discussMapper.selectDiscussPostById(241));
        discussRepository.save(discussMapper.selectDiscussPostById(242));
        discussRepository.save(discussMapper.selectDiscussPostById(243));
    }
//增加
    @Test
    public void testInsertList() {
        discussRepository.saveAll(discussMapper.selectDiscussPosts(101, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(102, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(103, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(111, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(112, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(131, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(132, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(133, 0, 100));
        discussRepository.saveAll(discussMapper.selectDiscussPosts(134, 0, 100));
    }
    }
//更新
    @Test
    public void testUpdate() {
        DiscussPost post = discussMapper.selectDiscussPostById(231);
        post.setContent("我是新人,使劲灌水.");
        discussRepository.save(post);
    }
//删除
    @Test
    public void testDelete() {
        // discussRepository.deleteById(231);
        discussRepository.deleteAll();
    }
//删除
    @Test
    public void testSearchByRepository() {
        SearchQuery searchQuery = new NativeSearchQueryBuilder()
                .withQuery(QueryBuilders.multiMatchQuery("互联网寒冬", "title", "content"))
                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
                .withPageable(PageRequest.of(0, 10))
                .withHighlightFields(
                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
                ).build();

        // elasticTemplate.queryForPage(searchQuery, class, SearchResultMapper)
        // 底层获取得到了高亮显示的值, 但是没有返回.
        Page<DiscussPost> page = discussRepository.search(searchQuery);
        System.out.println(page.getTotalElements());
        System.out.println(page.getTotalPages());
        System.out.println(page.getNumber());
        System.out.println(page.getSize());
        for (DiscussPost post : page) {
            System.out.println(post);
        }
    }
//最重要的搜索功能
//可以进行高亮：在此前后加标签，我们负责加样式
    @Test
    public void testSearchByTemplate() {
        //SearchQuery提供的组件，实现类NativeSearchQueryBuilder
        //withQuery:构建搜索条件
        SearchQuery searchQuery = new NativeSearchQueryBuilder()
            //多字段搜索
                .withQuery(QueryBuilders.multiMatchQuery("互联网寒冬", "title", "content"))
            //构造排序条件，按照type排序
                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))
            //构造排序条件，按照score排序
                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))
            //构造排序条件，按照createTime排序
                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
            //分页
                .withPageable(PageRequest.of(0, 10))
            //高亮显示：指定高亮显示的字段和方式
                .withHighlightFields(
                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
                ).build();
        //
        Page<DiscussPost> page = elasticTemplate.queryForPage(searchQuery, DiscussPost.class, new SearchResultMapper() {
            @Override
            public <T> AggregatedPage<T> mapResults(SearchResponse response, Class<T> aClass, Pageable pageable) {
                SearchHits hits = response.getHits();
                if (hits.getTotalHits() <= 0) {
                    return null;
                }

                List<DiscussPost> list = new ArrayList<>();
                for (SearchHit hit : hits) {
                    DiscussPost post = new DiscussPost();

                    String id = hit.getSourceAsMap().get("id").toString();
                    post.setId(Integer.valueOf(id));

                    String userId = hit.getSourceAsMap().get("userId").toString();
                    post.setUserId(Integer.valueOf(userId));

                    String title = hit.getSourceAsMap().get("title").toString();
                    post.setTitle(title);

                    String content = hit.getSourceAsMap().get("content").toString();
                    post.setContent(content);

                    String status = hit.getSourceAsMap().get("status").toString();
                    post.setStatus(Integer.valueOf(status));

                    String createTime = hit.getSourceAsMap().get("createTime").toString();
                    post.setCreateTime(new Date(Long.valueOf(createTime)));

                    String commentCount = hit.getSourceAsMap().get("commentCount").toString();
                    post.setCommentCount(Integer.valueOf(commentCount));

                    // 处理高亮显示的结果
                    HighlightField titleField = hit.getHighlightFields().get("title");
                    if (titleField != null) {
                        post.setTitle(titleField.getFragments()[0].toString());
                    }

                    HighlightField contentField = hit.getHighlightFields().get("content");
                    if (contentField != null) {
                        post.setContent(contentField.getFragments()[0].toString());
                    }

                    list.add(post);
                }

                return new AggregatedPageImpl(list, pageable,
                        hits.getTotalHits(), response.getAggregations(), response.getScrollId(), hits.getMaxScore());
            }
        });

        System.out.println(page.getTotalElements());
        System.out.println(page.getTotalPages());
        System.out.println(page.getNumber());
        System.out.println(page.getSize());
        for (DiscussPost post : page) {
            System.out.println(post);
        }
    }

}
~~~

#### 3.开发社区搜索功能

>搜索服务：
>
>——将帖子保存至Elasticsearch服务器。
>
>——从Elasticsearch服务器删除帖子。
>
>——从Elasticsearch服务器搜索帖子。
>
>发布事件：
>
>——发布帖子时，将帖子异步的提交到Elasticsearch服务器。
>
>——增加评论时，将帖子异步的提交到Elasticsearch服务器。
>
>——在消费组件中增加一个方法，消费帖子发布事件。
>
>显示结果：
>
>——在控制器中处理搜索请求，在HTML上显示搜索结果。

~~~java
package com.nowcoder.community.service;

import com.nowcoder.community.dao.elasticsearch.DiscussPostRepository;
import com.nowcoder.community.entity.DiscussPost;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightBuilder;
import org.elasticsearch.search.fetch.subphase.highlight.HighlightField;
import org.elasticsearch.search.sort.SortBuilders;
import org.elasticsearch.search.sort.SortOrder;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.elasticsearch.core.ElasticsearchTemplate;
import org.springframework.data.elasticsearch.core.SearchResultMapper;
import org.springframework.data.elasticsearch.core.aggregation.AggregatedPage;
import org.springframework.data.elasticsearch.core.aggregation.impl.AggregatedPageImpl;
import org.springframework.data.elasticsearch.core.query.NativeSearchQueryBuilder;
import org.springframework.data.elasticsearch.core.query.SearchQuery;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.Date;
import java.util.List;

@Service
public class ElasticsearchService {

    @Autowired
    private DiscussPostRepository discussRepository;

    @Autowired
    private ElasticsearchTemplate elasticTemplate;

    public void saveDiscussPost(DiscussPost post) {
        discussRepository.save(post);
    }

    public void deleteDiscussPost(int id) {
        discussRepository.deleteById(id);
    }
//搜索帖子，支持分页
    public Page<DiscussPost> searchDiscussPost(String keyword, int current, int limit) {
        SearchQuery searchQuery = new NativeSearchQueryBuilder()
                .withQuery(QueryBuilders.multiMatchQuery(keyword, "title", "content"))
                .withSort(SortBuilders.fieldSort("type").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("score").order(SortOrder.DESC))
                .withSort(SortBuilders.fieldSort("createTime").order(SortOrder.DESC))
                .withPageable(PageRequest.of(current, limit))
                .withHighlightFields(
                        new HighlightBuilder.Field("title").preTags("<em>").postTags("</em>"),
                        new HighlightBuilder.Field("content").preTags("<em>").postTags("</em>")
                ).build();

        return elasticTemplate.queryForPage(searchQuery, DiscussPost.class, new SearchResultMapper() {
            @Override
            public <T> AggregatedPage<T> mapResults(SearchResponse response, Class<T> aClass, Pageable pageable) {
                SearchHits hits = response.getHits();
                if (hits.getTotalHits() <= 0) {
                    return null;
                }

                List<DiscussPost> list = new ArrayList<>();
                for (SearchHit hit : hits) {
                    DiscussPost post = new DiscussPost();

                    String id = hit.getSourceAsMap().get("id").toString();
                    post.setId(Integer.valueOf(id));

                    String userId = hit.getSourceAsMap().get("userId").toString();
                    post.setUserId(Integer.valueOf(userId));

                    String title = hit.getSourceAsMap().get("title").toString();
                    post.setTitle(title);

                    String content = hit.getSourceAsMap().get("content").toString();
                    post.setContent(content);

                    String status = hit.getSourceAsMap().get("status").toString();
                    post.setStatus(Integer.valueOf(status));

                    String createTime = hit.getSourceAsMap().get("createTime").toString();
                    post.setCreateTime(new Date(Long.valueOf(createTime)));

                    String commentCount = hit.getSourceAsMap().get("commentCount").toString();
                    post.setCommentCount(Integer.valueOf(commentCount));

                    // 处理高亮显示的结果
                    HighlightField titleField = hit.getHighlightFields().get("title");
                    if (titleField != null) {
                        post.setTitle(titleField.getFragments()[0].toString());
                    }

                    HighlightField contentField = hit.getHighlightFields().get("content");
                    if (contentField != null) {
                        post.setContent(contentField.getFragments()[0].toString());
                    }

                    list.add(post);
                }

                return new AggregatedPageImpl(list, pageable,
                        hits.getTotalHits(), response.getAggregations(), response.getScrollId(), hits.getMaxScore());
            }
        });
    }

}
~~~



## 第八章：构建安全高效的企业服务

#### 1.Spring Security

>简介：
>
>——Spring Security是专注于为java应用程序提供身份认证和授权的框架，它的强大之处在于它可以轻松扩展以满足自定义的需求。
>
>特征：
>
>——对身份的认证和授权提供全面的、可扩展的支持。
>
>——防止各种攻击，如会话固定攻击、点击劫持、csrf攻击等。
>
>——支持与Servlet API、Spring MVC等Web技术集成。

![image-20200604164520350](X:\Users\xu\AppData\Roaming\Typora\typora-user-images\image-20200604164520350.png)



#### 2. 权限控制

>登录检查:
>
>——之前采用拦截器实现登录检查，这仅仅是简单的权限管理方案，现将其废弃。
>
>授权配置：
>
>——对当前系统内包含的所有的请求，分配访问权限（普通用户、管理员、版主）
>
>认证方案：
>
>——绕过Security认证流程，采用系统原来的认证方案。
>
>CSRF配置：
>
>——防止CSRF攻击的基本原理，以及表单、AJAX相关的配置。

#### 3.置顶、加精、删除

>功能：
>
>——点击置顶，修改帖子类型
>
>——点击“加精”、“删除”，修改帖子状态
>
>权限管理：
>
>——版主：置顶、加精。
>
>——管理员：删除。
>
>按钮显示：
>
>——版主：置顶、加精。
>
>——管理员：删除。

#### 4. Redis高级数据类型

>HyperLogLog：
>
>——采用一种基数算法，用于完成独立总数统计。
>
>——占据空间小，无论统计多少个数据，只占12k内存空间。
>
>——不精确的统计算法，标准误差0.81%。
>
>Bitmap：
>
>——不是一种独立的数据结构，实际上就是字符串
>
>——支持按位存取数据，可以将其看成是byte数组。
>
>——适合存储索大量的连续的数据的布尔值。

#### 5. 网站数据统计

>UV（Unique Visitor）：
>
>——独立访客，需通过用户IP排统计数据。
>
>——每次访问都要进行统计。
>
>——HyperLogLog，性能好，存储空间小。
>
>DAU（Daily Active User）：
>
>——日活跃用户，需通过用户ID排重统计数据。
>
>——访问过一次，则认为活跃。
>
>——Bitmap，性能好，可统计精确的结果。

#### 6.任务执行和调度

>JDK线程池：
>
>——ExecutorService
>
>——ScheduledExecutorService
>
>Spring线程池：
>
>——ThreadPoolTaskExecutor
>
>——ThreadPoolTaskScheduler
>
>分布式定时任务：
>
>——Spring Quartz

#### 7. 热帖排行

>公式：
>
>——log ( 精华分 + 评论数 * 10 + 点赞数 * 2 ） + （ 发布时间 - 牛客纪元 ）

#### 8.生成长图

>wkhtmltopdf
>
>——wkhtmltopdf url file
>
>——wkhtmltoimage url file
>
>java
>
>——Runtime.getRuntime().exec()

#### 9.将文件上传至云服务器

>客户端上传：
>
>——客户端将数据提交给云服务器，并等待响应。
>
>——用户上传头像时，将表单数据提交给云服务器。
>
>服务器直传：
>
>——应用服务器将数据直接提交给云服务器，并等待响应。
>
>——分享时，服务端自动生成的图片，提交给云服务器。
>
>使用七牛云服务器。

#### 10. 优化网站性能

>本地缓存：
>
>——将数据缓存在应用服务器上，性能最好。
>
>——常用缓存工具：Ehcache、Guava、Caffeine等
>
>分布式缓存：
>
>——将数据缓存在NoSQL数据库上，跨服务器。
>
>——常用缓存工具：MemCache、Redis等。
>
>多级缓存：
>
>——一级缓存（本地缓存）> 二级缓存（分布式缓存）> DB
>
>——避免缓存雪崩（缓存失效，大量请求直达DB），提高系统的可用性。

## 第九章：项目发布与总结

![img](https://images2017.cnblogs.com/blog/1068446/201712/1068446-20171220235712881-1444752431.png)

<img src="X:\Users\xu\Desktop\20151126231945042.png" alt="20151126231945042" style="zoom:150%;" />



<img src="X:\Users\xu\Desktop\Center" alt="img" style="zoom: 200%;" />

1. ![20160712110935064](X:\Users\xu\Desktop\20160712110935064.png)



1. 启动kafka和zookeeper

~~~linux
//zookeeper
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
//kafka
nohup bin/kafka-server-start.sh config/server.properties  1>/dev/null 2>&1 &
//测试 kafka
bin/kafka-topics.sh  --list --bootstrap-server localhost:9092
~~~

2. Elasticsearch安装（6.4.3版）

https://www.cnblogs.com/pangyangqi/p/11277354.html

~~~linux
[root@VM_0_5_centos config]# groupadd nowcoder
[root@VM_0_5_centos config]# useradd nowcoderl -p 123456 -g nowcoder
~~~

~~~linux
[root@VM_0_5_centos /]# cd /opt
[root@VM_0_5_centos tmp]# chown -R nowcoder1:nowcoder *
~~~

~~~linux
[root@VM_0_5_centos /]# cd /tmp
[root@VM_0_5_centos tmp]# chown -R nowcoder1:nowcoder *
~~~

#解决Bug

~~~verilog
//
Scheduler class: 'org.quartz.core.QuartzScheduler' - running locally.
  NOT STARTED.
  Currently in standby mode.
  Number of jobs executed: 0
  Using thread pool 'org.quartz.simpl.SimpleThreadPool' - with 5 threads.
  Using job-store 'org.springframework.scheduling.quartz.LocalDataSourceJobStore' - which supports persistence. and is clustered.

2020-06-12 10:22:49,794 INFO [restartedMain] o.q.i.StdSchedulerFactory [StdSchedulerFactory.java:1362] Quartz scheduler 'communityScheduler' initialized from an externally provided properties instance.
2020-06-12 10:22:49,794 INFO [restartedMain] o.q.i.StdSchedulerFactory [StdSchedulerFactory.java:1366] Quartz scheduler version: 2.3.1
2020-06-12 10:22:49,794 INFO [restartedMain] o.q.c.QuartzScheduler [QuartzScheduler.java:2293] JobFactory set to: org.springframework.scheduling.quartz.SpringBeanJobFactory@7564f9a9
2020-06-12 10:22:49,846 INFO [restartedMain] o.q.c.QuartzScheduler [QuartzScheduler.java:666] Scheduler communityScheduler_$_DESKTOP-NEIJ1JA1591928569769 shutting down.
2020-06-12 10:22:49,846 INFO [restartedMain] o.q.c.QuartzScheduler [QuartzScheduler.java:585] Scheduler communityScheduler_$_DESKTOP-NEIJ1JA1591928569769 paused.
2020-06-12 10:22:50,273 INFO [restartedMain] o.q.c.QuartzScheduler [QuartzScheduler.java:740] Scheduler communityScheduler_$_DESKTOP-NEIJ1JA1591928569769 shutdown complete.
2020-06-12 10:22:50,274 WARN [restartedMain] o.s.b.w.s.c.AnnotationConfigServletWebServerApplicationContext [AbstractApplicationContext.java:557] Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'quartzScheduler' defined in class path resource [org/springframework/boot/autoconfigure/quartz/QuartzAutoConfiguration.class]: Invocation of init method failed; nested exception is org.quartz.JobPersistenceException: Couldn't retrieve job: Table 'community.qrtz_job_details' doesn't exist [See nested exception: java.sql.SQLSyntaxErrorException: Table 'community.qrtz_job_details' doesn't exist]
2020-06-12 10:22:50,275 INFO [restartedMain] o.s.s.c.ThreadPoolTaskExecutor [ExecutorConfigurationSupport.java:208] Shutting down ExecutorService 'applicationTaskExecutor'
2020-06-12 10:22:50,276 INFO [restartedMain] o.s.s.c.ThreadPoolTaskScheduler [ExecutorConfigurationSupport.java:208] Shutting down ExecutorService 'taskScheduler'
2020-06-12 10:22:51,290 INFO [restartedMain] c.z.h.HikariDataSource [HikariDataSource.java:350] HikariPool-1 - Shutdown initiated...
2020-06-12 10:22:51,299 INFO [restartedMain] c.z.h.HikariDataSource [HikariDataSource.java:352] HikariPool-1 - Shutdown completed.
2020-06-12 10:22:51,302 INFO [restartedMain] o.a.c.c.StandardService [DirectJDKLog.java:173] Stopping service [Tomcat]
2020-06-12 10:22:51,313 INFO [restartedMain] o.s.b.a.l.ConditionEvaluationReportLoggingListener [ConditionEvaluationReportLoggingListener.java:142] 

Error starting ApplicationContext. To display the conditions report re-run your application with 'debug' enabled.
2020-06-12 10:22:51,316 ERROR [restartedMain] o.s.b.SpringApplication [SpringApplication.java:858] Application run failed
org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'quartzScheduler' defined in class path resource [org/springframework/boot/autoconfigure/quartz/QuartzAutoConfiguration.class]: Invocation of init method failed; nested exception is org.quartz.JobPersistenceException: Couldn't retrieve job: Table 'community.qrtz_job_details' doesn't exist [See nested exception: java.sql.SQLSyntaxErrorException: Table 'community.qrtz_job_details' doesn't exist]
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1778)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.doCreateBean(AbstractAutowireCapableBeanFactory.java:593)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.createBean(AbstractAutowireCapableBeanFactory.java:515)
	at org.springframework.beans.factory.support.AbstractBeanFactory.lambda$doGetBean$0(AbstractBeanFactory.java:320)
	at org.springframework.beans.factory.support.DefaultSingletonBeanRegistry.getSingleton(DefaultSingletonBeanRegistry.java:222)
	at org.springframework.beans.factory.support.AbstractBeanFactory.doGetBean(AbstractBeanFactory.java:318)
	at org.springframework.beans.factory.support.AbstractBeanFactory.getBean(AbstractBeanFactory.java:199)
	at org.springframework.beans.factory.support.DefaultListableBeanFactory.preInstantiateSingletons(DefaultListableBeanFactory.java:824)
	at org.springframework.context.support.AbstractApplicationContext.finishBeanFactoryInitialization(AbstractApplicationContext.java:877)
	at org.springframework.context.support.AbstractApplicationContext.refresh(AbstractApplicationContext.java:549)
	at org.springframework.boot.web.servlet.context.ServletWebServerApplicationContext.refresh(ServletWebServerApplicationContext.java:142)
	at org.springframework.boot.SpringApplication.refresh(SpringApplication.java:775)
	at org.springframework.boot.SpringApplication.refreshContext(SpringApplication.java:397)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:316)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1260)
	at org.springframework.boot.SpringApplication.run(SpringApplication.java:1248)
	at com.nowcoder.community.CommunityApplication.main(CommunityApplication.java:19)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.springframework.boot.devtools.restart.RestartLauncher.run(RestartLauncher.java:49)
Caused by: org.quartz.JobPersistenceException: Couldn't retrieve job: Table 'community.qrtz_job_details' doesn't exist
	at org.quartz.impl.jdbcjobstore.JobStoreSupport.retrieveJob(JobStoreSupport.java:1401)
	at org.quartz.impl.jdbcjobstore.JobStoreSupport$9.execute(JobStoreSupport.java:1382)
	at org.quartz.impl.jdbcjobstore.JobStoreCMT.executeInLock(JobStoreCMT.java:245)
	at org.quartz.impl.jdbcjobstore.JobStoreSupport.executeWithoutLock(JobStoreSupport.java:3800)
	at org.quartz.impl.jdbcjobstore.JobStoreSupport.retrieveJob(JobStoreSupport.java:1379)
	at org.quartz.core.QuartzScheduler.getJobDetail(QuartzScheduler.java:1493)
	at org.quartz.impl.StdScheduler.getJobDetail(StdScheduler.java:498)
	at org.springframework.scheduling.quartz.SchedulerAccessor.addJobToScheduler(SchedulerAccessor.java:283)
	at org.springframework.scheduling.quartz.SchedulerAccessor.registerJobsAndTriggers(SchedulerAccessor.java:226)
	at org.springframework.scheduling.quartz.SchedulerFactoryBean.afterPropertiesSet(SchedulerFactoryBean.java:504)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.invokeInitMethods(AbstractAutowireCapableBeanFactory.java:1837)
	at org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory.initializeBean(AbstractAutowireCapableBeanFactory.java:1774)
	... 21 common frames omitted
Caused by: java.sql.SQLSyntaxErrorException: Table 'community.qrtz_job_details' doesn't exist
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:120)
	at com.mysql.cj.jdbc.exceptions.SQLError.createSQLException(SQLError.java:97)
	at com.mysql.cj.jdbc.exceptions.SQLExceptionsMapping.translateException(SQLExceptionsMapping.java:122)
	at com.mysql.cj.jdbc.ClientPreparedStatement.executeInternal(ClientPreparedStatement.java:955)
	at com.mysql.cj.jdbc.ClientPreparedStatement.executeQuery(ClientPreparedStatement.java:1005)
	at com.zaxxer.hikari.pool.ProxyPreparedStatement.executeQuery(ProxyPreparedStatement.java:52)
	at com.zaxxer.hikari.pool.HikariProxyPreparedStatement.executeQuery(HikariProxyPreparedStatement.java)
	at org.quartz.impl.jdbcjobstore.StdJDBCDelegate.selectJobDetail(StdJDBCDelegate.java:842)
	at org.quartz.impl.jdbcjobstore.JobStoreSupport.retrieveJob(JobStoreSupport.java:1390)
	... 32 common frames omitted

Process finished with exit code 0
~~~

