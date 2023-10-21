# Static Checking
> **Topics:**
> 1. 静态类型 
> 2. SFB ; ETU ; RFC


### 静态检查
静态检查（Static Checking）是编译器在编译源代码时对源代码进行类型检查的过程。
- 在程序运行前发现错误

### 动态检查
动态检查（Dynamic Checking）是程序运行时对程序进行类型检查的过程。
- 在程序运行后发现错误


## JAVA整数除法陷阱

- 整数除法 (5/2)
- 整数溢出 (int/long)
- 浮点特殊值 (NaN,POSITIVE_INFINITY,NEGATIVE_INFINITY)



## 数组和集合
### 定长序列
```java
int[] a = new int[100];
```
- 索引：`a[2]`
- 分配：`a[2]=0`
- 长度：`a.length`

定长序列会有数组溢出情况发生。

### 变长序列
```java
List<Integer> list = new Arraylist<>();
```
- 索引：`list.get(2)`
- 分配：`list.set(2,0)`
- 长度：`list.size()`

定长序列不会有数组溢出情况发生。
### 遍历元素
```java
// find the maximum point of a hailstone sequence stored in list
int max = 0;
for (int x : list) {
    max = Math.max(x, max);
}
```

## 不变量
最好使用 `final` 来声明方法的参数和尽可能多的局部变量。


```java
public static List<Integer> hailstoneSequence(final int n) { 
    final List<Integer> list = new ArrayList<Integer>();
```
`final int n = 5`

像变量的类型一样，这些声明是重要的文档，对代码阅读者很有用，并由编译器进行静态检查。