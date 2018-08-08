### Class.forName和ClassLoader区别
* Class.forName得到的class是已经初始化完成的，ClassLoader得到的class是还没有链接的，静态块和静态对象不会得到执行
***
###  描述动态代理的几种实现方式，分别说出相应的优缺点
* JDK动态代理：主要是对实现了接口的类生成代理，不能针对类
* cgli代理：针对类实现代理，主要是对指定的类生成一个子类，覆盖其中的方法(继承)
***