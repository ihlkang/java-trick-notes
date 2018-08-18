### redis的list结构相关的操作
* Redis的listNode节点可以看作是一个双向的链表，节点内部存的值，是一个void *的指针
* 插入操作：lpush,rpush,linsert
* 删除操作：lpop，rpop
* 查看操作：lrange，llen，lindex
* 修改操作：lset
***
### redis特性
* 提供String,list,set,zset,hash等数据结构的存储，redis支持数据的备份，即master-slave模式的数据备份，redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用
***
### redis和memcached的内存管理的区别
* Redis为了方便内存的管理，在分配一块内存之后，会将这块内存的大小存入内存块的头部,当需要释放内存的时候，ret_ptr被传给内存管理程序,通过ret_ptr，程序可以很容易的算出real_ptr的值，然后将real_ptr传给free释放内存
* Memcached默认使用Slab Allocation机制管理内存，其主要思想是按照预先规定的大小，将分配的内存分割成特定长度的块以存储相应长度的key-value数据记录，以完全解决内存碎片问题
***
### redis事务的CAS操作
* watch指令在redis事务中提供了CAS的行为，watch 命令会监视给定的每一个key，当exec时如果监视的任一个key自从调用watch后发生过变化，则整个事务会回滚，不执行任何动作
***