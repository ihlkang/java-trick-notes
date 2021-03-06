### java中基本数据类型是什么？占多少字节
- JAVA中8种基本数据类型：byte(8位)、short(16)、int(32)、long(64)、float(32)、double(64)、char(16)、boolean;
***

### String类能被继承？为什么
- String类有final修饰关键字修饰，所以不能被继承;
***

### Java中的String，StringBuilder，StringBuffer三者的区别
- 运行速度:StringBuilder>StringBuffer>String,前两为字符串变量可以被修改,但String是字符串常量,不能被修改;线程安全上,StringBuilder是线程不安全的,而StringBuffer是线程安全的;StringBuilder相较于StringBuffer有速度优势，所以多数情况下建议使用 StringBuilder类。然而在应用程序要求线程安全的情况下，则必须使用StringBuffer 类
***

### Java中ArrayList和LinkedList区别
- ArrayList是动态数组, 适用于查找操作，LinkedList是基于链表适用于新增和删除;
***

### 用过哪些 Map 类，都有什么区别，HashMap 是线程安全的吗,并发下使用的 Map 是什么，他们内部原理分别是什么，比如存储方式，hashcode，扩容,默认容量
- HashMap不是线程安全的，HashTable是线程安全的，但同步会带来性能开销，在不需要线程安全下，HashMap性能较好，Hashmap的迭代器初始化时会将modecount赋给迭代器的exceptedmodcount，在迭代过程中，判断modcount和exceptedmodcount是否相等，若不相等，则说明其他线程修改了map，抛出异常，这就是所谓的fail-fast策略
- LinkedHashMap，父类是HashMap,在HashMap基础上，在内部增加了一个链表，使用双向链表维护键值对次序，插入元素时性能低于HashMap，迭代访问时有很好性能
* TreeMap，根据元素的Key进行排序，Map接口派生sortMap子接口，为sortMap的实现类，Treemap的key以TreeSet形式存储
* HashMap内部Node数组默认大小16，自动扩容机制，重新计算容量，向HashMap对象里不断添加元素，当对象内部的数组无法装载更多元素时,使用新的数组代替已有容量小的数组
***

### JAVA8的ConcurrentHashMap为什么放弃了分段锁，有什么问题吗，如果你来设计，如何设计
* 并发编程中，使用频繁，提供了更好的写并发能力，大量使用了volatile，final，CAS等lock-free技术来减少锁竞争对于性能的影响
* 分段锁Segment继承ReentrantLock，只在同一个分段内存在竞态关系，不同分段锁之间没有锁竞争，多线程访问容器中不同数据段的数据时，线程间就不会存在锁竞争，相比于对整个Map加锁，大大提高了高并发环境下的处理能力
* 而在JDK1.8中摒弃了Segment分段锁机制，利用CAS+Synchronized来保证并发更新的安全，底层依然采用数组+链表+红黑树存储结构(降低时间复杂度)
***

### 抽象类和接口的区别
* 有抽象方法的类均叫做抽象类，抽象方法必须用abstract关键词修饰，抽象方法必须是public或protected(若为private，不能被子类继承),抽象类不能用来创建对象，继承抽象类的子类必须实现抽象方法，若没有实现，则必须将子类也定义为抽象类
* 接口中所有方法不能有具体实现，必须都是抽象方法使用public abstract修饰，变量要隐指定为public static final变量
* 抽象类是对一种事物的抽象，对类的抽象，接口是对行为的抽象，对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动
* 一般的应用里，最顶级的是接口，然后是抽象类实现接口，最后才到具体类实现
***

### 讲讲理解的NIO
* NIO同步非阻塞IO模型，同步是指线程不断轮询 IO 事件是否就绪，非阻塞是指线程在等待 IO 的时候，可以同时做其他任务
* 同步的核心就是 Selector，应用多路复用技术，Selector代替了线程本身轮询IO事件，避免了阻塞同时减少了不必要的线程消耗
* 非阻塞的核心就是通道和缓冲区，当 IO 事件就绪时，可以通过写到缓冲区，保证 IO 的成功，而无需线程阻塞式地等待
***

### 反射的原理及创建类的三种方式
* 是在运行状态中，对于任意一个类，都能都知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性，这种动态获取的信息以及动态调用对象的方法称之为反射
* model.getClass(),Model.class,Class.forName("Model")
***

