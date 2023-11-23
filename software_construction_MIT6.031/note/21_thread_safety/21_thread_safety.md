# 线程安全
> 上一节认为竞争是unsafe的
> 我们有四种解决方法:
> - 限制 : 不允许线程进行变量或者数据的共享
> - 不变性 : 使得共享变量不可更改
> - 线程安全的数据类型 : 将共享数据封装在线程安全的数据类型
> - 同步 : 使用同步的方法来防止线程对共享数据的同时访问

## 线程安全
**无论线程如何执行**,一个线程安全的数据类型始终**行为正确**而**无需额外的协调**.
- 行为正确 : 满足其行为规范且rep表达式不变
- 无论线程如何执行 : 不管线程是在多处理器下还是单处理器下
- 无需额外协调 : 对调用者不应有时间上的特异性条件控制

## 策略一 : 限制
线程限制 : 将引用或者数据限制在单个线程中

局部变量是线程限制的,因为局部变量存在堆栈中,每个线程都有自己的堆栈,尽管其方法被多个线程调用,但其调用都有自己的私有副本,变量本身是被限制的

注意,变量是线程限制的,但如果是对象引用型的,还要检查所指向的对象是否可变,是否受限

### 避免全局变量
记忆化调用(memoization) : 存储之前调用的结果以再次调用
但是这种方法并不多线程安全,因为静态变量会缓存在hashmap中,被所有方法调用.

### 线程限制的使用范围
即使声明为私有,也无法证明一个ADT是线程安全的.我们无法知道客户端是否进行的复制的别名调用,无法知道其字段有没有被并发访问

### 限制和匿名函数
匿名线程使得线程限制变得复杂:
当我们在方法中创建新的线程时,java允许此线程从外部范围读取变量.
不过在外部定义的局部变量虽然不再安全,但由于不允许其在线程内外重新赋值读取,可以认为其有一定的线程安全(不再被限制,但具有不变性)

## 策略二 : 不变性
保证变量以及对象的不可变性,使得其安全的从多个线程访问.因为只能读取而不能写入,竞争问题不会出现.

不可变对象存在突变,正常情况没有什么问题,而且还是有益的.
但并发情况下,使用了受益突变的不可变数据类型必须使用**锁**

为了确信不可变类型在没有锁时是安全的,我们认为以下数据类型是**线程安全的不可变类型**:
- 没有突变
- 所有字段都是private 和 final的
- 没有rep代表表达式的暴露
- rep中的可变对象不进行任何的突变

## 策略三 : 使用线程安全的数据类型
例如StringBuffer是线程安全的而StringBuilder不是.
线程安全的类型通常会带来性能损失.

Collections API提供了使集合封装的方法,使其在可变的同时线程安全,使得其操作方法以原子操作完成.
Now we see a way to fix the `isPrime()` method we had [earlier in the reading](http://web.mit.edu/6.031/www/sp21/classes/21-thread-safety/#isPrime):
```java
private static Map<Integer,Boolean> cache =
                Collections.synchronizedMap(new HashMap<>());
```

- 不要绕过封装器
- 迭代器仍然不能线程安全
- 原子操作仍然有可能竞争的出现

## 安全论证
安全论证需要对模块或程序中存在的所有线程及其使用的数据进行编目，并论证你使用了四种技术中的哪一种来防止每个数据对象或变量出现竞赛：限制、不变性、线程安全数据类型或同步。当你使用后两种技术时，你还需要证明对数据的所有访问都是适当的原子性访问，也就是说，你所依赖的不变性不会受到交错的威胁。

我们将专注于线程安全的论点，这些论点诉诸于不变性和线程安全的数据类型。

我们还必须避免 Rep 暴露，并记录我们对 Rep 暴露安全性的论证。Rep 暴露对任何数据类型都不利，因为它会威胁到数据类型的 rep 不变性。这对线程安全同样是致命的。

For example, here is a good argument that appeals to immutability

```java

/** MyString is an immutable data type representing a string of characters. */

public class MyString {
    private final char[] a;
    // Thread safety argument:
    //    This class is threadsafe immutable:
    //    - a is private and final
    //    - a points to a mutable char array, but that array is encapsulated
    //      in this object, not shared with any other object or exposed to a
    //      client, and is never mutated

```

Here are some incorrect thread safety arguments:
```java
/** MyStringBuffer is a threadsafe mutable string of characters. */
public class MyStringBuffer {
    private String text;
    // Rep invariant:
    //   none
    // Abstraction function:
    //   represents the sequence text[0],...,text[text.length()-1]
    // Thread safety argument:
    //   text is a threadsafe immutable String,
    //   so this object is also threadsafe

```

## 序列化
我们对线程安全数据类型的要求是，当客户端并发调用其原子操作时，结果必须与调用的某种顺序排序一致。在本例中，即清除和插入操作中，这意味着清除后插入，或插入后清除。这一特性被称为可序列化：对于任何一组并发执行的操作，其结果（客户可观察到的值和状态）必须是这些操作的某种顺序排序所给出的结果。