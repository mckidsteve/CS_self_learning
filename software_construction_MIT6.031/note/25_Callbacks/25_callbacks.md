# 回调
> 即实现者调用客户端提供的函数
> 回调的思想就是将函数当作数据来处理,将他们作为参数传递,将他们作为结果返回
## 图形用户界面的输入处理
侦听器模式 (发布 - 订阅) :
- 事件源 => 发布 => 离散事件流(状态转换)
- 侦听器 => 订阅 => 提供新事件函数

## 回调
侦听就是回调设计模式的一个例子.
回调是指函数并不由设计模块给出,而是由客户端提供给模块执行

## Web中的路由处理
Web服务器的控制流是一个输入循环(等待浏览器传入连接):
- 当新的连接到达服务器路由,服务器读取并解析(HTTP)
- 将请求的路由和匹配的路由注册程序
- 处理程序构造对请求的响应

## 优先层级函数(First-class function)
回调需要使用**优先层次函数** : 可以作为参数传递 , 也可以作为返回值返回.
然而在java中唯一的优先层级值只有原始值(int , bool ...)和对象引用.所以需要使用一个带有代表函数的对象
曾经使用过这种思想(函数型对象)的对象设计:
- Runnable : 用于给线程构造函数的对象
- Sort(cmp)中的cmp
- actionPerform(actionEvent e) 监听器
- httphandle http处理

### lambda exp

## 事件处理系统中的并发
由于回调的出现,回调可能在任何时候被其它线程调用,所以应当考虑其并发问题.

### Code Design
[zip](./ex25-callbacks.zip)
[code](./25_code.md)

这里提前考虑线程问题,将方法标记为同步,由锁来保护线程安全

