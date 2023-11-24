# 锁与同步

> 讨论策略四 : 同步 在线程共享的应用

## 同步
原则　：并发的正确不应该取决于时间的差异

### 锁
1. **锁(locks)**:
    - **acquire** : 允许一个线程获得锁的所有权,锁将会使得其它线程无法与它竞争锁的拥有
    - **release** : 释放锁的拥有权

2. **阻塞(blocking)** : 
   - 除非有某一事件发生 , 否则线程始终陷入等待状态

### 死锁
当两个以上的并发模块都在互相等待对方结束运行时,就会发生[**死锁**](http://docs.oracle.com/javase/tutorial/essential/concurrency/deadlock.html). 
比如A等待B,B等待C , C等待A

就算不使用锁也存在死锁现象.比如客户端将服务端的消息缓冲填满后阻塞等待,而服务端又将返回给客户端的消息缓冲填满了也被阻塞了

## 开发一个线程安全的ADT
[Zip Example(./ex22-editor.zip)](./ex22-editor.zip)
[Code Example(./22code_example.md)](./22code_example.md)

该代码实现了一个支持多人同时链接编辑的多用户编辑器:
- EditBuffer.java : 实现了一个ADT(允许插入和编辑的字符串)
- SimpleBuffer.java : give a rep invariant
- [GapBuffer.java](https://en.wikipedia.org/wiki/Gap_buffer) : 实现一个间隔缓冲区,以一个gap代表一个光标,插入和删除只在gap附近进行

经过`spec` , `test` , `rep`的步骤后,我们成功在顺序的,单线程的环境下完成了测试
现在开始考虑`Synchronize` : 论证rep的线程安全,明确的写入注释
最后`iterate` : 迭代并完善ADT的操作集

## Locking in Java
Java中,每个对象都隐式关联了锁,你只需要创建一个裸对象.
但java的固有锁无法进行获取和释放,只能通过同步语句:
```java
synchronized (lock){ // lock is a object used to control lock
    // now threads has locked
    res ++;
} // exit and release
```
这样就将多线程问题转化为了原来的顺序编程


### 数据访问的保护者 : 锁
使用同一个锁来保护共享的数据变量 , 这样这些访问被保证是原子性的而不会被中断.

### 监控模式
在编写类时,最方便的锁是使用对象本身,即this(self).
通过将rep的所有访问封装在synchronized(this)中来保护rep
```java
public synchronized void function{}
// synchronization your constructors is unnecessary
```

## 线程安全分析

锁的规范 : 
- 每个共享的可变变量都应该有锁的保护 , 只允许在有锁的同步块中读写数据
- repint中的多个共享变量也应当用同一个锁保护

## 原子操作方法
如何让一个方法端与其它端同步
- 给客户端加锁 : 用其操作对象将方法实现区域锁起来,保证不受其它端的影响
```java
public function(obj){
    synchronized(obj){
        obj.insert;
        obj.delete;
        ...
    }
}
```

## 同步的代价
- 性能问题 : 需要更多的时间调用获取锁
- 滥用问题 : 思考同步声明(锁的位置,锁的作用,对安全性的作用)

## 死锁的再次出现
即使我们已经采用了监控式的设计,死锁仍然会出现.
例如当线程同时互相需要竞争锁,两个线程最终会各自拥有一个锁并且互相等待对方将锁释放.
```
- thread A has lock 1  |  thread B has lock 2
- thread A need lock 2 |  thread B need lock 1
- but B locked by 2      |  but A locked by 1
```

### 解决方法1 : 锁排序
需要同时获取锁时,对锁进行排序,保证代码按顺序获取锁而不是同时.
```java
// release b => release a => lock a => lock b
public void function(obj that){
    obj lock1 , lock2;
    if (this < that){lock1 = this , lock2 = that};
    else            {lock1 = that , lock2 = this};

    synchronized(lock1){
        synchronized(lock2){
            operate(this);
        }
    }
}
```

缺点 : 不是模块化的(你必须知道所有锁),而且你可能不能事先就知道所有锁

### 解决方法2 : 粗锁定
用一个锁来保护许多的对象实例,用该锁来同步其组成的所有操作
```java
public class obj{
    private final Lock lock;
    public void function(obj that){
        synchronized(lock){
            operate(this);
        }
    }
}
```
缺点 : 性能损失 , 且保护就意味着这些数据失去了并发访问的能力

# 并发实践

> 回顾 : 我们的目标是为了在构建并发软件时SFB , ETU , RFC
> - safety : 并发程序是否还符合其不变式和规范 
> - liveness : 程序是否会正确的继续运行 还是会卡住

> 策略: 
> 1. 库中的数据结构 : 干脆不使用锁 / 使用监控模式
> 2. 多组成的可变数据结构 : 粗锁定 / 线程限制
> 3. 搜索 : 改用不可变类型
> 4. 操作系统 : 精密锁 + 锁排序
> 5. 数据库系统 : 使用事务的概念来避免竞争,也可以回滚 \ 锁管理

