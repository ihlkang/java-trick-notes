### java中基本数据类型是什么？占多少字节
* JAVA中8种基本数据类型：byte(8位)、short(16)、int(32)、long(64)、float(32)、double(64)、char(16)、boolean;

### String类能被继承？为什么
* String类有final修饰关键字修饰，所以不能被继承;

### Java中的String，StringBuilder，StringBuffer三者的区别
* 运行速度:StringBuilder>StringBuffer>String,前两为字符串变量可以被修改,但String是字符串常量,不能被修改;线程安全上,StringBuilder是线程不安全的,而StringBuffer是线程安全的;StringBuilder相较于StringBuffer有速度优势，所以多数情况下建议使用 StringBuilder类。然而在应用程序要求线程安全的情况下，则必须使用StringBuffer 类

### Java中ArrayList和LinkedList区别
* ArrayList是动态数组, 适用于查找操作，LinkedList是基于链表适用于新增和删除;

### 代码执行顺序：
* 父类静态代码块->子类静态代码块->main()方法->父类构造代码块->父类构造方法->子类构造代码块->子类构造方法
* 普通代码块和一般的语句执行顺由出现的次序决定
* 构造代码块的执行次序优先于类构造函数
* 每个静态代码块只会执行一次，静态代码块先于主方法执行
* 如果类中包含多个静态代码块，那么将按照出现次序执行

### 用过哪些 Map 类，都有什么区别，HashMap 是线程安全的吗,并发下使用的 Map 是什么，他们内部原理分别是什么，比如存储方式，hashcode，扩容,默认容量
* HashMap不是线程安全的，HashTable是线程安全的，但同步会带来性能开销，在不需要线程安全下，HashMap性能较好，Hashmap的迭代器初始化时会将modecount赋给迭代器的exceptedmodcount，在迭代过程中，判断modcount和exceptedmodcount是否相等，若不相等，则说明其他线程修改了map，抛出异常，这就是所谓的fail-fast策略
* LinkedHashMap，父类是HashMap,在HashMap基础上，在内部增加了一个链表，使用双向链表维护键值对次序，插入元素时性能低于HashMap，迭代访问时有很好性能
* TreeMap，根据元素的Key进行排序，Map接口派生sortMap子接口，为sortMap的实现类，Treemap的key以TreeSet形式存储
* HashMap内部Node数组默认大小16，自动扩容机制，重新计算容量，向HashMap对象里不断添加元素，当对象内部的数组无法装载更多元素时,使用新的数组代替已有容量小的数组

### JAVA8的ConcurrentHashMap为什么放弃了分段锁，有什么问题吗，如果你来设计，如何设计
* 并发编程中，使用频繁，提供了更好的写并发能力，大量使用了volatile，final，CAS等lock-free技术来减少锁竞争对于性能的影响
* 分段锁Segment继承ReentrantLock，只在同一个分段内存在竞态关系，不同分段锁之间没有锁竞争，多线程访问容器中不同数据段的数据时，线程间就不会存在锁竞争，相比于对整个Map加锁，大大提高了高并发环境下的处理能力
* 而在JDK1.8中摒弃了Segment分段锁机制，利用CAS+Synchronized来保证并发更新的安全，底层依然采用数组+链表+红黑树存储结构(降低时间复杂度)

### volatile
* i = i + 1此操作会触发缓存不一致问题，在并发编程中，通常会遇到原子性、可见性和有序性问题，想并发程序正确地执行，必须要保证原子性、可见性以及有序性
* 原子性：即一个操作或者多个操作要么全部执行并且执行的过程不会被任何因素打断，要么就都不执行
* 可见性：一个线程修改了某个变量的值，其他线程能够立即看得到修改的值
* 有序性：程序执行的顺序按照代码的先后顺序执行，指令重排序不会影响单个线程的执行，但是会影响到线程并发执行的正确性
* volatile修饰的成员变量、静态成员变量，一个线程修改了变量值，对其他线程是立即可见的，并且禁止指令重排,volatile某些情况下性能优于synchronized，但由于volatile无法保证原子性,不是线程安全的

### Object
* Object类常用方法：toString、equals、hashCode、finalize、wait、notify。
* toString将对象信息变为字符串返回，默认输出对象地址，可以在子类中重写object类的toString方法输出对象属性信息。
* equals方法用于比较对象是否相等，而且此方法必须被重写，因为object中的equals方法与== 均是比较对象的地址,像String类是对equals方法进行了重写。
```java
public boolean equals(Object obj) {
    return (this == obj);
    }
```
* clone，用途是用来另存一个当前存在的对象。
* hashCode方法，该方法用来返回其所在对象的物理地址（哈希码值），常会和equals方法同时重写，确保相等的两个对象拥有相等的hashCode。
* wait、notify，wait释放obj对象锁，进入线程等待队列，notify通知等待队列中的第一个线程去获得该对象锁。
* finalize 进行垃圾回收时候会用到