### Class.forName和ClassLoader区别
* Class.forName得到的class是已经初始化完成的，ClassLoader得到的class是还没有链接的，静态块和静态对象不会得到执行
***
###  描述动态代理的几种实现方式，分别说出相应的优缺点
* JDK动态代理：只能代理实现了接口的类，代理对象本身并不真正实现服务，而是通过调用委托类的对象相关方法，提供特定服务，通过接口中的方法名，在动态生成的代理类中调用委托实现类的同名方法
* cgli代理：针对类实现代理，通过字节码技术为一个类创建子类，并在子类中采用方法拦截的技术拦截所有父类方法的调用，顺势织入横切逻辑，通过继承业务类，生成的动态代理类是业务类的子类，通过重写业务方法进行代理
***
### final的用途
* 可以用来修饰类、方法和变量,修饰类时表示类不能被继承，类中所有成员变量会隐式的指定为final方法
* 修饰方法，防止继承类修改方法，类的private方法会隐式指定为final方法
* 修饰变量，如果是引用变量，初始化之后便不能再让其指向另一个对象，如果是基本类型的变量，一旦初始化之后便不能更改
***
### 三种单例模式
* 懒汉式:线程是不安全的，若多个线程同时进入if(uniqueInstance==null)
```java
public class Singleton {
    private static Singleton uniqueInstance;
    private Singleton() {
    }
    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            uniqueInstance = new Singleton();
        }
        return uniqueInstance;
    }
}
```
* 懒汉式线程安全:一个线程进入方法后，其他试图进入的线程必须等待，性能有损耗
```java
public static synchronized Singleton getUniqueInstance() {
    if (uniqueInstance == null) {
        uniqueInstance = new Singleton();
    }
    return uniqueInstance;
}
```
* 双重校验锁：先判断是否已经实例化，如果没有才对实例化语句加锁，内部是if(==null)是为了防止两个线程同时进入外层的if语句块内
```java
public class Singleton {
    private volatile static Singleton uniqueInstance;
    private Singleton() {
    }
    public static Singleton getUniqueInstance() {
        if (uniqueInstance == null) {
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```
***
### 如何在父类中为子类自动完成所有的hashcode和equals实现？
* 重写hashcode方法和equals方法
*** 
### 访问修饰符作用范围
* private仅能在类内被访问，子类不能继承也不能访问
* default可以被同一个包内的其他类访问
* protected修饰的方法和属性，在同一个包内可以被访问和继承，不同包内，子类可以继承，非子类不能访问
* public修饰的方法和属性，可以被任意包内的类访问
***
### 深拷贝和浅拷贝的区别
* 是否真正获取一个对象的复制实体，而不是引用
* 浅拷贝是指向已存在的内存地址，B浅拷贝A，修改A的值，B会跟着变
* 深拷贝是申请了一个新的内存，B不会跟着A变化，不会在释放内存时出现浅拷贝释放同一个内存的错误
***
### 数组和链表数据结构，各自的时间复杂度
* 数组是线性结构，连续存储，一段连续的内存空间
* 链表是一种元素内存空间离散排列的线性数据结构，彼此通过指针相连
***
### 异常
* error和exception的区别：
    * 两者均继承自Throwale类
    * exception:可以是可控的(checked)或不可控的(unchecked),表示由程序员导致的错误，应该在应用程序级被处理
    * error:总是不可控的，经常用来表示系统错误或底层资源的错误，可以的话，应该在系统级被捕获
* 运行时异常(RuntimeExepction)和检查式异常(checkedExecption)区别：
    * 运行时异常：我们可以不处理，当出现这样的异常时，总是由虚拟机接管，如常见的空指针异常
    * 检查式异常：我们经常遇到的IO异常及sql异常，对于这种异常，java编译器要求我们必须对出现的这些异常进行catch
* 5个运行时异常： 
    * NullPointerException 空指针引用异常
    * ClassCastException 类型强制转换异常
    * IllegalArgumentException 传递非法参数异常
    * IndexOutOfBoundsException 下标越界异常
    * NumberFormatException 数字格式异常
***
### 自定义Java.lang.String会被加载？
  * 不会，双亲委派模型机制不允许，考虑到安全因素，如果不使用这种委托模式，我们随时使用自定义的String来动态替换java核心api中定义类型，会存在非常大的安全隐患，并且当父类已经加载了该类时，子类没必要再次加载，节省加载损耗
  ***
### 什么时候需要重新实现Object中的hashCode和equals方法，为什么重写equals需要重写hashcode
  * Object中的equals方法是比较两个对象地址是否相等，若比较对象的内容是否相等，需要重写equals方法
  * 当类的实例对象需要被采用哈希算法进行存储和检索时，需要按要求重写hashCode方法
  * hashset中先判断两个对象的hashCode是否相等，再判断equals是否相等
  * 本来equals的两个对象，若没有重写hashcode,当进行put时，实质上是通过Object的hashCode计算下标，不同的对象可能会得到不同的下标
***
### jdk1.5之后引入了泛型，泛型的存在是为了解决什么问题？
  * 泛型是为了解决开发中，遇到功能模块相似，只是数据类型不同的情况，合并冗余的代码
***
### 什么是序列化，怎么序列化，为什么序列化，会遇到什么问题？
* 序列化：将对象转换为字节序列的过程
* 反序列化：将字节序列恢复为对象的过程
* 为什么序列化：将内存中的对象状态保存到一个文件中或者数据库中，用套接字在网络上传送对象，通过RMI传输对象时
* 实现Serializable接口即可实现序列化，transient修饰的属性不会被序列化
***
### RMI和RPC区别
* RMI远程方法调用：
    * 客户调用客户端辅助对象stub上的方法
    * 客户端辅助对象打包调用信息(变量方法名)通过网络发给服务端辅助对象skeleton
    * 将发送来的信息解包，找出真正被调用的方法以及该方法所在对象
    * 调用真正服务对象上的真正方法，并将结果返回给服务端辅助对象
    * 服务端辅助对象将结果打包，发送给客户端辅助对象stub
    * 客户端辅助对象将返回值解包，返回给调用者
* RPC远程过程调用
    * 执行客户端调用语句，传送参数，调用本地系统，发送网络消息
    * 消息传送到远程主机，服务器得到消息并取得参数
    * 根据调用请求以及参数执行远程过程服务，执行完毕将结果返回服务器句柄
    * 服务器句柄返回结果，调用远程主机的系统网络服务发送结果
    * 消息返回本地主机，客户端句柄由本地主机的网络服务接收消息
***
### IP地址的正则表达式
* ((25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))\.){3}(25[0-5]|2[0-4]\d|((1\d{2})|([1-9]?\d)))  ?代表一个或零个[1-9]