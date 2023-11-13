# 等恒性(Equality)
> - 了解等价性
> - 区分等价与对象等价
> - 区分可变对象的严格观测等价和行为等价
> - 理解对象契约 , 能正确为可变/不可变的对象类型设计等价性

## 等价关系(Equivalence relation)
若$(x , y) \in E $ , E满足 
1. 自反性reflexive:
`t == t  ∀ t ∈ T`
2. 对称性symmetric:
`t == u  ⇒  u == t`
3. 传递性transitive:
`t == u ∧ u == v  ⇒  t == v`

## 不可变类型的等价性
不可变类型的等价性定义如下:
- **从抽象函数的角度** : 对于抽象函数AF : R->A . 当且仅当AF(a) = AF(b) , a = b
- **从观察者的角度**: 对于两个观察一致的对象可以认为相等 , 即两者ADT操作上没有区别

## == vs equals()

- == 测试引用是否相等 , 即两者指向的存储空间是否相同
- equals()比较两者内容是否相等

<table class="equality-table"><tbody><tr><td></td><td><p id="@referenceequality"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@referenceequality"></a><em>reference<br>equality</em></p></td><td><p id="@objectequality"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@objectequality"></a><em>object<br>equality</em></p></td></tr><tr><td><p>Java</p></td><td><p><code>==</code></p></td><td><p><code>equals()</code></p></td></tr><tr><td><p id="@objective_c"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@objective_c"></a>Objective C</p></td><td><p><code>==</code></p></td><td><p id="@isequal"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@isequal"></a><code>isEqual:</code></p></td></tr><tr><td><p>C#</p></td><td><p><code>==</code></p></td><td><p><code>Equals()</code></p></td></tr><tr><td><p id="@python"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@python"></a>Python</p></td><td><p><code>is</code></p></td><td><p><code>==</code></p></td></tr><tr><td><p id="@javascript"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@javascript"></a>JavaScript</p></td><td><p><code>==</code></p></td><td><p id="@n-a"><a class="jump" href="http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@n-a"></a>n/a</p></td></tr></tbody></table>

## equals实现
equals默认定义在object内
```java
public class Object { 
     public boolean equals(Object that) { 
        return this == that; } 
     }
```

