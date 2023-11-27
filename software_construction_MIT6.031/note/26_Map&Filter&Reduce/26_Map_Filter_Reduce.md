# 映射 , 过滤 , 聚合

## 导言：一个示例
假设我们面临以下问题：编写一个方法，找出项目中 Java 文件中的所有单词。

按照良好的做法，我们将问题分解成几个较简单的步骤，并为每个步骤编写一个方法：

- 从项目的根文件夹开始递归扫描，找到项目中的所有文件
- 将它们限制为具有特定后缀的文件，在本例中为 .java
- 打开每个文件并逐行读取
- 将每一行分成单词
在编写这些子步骤的各个方法时，我们会发现自己编写了大量底层迭代代码

我们使用map/filter/reduce设计模式 , 简化了对元素序列操作的实现 .

## 抽象化出控制流
### 迭代器抽象
对于任何元素序列,都可以抽象出 `size` , `get` , `list`:
于是可以提供`iterable`抽象接口:
```java
Iterator<File> iter = files.iterator();
while (iter.hasNext()){
    File f = iter.next();
}
```

### Map / Filter / Reduce 抽象
这三者类似于iterator , 但在更高的层次上将整个元素序列视为一个单元 . 
- [流`Stream`](http://docs.oracle.com/en/java/javase/15/docs/api/java.base/java/util/stream/Stream.html) : 流是以下序列化数据类型的构成基础
- [映射`map`](http://web.mit.edu/6.031/www/sp21/classes/26-map-filter-reduce/#map) : Stream<E> * (E -> F) => Stream<F> 一个映射就是从流E转化为流F的过程(E->F即为转化关系式)
- [过滤`Filter`](http://web.mit.edu/6.031/www/sp21/classes/26-map-filter-reduce/#filter) : Stream<E> * (E -> boolean) => Stream<E> 过滤的作用是滤去E中不符合E->bool的元素
- [聚合`Reduce`](http://web.mit.edu/6.031/www/sp21/classes/26-map-filter-reduce/#reduce) : Stream<E> * E * (E * E -> E) => E Reduce的作用是将初始值E和元素E按照关系(EE->E)组合成为新的E返回

