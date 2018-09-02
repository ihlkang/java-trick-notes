### hashMap和ConcurrentHashMap的区别
* HashMap不是线程安全的，多线程操作时需要格外小心
* ConcurrentHashMap使用的分段锁Segment继承ReentrantLock，只在同一个分段内存在竞态关系，不同分段锁之间没有锁竞争，多线程访问容器中不同数据段的数据时，线程间就不会存在锁竞争，相比于对整个Map加锁，大大提高了高并发环境下的处理能力
***
### hashMap内部具体如何实现的
* HashMap默认静态内部类Entry<K,V>实现了Map.Entry<K,V>接口。内部类中定义了key来存储键，value来存储值，hash存储key的hash值，next来存储地址引用
* 数据存储时使用在table中存放Entry<K,V>的形式来维护数据，Entry<K,V>之间使用next引用来建立连接
***
### 
