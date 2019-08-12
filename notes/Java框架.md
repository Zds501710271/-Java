<!-- TOC -->

- [SSH （Spring+Structs+Hibernate)](#ssh-springstructshibernate)
    - [Spring](#spring)
            - [什么是spring?](#什么是spring)
            - [什么是控制反转(IoC)?](#什么是控制反转ioc)
            - [什么是面向切面(AoP)?](#什么是面向切面aop)
            - [Spring AOP中的动态代理](#spring-aop中的动态代理)
            - [静态代理与动态代理区别](#静态代理与动态代理区别)
            - [Spring 框架的主要优点?](#spring-框架的主要优点)
            - [ApplicationContext 和 beanfactory 的区别?](#applicationcontext-和-beanfactory-的区别)
            - [Spring Bean 生命周期?](#spring-bean-生命周期)
            - [spring 中 bean 的作用域?](#spring-中-bean-的作用域)
            - [什么是 Spring MVC?](#什么是-spring-mvc)
            - [springMVC 和 struts2 的区别](#springmvc-和-struts2-的区别)
            - [Spring 中设计模式?](#spring-中设计模式)
    - [Structs](#structs)
            - [Structs是什么？](#structs是什么)
            - [Struts2详细运行流程](#struts2详细运行流程)
            - [Struts2请求流程](#struts2请求流程)
            - [springmvc和strus2的区别？](#springmvc和strus2的区别)
    - [Hibernate](#hibernate)
            - [Hibernate是什么?](#hibernate是什么)
            - [ORM / JPA / Hibernate 概念与关系](#orm--jpa--hibernate-概念与关系)
            - [Hibernate 工作原理](#hibernate-工作原理)
            - [为什么用Hibernate?](#为什么用hibernate)
            - [什么是缓存？](#什么是缓存)
            - [Hibernate一级，二级缓存？](#hibernate一级二级缓存)
            - [什么是缓存？](#什么是缓存-1)
- [SSM (Spring+SpringMVC+MyBatis)](#ssm-springspringmvcmybatis)
    - [Spring](#spring-1)
    - [SpringMVC](#springmvc)
    - [MyBatis](#mybatis)
            - [什么是MyBatis？](#什么是mybatis)
            - [Mybaits的优点：](#mybaits的优点)
            - [MyBatis与Hibernate有哪些不同？](#mybatis与hibernate有哪些不同)
- [其他](#其他)
    - [Servlet](#servlet)
            - [servlet 是什么?](#servlet-是什么)
            - [Tomcat：](#tomcat)
            - [Servlet 生命周期](#servlet-生命周期)
            - [Servlet工作原理](#servlet工作原理)
    - [JSP](#jsp)
            - [JSP 是什么?](#jsp-是什么)
            - [JSP 运行原理?](#jsp-运行原理)
            - [JSP的生命周期](#jsp的生命周期)
            - [JSP和Servlet的区别](#jsp和servlet的区别)

<!-- /TOC -->
# SSH （Spring+Structs+Hibernate)

## Spring

#### 什么是spring?
- Spring是一个开源的轻量级Java SE（Java 标准版本）/Java EE（Java 企业版本）开发应用框架，其目的是用于简化企业级应用程序开发
- Spring的核心是控制反转（IoC）和面向切面（AOP）
- Spring是一个轻量级的，用来装javabean的，控制反转(IoC)和面向切面(AoP)的容器框架，它可以使得开发者更专注于应用程序的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以从Spring中受益。它就像万能胶，可以起到连接的作用，例如最常用到的ssm和ssh。

#### 什么是控制反转(IoC)?
- IoC(或依赖注入DI=Dependency Injection) :
    - IOC 利用 java 反射机制，
    - IOC就是控制反转，是指创建对象的控制权的转移，以前创建对象的主动权和时机是由自己把控的，而现在这种权力转移到Spring容器中，并由容器根据配置文件去创建实例和管理各个实例之间的依赖关系，对象与对象之间松散耦合，也利于功能的复用。
    - DI依赖注入，和控制反转是同一个概念的不同角度的描述，即应用程序在运行时依赖IoC容器来动态注入对象需要的外部资源。Spring的IOC有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。
    - 最直观的表达就是，IOC让对象的创建不用去new了，可以由spring自动生产，使用java的反射机制，根据配置文件在运行时动态的去创建对象以及管理对象，并调用对象的方法的。

#### 什么是面向切面(AoP)?
- AOP，一般称为面向切面，作为面向对象的一种补充，用于将那些与业务无关，但却对多个对象产生影响的公共行为和逻辑，抽取并封装为一个可重用的模块，这个模块被命名为“切面”（Aspect），减少系统中的重复代码，降低了模块间的耦合度，同时提高了系统的可维护性。可用于权限认证、日志、事务处理。
- AOP实现的关键在于 代理模式，AOP代理主要分为静态代理和动态代理。静态代理的代表为AspectJ；动态代理则以Spring AOP为代表。
    - （1）AspectJ是静态代理的增强，
        - 所谓静态代理，就是AOP框架会在编译阶段生成AOP代理类，因此也称为编译时增强，他会在编译阶段将AspectJ(切面)织入到Java字节码中，运行的时候就是增强之后的AOP对象。
    - （2）Spring AOP使用的动态代理，
        - 所谓的动态代理就是说AOP框架不会去修改字节码，而是每次运行时在内存中临时为方法生成一个AOP对象，这个AOP对象包含了目标对象的全部方法，并且在特定的切点做了增强处理，并回调原对象的方法。

#### Spring AOP中的动态代理
- 主要有两种方式，JDK动态代理和CGLIB动态代理：
-  ①JDK动态代理只提供接口的代理，不支持类的代理。核心InvocationHandler接口和Proxy类，InvocationHandler 通过invoke()方法反射来调用目标类中的代码，动态地将横切逻辑和业务编织在一起；接着，Proxy利用 InvocationHandler动态创建一个符合某一接口的的实例,  生成目标类的代理对象。
-  ②如果代理类没有实现 InvocationHandler 接口，那么Spring AOP会选择使用CGLIB来动态代理目标类。CGLIB（Code Generation Library），是一个代码生成的类库，可以在运行时动态的生成指定类的一个子类对象，并覆盖其中特定方法并添加增强代码，从而实现AOP。CGLIB是通过继承的方式做的动态代理，因此如果某个类被标记为final，那么它是无法使用CGLIB做动态代理的。

#### 静态代理与动态代理区别
- 在于生成AOP代理对象的时机不同，相对来说AspectJ的静态代理方式具有更好的性能，但是AspectJ需要特定的编译器进行处理，而Spring AOP则无需特定的编译器处理。

#### Spring 框架的主要优点?
- 1）方便解耦，简化开发
Spring 就是一个大工厂，可以将所有对象的创建和依赖关系的维护交给 Spring 管理。
- 2）方便集成各种优秀框架
Spring 不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如 Struts2、Hibernate、MyBatis 等）的直接支持。
- 3）降低 Java EE API 的使用难度
Spring 对 Java EE 开发中非常难用的一些 API（JDBC、JavaMail、远程调用等）都提供了封装，使这些 API 应用的难度大大降低。
- 4）方便程序的测试
Spring 支持 JUnit4，可以通过注解方便地测试 Spring 程序。
- 5）AOP 编程的支持
Spring 提供面向切面编程，可以方便地实现对程序进行权限拦截和运行监控等功能。
- 6）声明式事务的支持
只需要通过配置就可以完成对事务的管理，而无须手动编程。

- 1、使用Spring的IOC容器，将对象之间的依赖关系交给Spring，降低组件之间的耦合性，让我们更专注于应用逻辑；
- 2、可以提供众多服务，事务管理，WS等；
- 3、AOP的很好支持，方便面向切面编程；
- 4、对主流的框架提供了很好的集成支持，如Hibernate、Struts2、JPA等。
- 5、Spring DI机制降低了业务对象替换的复杂性；
- 6、Spring属于低侵入，代码污染极低；
- 7、Spring的高度可开放性，并不强制依赖于Spring，开发者可以自由选择Spring部分或全部。

好处：
- 轻量： Spring 是轻量的， 基本的版本大约 2MB。
- 控制反转： Spring 通过控制反转实现了松散耦合， 对象们给出它们的依赖， 而不是创建或查找依赖的对象们。
- 面向切面的编程(AOP)： Spring 支持面向切面的编程， 并且把应用业务逻辑和系统服务分开。
- 容器： Spring 包含并管理应用中对象的生命周期和配置。
- MVC 框架： Spring 的 WEB 框架是个精心设计的框架， 是 Web 框架的一个很好的替代品。
- 事务管理： Spring 提供一个持续的事务管理接口， 可以扩展到上至本地事务下至全局事务（JTA） 。
- 异常处理： Spring 提供方便的 API 把具体技术相关的异常（比如由 JDBC， Hibernate or JDO 抛出的） 转化为一致的 unchecked 异常。

#### ApplicationContext 和 beanfactory 的区别?
#### Spring Bean 生命周期?
- Spring 上下文中的 Bean 也类似， 如下
    - 1、 实例化一个 Bean－－也就是我们常说的 new；
    - 2、 按照 Spring 上下文对实例化的 Bean 进行配置－－也就是 IOC 注入；
    - 3、 如果这个 Bean 已经实现了 BeanNameAware 接口， 会调用它实现的 setBeanName(String)方法， 此处传递的就是 Spring 配置文件中 Bean 的 id 值
    - 4、 如果这个 Bean 已经实现了 BeanFactoryAware 接口， 会调用它实现的 setBeanFactory(setBeanFactory(BeanFactory)传递的是 Spring 工厂自身（可以用这个方式来获取其它 Bean， 只需在 Spring 配置文件中配置一个普通的 Bean 就可以） ；
    - 5、 如果这个 Bean 已经实现了 ApplicationContextAware 接口， 会调用 setApplicationContext(ApplicationContext)方法， 传入 Spring 上下文（同样这个方式也可以实现步骤 4 的内容， 但比 4 更好， 因为 ApplicationContext 是 BeanFactory 的子接口， 有更
    多的实现方法） ；
    - 6、 如果这个 Bean 关联了 BeanPostProcessor 接口， 将会调用 postProcessBeforeInitialization(Object obj, String s)方法， BeanPostProcessor 经常被用作是 Bean内容的更改， 并且由于这个是在 Bean 初始化结束时调用那个的方法， 也可以被应用于内存或缓存技术；
    - 7、 如果 Bean 在 Spring 配置文件中配置了 init-method 属性会自动调用其配置的初始化方法。
    - 8、 如果这个 Bean 关联了 BeanPostProcessor 接口， 将会调用 postProcessAfterInitialization(Object obj, String s)方法、 ；
    注： 以上工作完成以后就可以应用这个 Bean 了， 那这个 Bean 是一个 Singleton 的，所以一般情况下我们调用同一个 id 的 Bean 会是在内容地址相同的实例， 当然在 Spring配置文件中也可以配置非 Singleton， 这里我们不做赘述。
    - 9、 当 Bean 不再需要时， 会经过清理阶段， 如果 Bean 实现了 DisposableBean 这个接口， 会调用那个其实现的 destroy()方法；
    - 10、 最后， 如果这个 Bean 的 Spring 配置中配置了 destroy-method 属性， 会自动调用其配置的销毁方法。

#### spring 中 bean 的作用域?
#### 什么是 Spring MVC?

- Spring MVC
    - Spring MVC 是一个基于 MVC 架构的用来简化 web 应用程序开发的应用开发框架， 它是 Spring 的一个模块,无需中间整合层来整合 ， 它和 Struts2 一样都属于表现层的框架。 在 web 模型中， MVC 是
一种很流行的框架， 通过把 Model， View， Controller 分离， 把较为复杂的 web 应用分成逻辑清晰的几部分， 简化开发， 减少出错， 方便组内开发人员之间的配合

- Spring MVC 执行流程（工作原理）
- ![Struts2详细运行流程](https://images2018.cnblogs.com/blog/1370903/201808/1370903-20180827201021158-682489195.png)
    -  1.用户发送请求至前端控制器 DispatcherServlet
    - 2.DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器。
    - 3.处理器映射器根据请求 url 找到具体的处理器， 生成处理器对象及处理器拦截器(如果有则生成)一并返回给 DispatcherServlet。
    - 4.DispatcherServlet 通过 HandlerAdapter 处理器适配器调用处理器
    - 5.执行处理器(Controller， 也叫后端控制器)。
    - 6.Controller 执行完成返回 ModelAndView
    - 7.HandlerAdapter 将 controller 执行结果 ModelAndView 返回给 DispatcherServlet
    - 8.DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器
    - 9.ViewReslover 解析后返回具体 View
    - 10.DispatcherServlet 对 View 进行渲染视图（即将模型数据填充至视图中） 。
    - 11.DispatcherServlet 响应用

- SpringMVC 加载流程
    - 1.Servlet 加载（监听器之后即执行） Servlet 的 init()
    - 2.加载配置文件
    - 3.从 ServletContext 拿到 spring 初始化 springmvc 相关对象
    - 4.放入 ServletContext

#### springMVC 和 struts2 的区别
- （1） springmvc 的入口是一个 servlet 即前端控制器（DispatchServlet） ， 而 struts2 入口是一个filter 过虑器（StrutsPrepareAndExecuteFilter） 。
- （2） springmvc 是基于方法开发(一个 url 对应一个方法)， 请求参数传递到方法的形参， 可以设计为单例或多例(建议单例)， struts2 是基于类开发， 传递参数是通过类的属性， 只能设计为多例。
- （3） Struts 采用值栈存储请求和响应的数据， 通过 OGNL 存取数据， springmvc 通过参数解析器是将request 请求内容解析， 并给方法形参赋值， 将数据和视图封装成 ModelAndView 对象， 最后又将 Mo
delAndView 中的模型数据通过 reques 域传输到页面。 Jsp 视图解析器默认使用 jstl。

#### Spring 中设计模式?

- 第一种： 简单工厂
    - 静态工厂方法（StaticFactory Method） 模式， 但不属于 23 种 GOF 设计模式之一。
    - 简单工厂模式的实质是由一个工厂类根据传入的参数， 动态决定应该创建哪一个产品类。
    - spring 中的 BeanFactory 就是简单工厂模式的体现， 根据传入一个唯一的标识来获得 bean 对象， 但是否是在传入参数后创建还是传入参数前创建这个要根据具体情况来定。 

 - 单例模式（Singleton）
    - 保证一个类仅有一个实例， 并提供一个访问它的全局访问点。
    - spring 中的单例模式完成了后半句话， 即提供了全局的访问点 BeanFactory。 但没有从构造器级别去控制单例， 这是因为 spring 管理的是是任意的 java 对象。
    - 核心提示点： Spring 下默认的 bean 均为 singleton， 可以通过 singleton=“true|false” 或者 scope=“？ ”来指定

 适配器（Adapter）
 - 代理（Proxy）
    - 为其他对象提供一种代理以控制对这个对象的访问。 从结构上来看和 Decorator 模式类似， 但 Proxy 是控制， 更像是一种对功能的限制， 而 Decorator 是增加职责。
spring 的 Proxy 模式在 aop 中有体现， 比如 JdkDynamicAopProxy 和 Cglib2AopProxy。 

 - 观察者（Observer）
    - 定义对象间的一种一对多的依赖关系， 当一个对象的状态发生改变时， 所有依赖于它的对象都得到通知并被自动更新。
    - spring 中 Observer 模式常用的地方是 listener 的实现。 如 ApplicationListener。

 策略（Strategy
 模板方法（Template Method）


## Structs

#### Structs是什么？

- struts是一个基于Sun J2EE平台的MVC框架，主要是采用Servlet和JSP技术来实现的。Struts把Servlet、JSP、自定义标签和信息资源(message resources)整合到一个统一的框架中，开发人员利用其进行开发时不用再自己编码实现全套MVC模式，极大的节省了时间，所以说Struts是一个非常不错的应用框架。

#### Struts2详细运行流程
- ![Struts2详细运行流程](https://images2015.cnblogs.com/blog/799093/201607/799093-20160724232754919-1384501318.png)

#### Struts2请求流程
- 1、客户端初始化一个指向Servlet容器（例如Tomcat）的请求；
- 2、这个请求经过一系列的过滤器（Filter）（这些过滤器中有一个叫做ActionContextCleanUp的可选过滤器，这个过滤器对于Struts2和其他框架的集成很有帮助，例如：SiteMesh Plugin）；
- 3、接着StrutsPrepareAndExecuteFilter被调用，StrutsPrepareAndExecuteFilter询问ActionMapper来决定这个请求是否需要调用某个Action；
- 4、如果ActionMapper决定需要调用某个Action，FilterDispatcher把请求的处理交给ActionProxy；
- 5、ActionProxy通过Configuration Manager询问框架的配置文件，找到需要调用的Action类；
- 6、ActionProxy创建一个ActionInvocation的实例。
- 7、ActionInvocation实例使用命名模式来调用，在调用Action的过程前后，涉及到相关拦截器（Intercepter）的调用。
- 8、一旦Action执行完毕，ActionInvocation负责根据struts.xml中的配置找到对应的返回结果。返回结果通常是（但不总是，也可能是另外的一个Action链）一个需要被表示的JSP或者FreeMarker的模版。在表示的过程中可以使用Struts2 框架中继承的标签。在这个过程中需要涉及到ActionMapper。

#### springmvc和strus2的区别？

- 入口不同:
    - springmvc 入口是Servlet。struts2入口是filter。
    
- 生命周期不同:
    - spring mvc Controller是单例的。所以不能使用成员变量获取参数。所以效率高。
    - struts action是多例的。所以可以使用成员变量获取参数。所以效率低。
    
- 拦截级别
    - Struts2是类级别的拦截， 一个类对应一个request上下文，SpringMVC是方法级别的拦截，一个方法对应一个request上下文，而方法同时又跟一个url对应,所以说从架构本身上SpringMVC就容易实现restful url,而struts2的架构实现起来要费劲，因为Struts2中Action的一个方法可以对应一个url，而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了。
    
- 数据共享
    - 由上边原因，SpringMVC的方法之间基本上独立的，独享request response数据，请求数据通过参数获取，处理结果通过ModelMap交回给框架，方法之间不共享变量
    - 而Struts2搞的就比较乱，虽然方法之间也是独立的，但其所有Action变量是共享的，这不会影响程序运行，却给我们编码 读程序时带来麻烦，每次来了请求就创建一个Action，一个Action对象对应一个request上下文。
    
- 内存
    - 由于Struts2需要针对每个request进行封装，把request，session等servlet生命周期的变量封装成一个一个Map，供给每个Action使用，并保证线程安全，所以在原则上，是比较耗费内存的。
    
- 拦截器实现机制
    - Struts2有以自己的interceptor机制，SpringMVC用的是独立的AOP方式，这样导致Struts2的配置文件量还是比SpringMVC大。
    
- Ajax
    - SpringMVC集成了Ajax，使用非常方便，只需一个注解@ResponseBody就可以实现，然后直接返回响应文本即可，而Struts2拦截器集成了Ajax，在Action中处理时一般必须安装插件或者自己写代码集成进去，使用起来也相对不方便。
    
- 设计思想上
    - Struts2更加符合OOP的编程思想， SpringMVC就比较谨慎，在servlet上扩展。


## Hibernate

#### Hibernate是什么?
- Hibernate 是一个开放源代码的对象关系映射框架，它对 JDBC 进行了非常轻量级的对象封装，它将 pojo 与数据库表建立映射关系，是一个全自动的 ORM（Object - Relationship - Mapping）框架，Hibernate 可以自动生成 SQL 语句，自动执行，使得 Java 程序员可以随心所欲的使用对象编程思维来操纵数据库。

#### ORM / JPA / Hibernate 概念与关系
- ORM（Object - Relationship - Mapping）即对象关系映射，他是一种思想，他的实质就是将关系数据库中的业务数据用对象的形式表示出来，并通过面向对象的方式将这些对象组织出来，实现系统的业务逻辑。说到底就是 Java 实体对象跟数据库数据的映射关系。
- JPA（Java - Persistence - API）是 Java EE 关于 ORM 思想的标准接口，仅仅是一套规范和接口，不是实现。
- Hibernate 就是实现这一规范和接口的 ORM 组件。

#### Hibernate 工作原理
- ![Hibernate 工作原理](https://img-blog.csdn.net/20180526102631543?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2E5MDkzMDE3NDA=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
- 	
原理：

1.通过Configuration().configure();读取并解析hibernate.cfg.xml配置文件

2.由hibernate.cfg.xml中的<mapping resource="com/xx/User.hbm.xml"/>读取并解析映射信息

3.通过config.buildSessionFactory();//创建SessionFactory

4.sessionFactory.openSession();//打开Sesssion

5.session.beginTransaction();//创建事务Transation

6.persistent operate持久化操作

7.session.getTransaction().commit();//提交事务

8.关闭Session

9.关闭SesstionFactory

#### 为什么用Hibernate?

1. 对JDBC访问数据库的代码做了封装，大大简化了数据访问层繁琐的重复性代码。

2. Hibernate是一个基于JDBC的主流持久化框架，是一个优秀的ORM实现。他很大程度的简化DAO层的编码工作

3. hibernate使用Java反射机制，而不是字节码增强程序来实现透明性。

4. hibernate的性能非常好，因为它是个轻量级框架。映射的灵活性很出色。它支持各种关系数据库，从一对一到多对多的各种复杂关系。

#### 什么是缓存？

#### Hibernate一级，二级缓存？

#### 什么是缓存？

# SSM (Spring+SpringMVC+MyBatis)

## Spring

## SpringMVC

## MyBatis
#### 什么是MyBatis？
- （1）Mybatis是一个半ORM（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL语句本身，不需要花费精力去处理加载驱动、创建连接、创建statement等繁杂的过程。程序员直接编写原生态sql，可以严格控制sql执行性能，灵活度高。

- （2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

- （3）通过xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过java对象和 statement中sql的动态参数进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射为java对象并返回。（从执行sql到返回result的过程）。

#### Mybaits的优点：

（1）基于SQL语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL写在XML里，解除sql与程序代码的耦合，便于统一管理；提供XML标签，支持编写动态SQL语句，并可重用。

（2）与JDBC相比，减少了50%以上的代码量，消除了JDBC大量冗余的代码，不需要手动开关连接；

（3）很好的与各种数据库兼容（因为MyBatis使用JDBC来连接数据库，所以只要JDBC支持的数据库MyBatis都支持）。

（4）能够与Spring很好的集成；

（5）提供映射标签，支持对象与数据库的ORM字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

#### MyBatis与Hibernate有哪些不同？

（1）Mybatis和hibernate不同，它不完全是一个ORM框架，因为MyBatis需要程序员自己编写Sql语句。

（2）Mybatis直接编写原生态sql，可以严格控制sql执行性能，灵活度高，非常适合对关系数据模型要求不高的软件开发，因为这类软件需求变化频繁，一但需求变化要求迅速输出成果。但是灵活的前提是mybatis无法做到数据库无关性，如果需要实现支持多种数据库的软件，则需要自定义多套sql映射文件，工作量大。 

（3）Hibernate对象/关系映射能力强，数据库无关性好，对于关系模型要求高的软件，如果用hibernate开发可以节省很多代码，提高效率。 


# 其他

## Servlet
####servlet 是什么?
- Servlet是一种服务器端的Java应用程序，具有独立于平台和协议的特性,可以生成动态的Web页面。 
- 它担当客户请求（Web浏览器或其他HTTP客户程序）与服务器响应（HTTP服务器上的数据库或应用程序）的中间层。 
- 一句话概括：servlet就是浏览器与服务器之间的桥梁。

####Tomcat：
- 由Apache组织提供的一种Web服务器，提供对jsp和Servlet的支持。它是一种轻量级的javaWeb容器（服务器），也是当前应用最广的JavaWeb服务器（免费）。

#### Servlet 生命周期
- Servlet 生命周期：Servlet 加载--->实例化--->服务--->销毁。
    - init（）：在Servlet的生命周期中，仅执行一次init()方法。它是在服务器装入Servlet时执行的，负责初始化Servlet对象。可以配置服务器，以在启动服务器或客户机首次访问Servlet时装入Servlet。无论有多少客户机访问Servlet，都不会重复执行init（）。
    - service（）：它是Servlet的核心，负责响应客户的请求。每当一个客户请求一个HttpServlet对象，该对象的Service()方法就要调用，而且传递给这个方法一个“请求”（ServletRequest）对象和一个“响应”（ServletResponse）对象作为参数。在HttpServlet中已存在Service()方法。默认的服务功能是调用与HTTP请求的方法相应的do功能。
    - destroy（）： 仅执行一次，在服务器端停止且卸载Servlet时执行该方法。当Servlet对象退出生命周期时，负责释放占用的资源。一个Servlet在运行service()方法时可能会产生其他的线程，因此需要确认在调用destroy()方法时，这些线程已经终止或完成。

#### Servlet工作原理
- Tomcat 与 Servlet 是如何工作的：
- ![Servlet工作](https://images0.cnblogs.com/blog/384192/201302/24114945-4774512d1247438fa58c37399d3999ae.jpg)
- 步骤：
    - 1.Web Client 向Servlet容器（Tomcat）发出Http请求
    - 2.Servlet容器接收Web Client的请求
    - 3.Servlet容器创建一个HttpRequest对象，将Web Client请求的信息封装到这个对象中。
    - 4.Servlet容器创建一个HttpResponse对象
    - 5.Servlet容器调用HttpServlet对象的service方法，把HttpRequest对象与HttpResponse对象作为参数传给 HttpServlet 对象。
    - 6.HttpServlet调用HttpRequest对象的有关方法，获取Http请求信息。
    - 7.HttpServlet调用HttpResponse对象的有关方法，生成响应数据。
    - 8.Servlet容器把HttpServlet的响应结果传给Web Client。

- Servlet工作原理：
    - 1、首先简单解释一下Servlet接收和响应客户请求的过程，首先客户发送一个请求，Servlet是调用service()方法对请求进行响应的，通过源代码可见，service()方法中对请求的方式进行了匹配，选择调用doGet,doPost等这些方法，然后再进入对应的方法中调用逻辑层的方法，实现对客户的响应。在Servlet接口和GenericServlet中是没有doGet（）、doPost（）等等这些方法的，HttpServlet中定义了这些方法，但是都是返回error信息，所以，我们每次定义一个Servlet的时候，都必须实现doGet或doPost等这些方法。

    - 2、每一个自定义的Servlet都必须实现Servlet的接口，Servlet接口中定义了五个方法，其中比较重要的三个方法涉及到Servlet的生命周期，分别是上文提到的init(),service(),destroy()方法。GenericServlet是一个通用的，不特定于任何协议的Servlet,它实现了Servlet接口。而HttpServlet继承于GenericServlet，因此HttpServlet也实现了Servlet接口。所以我们定义Servlet的时候只需要继承HttpServlet即可。

    - 3、Servlet接口和GenericServlet是不特定于任何协议的，而HttpServlet是特定于HTTP协议的类，所以HttpServlet中实现了service()方法，并将请求ServletRequest、ServletResponse 强转为HttpRequest 和 HttpResponse。

- 创建Servlet对象的时机：

    - Servlet容器启动时：读取web.xml配置文件中的信息，构造指定的Servlet对象，创建ServletConfig对象，同时将ServletConfig对象作为参数来调用Servlet对象的init方法。
    - 在Servlet容器启动后：客户首次向Servlet发出请求，Servlet容器会判断内存中是否存在指定的Servlet对象，如果没有则创建它，然后根据客户的请求创建HttpRequest、HttpResponse对象，从而调用Servlet 对象的service方法。
    - Servlet Servlet容器在启动时自动创建Servlet，这是由在web.xml文件中为Servlet设置的<load-on-startup>属性决定的。从中我们也能看到同一个类型的Servlet对象在Servlet容器中以单例的形式存在。
    
## JSP
#### JSP 是什么?
- JSP是Java Server Page的缩写，它是Servlet的扩展，它的作用是简化网站的创建和维护。
- JSP代码是在服务器上执行的，因此JSP网页内容可以动态变化，So:通常将JSP技术归类于动态网页技术；
- JSP是HTML代码与Java代码的混合体。
- JSP形式上像HTML，但本质上是Servlet。

#### JSP 运行原理?
- ![JSP执行过程1](https://img-blog.csdnimg.cn/20181209180703585.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0phY2tfX0Zyb3N0,size_16,color_FFFFFF,t_70)
- ![JSP执行过程1](https://img-blog.csdnimg.cn/20181209180714217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0phY2tfX0Zyb3N0,size_16,color_FFFFFF,t_70)

- 执行过程：
    - 1）首先，客户端发出请求(request )，请求访问JSP网页
    - 2）接着，JSP Container将要访问的.JSP文件 转译成Servlet的源代码（.java文件）
    - 3）然后，将产生的Servlet的源代码（.java文件）经过编译，生成.class文件，并加载到内存执行
    - 4）最后把结果响应(response )给客户端
    
    - 转译时期(TranslationTime)和请求时期：
        - 转译时期：JSP转译成Servlet类(.class文件)。
            -  (1)将JSP网页转译为Servlet源代码(.java)，此段称为转译时期(Translation time)；
            -  (2)将Servlet源代码(.java)编译成Servlet类(.class)，此阶段称为编译时期(Compilation time)。
        - 请求时期：Servlet类(.class文件)执行后，响应结果至客户端。
       
- 执行过程详细：
    - 1.WEB容器（Servlet引擎）接收到以.jsp为扩展名的URL的访问请求时，容器会把访问请求交给JSP引擎去处理
    - 2.每个JSP页面在第一次被访问时，JSP引擎将它翻译成一个Servlet源程序，接着再把这个Servlet源程序编译成Servlet的.class类文件，然后再由WEB容器（Servlet引擎）像调用普通Servlet程序一样的方式来装载和解释执行这个由JSP页面翻译成的Servlet程序，并执行该servlet实例的jspInit()方法(jspInit()方法在Servlet的生命周期中只被执行一次)。。
    - 3.然后创建并启动一个新的线程，新线程调用实例的jspService()方法。(对于每一个请求，JSP引擎会创建一个新的线程来处理该请求。如果有多个客户端同时请求该JSP文件，则JSP引擎会创建多个线程，每个客户端请求对应一个线程)。
    - 4.浏览器在调用JSP文件时，Servlet容器会把浏览器的请求和对浏览器的回应封装成HttpServletRequest和HttpServletResponse对象，同时调用对应的Servlet实例中的jspService()方法，把这两个对象作为参数传递到jspService()方法中。
    - 5.jspService()方法执行后会将HTML内容返回给客户端。
    如果JSP文件被修改了，服务器将根据设置决定是否对该文件进行重新编译。如果需要重新编译，则将编译结果取代内存中的Servlet，并继续上述处理过程。 如果在任何时候由于系统资源不足，JSP引擎将以某种不确定的方式将Servlet从内存中移去。当这种情况发生时，jspDestroy()方法首先被调用, 然后Servlet实例便被标记加入“垃圾收集”处理。


#### JSP的生命周期
- 包括六个阶段：转换，编译，加载并实例化，初始化（_jspInit），请求处理（_jspService()调用），销毁（_jspDestory()
    - 转译：就是web容器将JSP文件转换成一个包含了Servlet类定义的java源文件。
    - 编译：把在转换阶段创建的java源文件变异成类文件。
    - JSP生命周期其他的四个阶段(即：实例化、初始化、请求处理、销毁)跟Servlet生命周期相同。

#### JSP和Servlet的区别
- (1)JSP经编译后就变成了“类servlet”。
- (2)JSP由HTML代码和JSP标签构成，更擅长页面显示；Servlet更擅长流程控制。
- (3)JSP中嵌入JAVA代码，而Servlet中嵌入HTML代码。