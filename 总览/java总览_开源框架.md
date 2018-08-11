### tomcat结构、以及其类加载流程
* 两个核心组件：Connecter和Container,一个Container可以选择多个Connecter，多个Connector和一个Container形成一个Service
* 类加载流程：JDK默认提供的启动类、扩展类和应用程序类加载器，CommonClassLoader，CatalinaClassLoader，SharedClassLoader和WebappClassLoader则是Tomcat自己定义的类加载器
***
### tomcat如何调优
* 增加JVM堆内存大小，修复JRE内存泄漏，数据库性能调优：给数据库连接池设置正确的值，最大空闲数，最大连接数,最大建立连接等待时间
* 线程池：可以在Tomcat组件之间共享，使用线程池的好处在于减少了创建销毁线程的相关消耗，而且可以提高线程的使用效率
***
### spring加载流程
* servlet容器启动，为应用创建一个“全局上下文环境”：ServletContext
* 容器调用contextLoaderListener，初始化上下文环境（即IOC容器），加载context-param指定的配置文件信息到IOC容器中
* 容器初始化web.xml中配置的servlet，为其初始化自己的上下文信息servletContext，并加载其设置的配置信息到该上下文中，将WebApplicationContext设置为它的父容器
* 此后的所有servlet的初始化都按照3步中方式创建，初始化自己的上下文环境，将WebApplicationContext设置为自己的父上下文环境
***
### spring事务的传播行为
* PROPAGATION_REQUIRED：如果存在一个事务，则支持当前事务，如果没有事务则开启一个新的事务
* PROPAGATION_SUPPORTS：如果存在一个事务，支持当前事务，如果没有事务，则非事务的执行
* PROPAGATION_MANDATORY：如果没有一个活动的事务，则抛出异常
* PROPAGATION_MANDATORY：它会开启一个新的事务，如果一个事务已经存在，则先将这个存在的事务挂起
* PROPAGATION_NOT_SUPPORTED：总是非事务地执行，并挂起任何存在的事务
* PROPAGATION_NEVER：总是非事务地执行，如果存在一个活动事务，则抛出异常
* PROPAGATION_NESTED：如果一个活动的事务存在，则运行在一个嵌套的事务中，如果没有活动事务, 则按TransactionDefinition.PROPAGATION_REQUIRED 属性执行
***
### spring如何管理事务
* 编程式事务管理：使用TransactionTemplate或者直接使用底层的PlatformTransactionManager
* 声明式事务管理：建立在AOP之上的，其本质是对方法前后进行拦截，然后在目标方法开始之前创建或者加入一个事务
***
### spring如何配置事务
* DataSource、TransactionManager和代理机制三部分
* 代理机制：
    * 每个Bean有一个代理
    * 所有Bean共享一个代理基类
    * 使用拦截器
    * 使用Tx标签配置的拦截器
    * 全注解配置
***
### Spring的理解，非单例注入的原理？生命周期？循环注入的原理，aop的实现原理，aop的几个术语
* spring依赖注入特性将组件关系透明化，降低耦合，实现了面向切面编程，提高了复用性
* spring的bean对象默认是单例的，spring提供了ApplicationContextAware，调用非单例类的时候通过容器去询问类是否是单例模式，需要重新初始化
* 生命周期：Definition->Pre-initialized->Ready->Destroyed
    * 实例化单例bean，注入依赖
    * 重写相应的方法能够获取bean的名字，重写相应的方法能够获取beanfactory实例
    * 重写相应的方法能够在bean定义前，在bean定义时，在bean定义后执行
    * 重写相应的方法在bean销毁时执行
* 循环注入：构造器注入，单例bean的setter注入，多例bean的setter注入
* aop术语：连接点，切点，增强，目标对象，引介，织入，代理，切面