### Objects
* Objects是java.util包下的工具类，具有的方法：compare、equals/deepEquals、hash/hashCode、isNull/nonNull、toString、requireNonNull。

### java8流式处理：函数式接口和Lambda表达式
[Stream流式处理](https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/ )

### 常见的锁
* 共享锁：又称读锁，事务T对对象A加了S锁，则事务T可以读A但不能修改A,其他事务可以读A，但在事务T释放S锁前，不能对A做任何操作。
* 排他锁：又称写锁，事务T对对象A加了X锁，则事务T可以读A也可以修改A，其他事务在事务T释放S锁前，不能对A读和写。
* 互斥锁：一个执行单元要想访问被自旋锁保护的共享资源，必须先得到锁，在访问完共享资源后，必须释放锁。
* 自旋锁：如果在获取自旋锁时锁已经有保持者，那么获取锁操作将自旋在那里，直到该自旋锁的保持者释放了锁。
* 可重入锁：一个线程在获取了锁之后，再次去获取了同一个锁，这时候仅仅是把状态值进行累加
* 双重检查锁，在单例模式中
* 死锁：互斥、请求和保持、不剥夺和循环等待

### 抽象类和接口的区别
* 有抽象方法的类均叫做抽象类，抽象方法必须用abstract关键词修饰，抽象方法必须是public或protected(若为private，不能被子类继承),抽象类不能用来创建对象，继承抽象类的子类必须实现抽象方法，若没有实现，则必须将子类也定义为抽象类
* 接口中所有方法不能有具体实现，必须都是抽象方法使用public abstract修饰，变量要隐指定为public static final变量
* 抽象类是对一种事物的抽象，对类的抽象，接口是对行为的抽象，对于抽象类，如果需要添加新的方法，可以直接在抽象类中添加具体的实现，子类可以不进行变更；而对于接口则不行，如果接口进行了变更，则所有实现这个接口的类都必须进行相应的改动
* 一般的应用里，最顶级的是接口，然后是抽象类实现接口，最后才到具体类实现

### 讲讲理解的NIO
* NIO同步非阻塞IO模型，同步是指线程不断轮询 IO 事件是否就绪，非阻塞是指线程在等待 IO 的时候，可以同时做其他任务
* 同步的核心就是 Selector，Selector 代替了线程本身轮询 IO 事件，避免了阻塞同时减少了不必要的线程消耗
* 非阻塞的核心就是通道和缓冲区，当 IO 事件就绪时，可以通过写到缓冲区，保证 IO 的成功，而无需线程阻塞式地等待

### 反射的原理及创建类的三种方式
* 是在运行状态中，对于任意一个类，都能都知道这个类的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性，这种动态获取的信息以及动态调用对象的方法称之为反射
* model.getClass(),Model.class,Class.forName("Model")

### 反射实例 requestToModel(request,outClass)
* outClass(PreLoanAssets.class) 获取newInstance(outClass)对象 outModel
* 通过request对象获取所有字段getAllFieldsOfObject(request) allFields
    * 获取request对象的类对象,Class<?> requestClass = request.getClass()
    * 获取所有声明的字段,Field[] requestFields = requestClass .getDeclaredFields()
    * Map<String,FieldInfo> allFields = new HashMap<>(requestFields.length)
    * requestFields 中 非静态 非Neglected{字段f.getAnnotation(Neglected.class)}
    * 取原始字段名 映射字段名 字段值
        * f.getName() 获取字段名
        * getFieldName(requestClass, f)，getMappingFieldName(f, originalFieldName)，MapToField mapToField =field.getAnnotation(MapToField.class); 返回mapToField 获取映射字段名
        * getGetterMethodFromClass(requestClass, fieldName);
        * String getterName = String.format("get%c%s", Character.toUpperCase(fieldName.charAt(0))
        * ,fieldName.substring(1));Method method = requestClass.getMethod(getterName)
        * 获取request类的getXXX方法invokeMethod(method, request);执行该get方法，返回字段值fieldValue
        * 将获取的映射字段名作为键，FieldInfo(原始字段名、字段类型、字段值)作为值存入allFields中
* 依此获取FieldInfo字段信息 映射字段名fName、字段类型fClass、字段值fValue 获取set方法 执行set方法
    * FieldInfo fi = allFields.get(fName); Class<?> fClass = fi.fieldClass;  Object fValue = fi.fieldValue;
    * Method setter = ReflectUtil.getSetterMethodFromClass(outClass, fName, fClass);获取setter方法
        * String setterName = String.format("set%s%s",Character.toUpperCase(fieldName.charAt(0)),fieldName.subString(1));
        * Method method = outClass.getMethod(setterName,fClass);
        * ReflectUtil.invokeMethod(setter, outModel, fValue);执行setter方法；
* 返回outModel

### MyIsam和Innodb

### spring实例化两种方式：
* BeanUtils和cglib
* spring提供两种方式生成代理对象jdkproxy cglib
