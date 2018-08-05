### Java IO通信
- BIO通信
    - 一个线程负责连接
    - 一请求一应答
    - 缺乏弹性伸缩能力
- 伪异步IO通信
    - 线程池负责连接
    - M请求N应答
    - 线程池阻塞
- NIO通信
    - 缓冲区Buffer
    - 通道Channel
    - 多路复用器Selector epoll
- AIO通信
    - 连接注册读写事件和回调函数
    - 读写方法异步
    - 主动通知程序
- 四种IO对比
    - 客户端个数
    - IO类型：BIO 伪异步IO是阻塞同步IO,NIO是非阻塞同步IO,AIO是非阻塞异步IO
    - API使用难度：
    - 调试难度：后两种较复杂
    - 可靠性：后两种较高
    - 吞吐量：后两种较高

### 相比原生NIO优势
- API使用简单，定制能力强，可以通过ChannelHandler对框架进行灵活的扩展
- 入门门槛低，功能强大，预制了多种编解码功能，支持多种主流协议
- 性能高，通过与其他的业界主流的NIO框架对比，Netty的综合性能最优
- Netty比较成熟稳定，Netty修复了JDK NIO所有发现的bug

### WebSocket
- 什么是WebSocket
    - H5协议规范
    - 握手机制
    - 解决客户端与服务器实时通信而产生的技术
- WebSocket优点
    - 节省通信开销
    - 服务器主动传送数据给客户端
    - 实时通信
- WebSocket建立连接
    - 客户端发起握手请求
    - 服务端响应请求
    - 连接建立
- WebSocket生命周期
    - 打开事件
        - session对象、配置对象、一组路径参数
    - 消息事件
        - 文本消息、二进制、Pong消息
    - 错误事件
        - 处理入栈消息异常：sessionException
    - 关闭事件
        - 做一些通用清理工作
- WebSocket关闭连接
    - 服务器关闭底层TCP连接
    - 客户端发起TCP Close
### Netty实现WebSocket案例
- Netty开发服务端
- HTML实现客户端
- 实现服务端与客户端的实时交互