> 了解: [动态调度](http://web.mit.edu/6.031/www/sp21/classes/12-interfaces-generics-enums/#overriding_and_dynamic_dispatch)


正确的 `equals()` 实现：

```java
@Override
public boolean equals(Object that) {
    return that instanceof Duration && this.sameValue((Duration)that);
}

// returns true iff this and that represent the same abstract value
private boolean sameValue(Duration that) {
    return this.getLength() == that.getLength();
}
```
- `instanceof` 测试传入的 `that` 是 `Duration` 或 `Duration` 的子类
- 调用 `sameValue()` 去判断它们的值是否相等


```java
//test
Duration d1 = new Duration(1, 2);
Duration d2 = new Duration(1, 2);
Object o2 = d2;
d1.equals(d2) → true
d1.equals(o2) → true
```
### instanceof
用来测试一个实例是否属于特定的类型
- 是动态检查
- 在面向对象编程中使用 `instanceof` 是一个不好的选择
- 除了实现相等操作，`instanceof` 不能被使用


## 对象契约
对于`Object`的规范称为对象契约:
- `equals`应当定义一个严格等价关系
- `equals`应当确定:连续重复操作返回相同结果
- `equals`对于非null的引用x,x.equals(null)=false
- 若两个对象equals为真,则其hash值也应当相同

### 破坏等价关系

`equals()` 操作必须构建出一个满足自反性、对称性、传递性的等价关系。
如果没有满足，那么与相等相关的操作（例如集合、搜索）将变得不可预测。

```java
@Override
public boolean equals(Object that) {
    return that instanceof Duration && this.sameValue((Duration)that);
}

private static final int CLOCK_SKEW = 5; // seconds

// returns true iff this and that represent the same abstract value within a clock-skew tolerance
private boolean sameValue(Duration that) {
    return Math.abs(this.getLength() - that.getLength()) <= CLOCK_SKEW;
}

// test
Duration d_0_60 = new Duration(0, 60);
Duration d_1_00 = new Duration(1, 0);
Duration d_0_57 = new Duration(0, 57);
Duration d_1_03 = new Duration(1, 3);
```

- 传递性无法满足。 `d_0_57` equals `d_1_00`，`d_1_00` equals `d_1_03`，然而 `d_0_57` 不 equals `d_1_03`。

### 破坏哈希表
ADT 必须提供 `equals` 和 `hashCode` 两个方法。相同的 objects 的 `hashCode` 必须相同，不然无法在哈希表中找到相同的 object。

Object 默认的 `hashCode()` 实现和默认的 `equals()` 保持一致：

```java
public class Object {
  ...
  public boolean equals(Object that) { return this == that; }
  public int hashCode() { return /* the memory address of this */; }
}
```
对于索引a和b，如果 `a == b`，那么a和b的存储地址也就相同，`hashCode()`的结果也就相同。所以Object的契约满足。

对于不可变对象来说，它们需要重新实现 `hashCode()`
```java
Duration d1 = new Duration(1, 2);
Duration d2 = new Duration(1, 2);
d1.equals(d2) → true
d1.hashCode() → 2392
d2.hashCode() → 4823
```
d1 和 d2 是 `equals()` 为真的，但是它们的 `hashCode` 不一样，所以我们需要修复它。

只要你满足了相等的对象产生相同的 `hashCode`，不管你的 `hashCode` 是如何实现的，你的代码都会是正确的。
哈希碰撞仅仅只会性能，而一个错误哈希方法则会带来错误！
**当你覆盖 equals 后，将 hashCode 也覆盖。**


## 可变类型的等价性
- **观察相等** : 当此时无法区分两个引用是否相等时,仅仅观察两者操作结果是否看起来相同
- **行为相等** : 始终测试两个引用在现在和未来所有状态下是否行为相同

对于不可变对象 , 观察相等和行为相等是一样的,因为对象状态不变
但对于可变对象 , 观察相等可能是一种节省的设计 , 但是使用观察相等会带来隐性bug : 当一个`List`存入`Set`,改变`List`,其equals和hashcode均改变,此时`set`会认为这个`list`不存在

> 在`java.util.Set`规范中说到:当可变对象作为集合元素时应当注意
>
> 因此**可变类型的`equals()`应当保证行为相等**,其`equals`和`hashcode`应当从`Object`继承而不进行`override`
> 
> 对于需要观察相等操作的可变类型（即当前状态下是否“看起来”一样），最好是设计一个新的操作。 `similar()` 或 `sameValue()`。不幸的是Java没有采取这种设计。

## 对于`equals`和`hashcode`的总结
- 对于不可变类型:
    - 行为平等与观察平等相同
    - 应当重载equals来比较抽象对象的值
    - 应当重载hashCode才能映射抽象对象的值

- 对于不可变对象:
    - equals一般不应该被重写而是始终继承Object.equals实现,类似于==的用法
    - hashCode同样
    - 对于需要观察相等的对象,最好定义一个新的操作(如samevalue)


## 自动闭包(autoboxing)与等价性
在java中,equals对基元类型和基础类型的返回并不一致,if you create two `Integer` objects with the same value, they’ll be `equals()` to each other:

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@integer_x_new)`Integer x = new Integer(3);
Integer y = new Integer(3);
x.equals(y) → true`

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@but_there-s_subtle)But there’s a subtle problem here; `==` is overloaded. For reference types like `Integer`, it implements reference equality:

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@x_y_returns)`x == y // returns false`

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@but_primitive_types)But for primitive types like `int`, `==` implements behavioral equality:

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@int-x_int-y_returns)`(int)x == (int)y // returns true`

[](http://web.mit.edu/6.031/www/sp21/classes/15-equality/#@so_you_can-t)So you can’t really use `Integer` interchangeably with `int`. The fact that Java automatically converts between `int` and `Integer` (this is called [_autoboxing_ and _autounboxing_](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)) can lead to subtle bugs! You have to be aware what the compile-time types of your expressions are