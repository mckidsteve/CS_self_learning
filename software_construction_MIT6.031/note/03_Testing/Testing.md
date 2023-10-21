# Testing

> 了解测试的重要性

## 程序验证
- 验证程序正确性
- 代码审查
- 测试

即使经过最好的验证也难以达到完美的软件质量.
每千行代码残余缺陷率:
缺陷|每一千行|质量
-|-|-
1~10|kloc|正常软件
0.1~1|kloc|高质量的验证
0.01~0.1|kloc|最好的安全验证



## 优先测试的编程
- 规范:编写程序规范 Spec(write specification for function)
- 测试:编写执行规范的测试(write test for specification)
- 实现:编写代码实现(write the implementation)

```java
/**
 * ...
 * comment : a function
 * ...
*/ // 规范
public void function(args){ // 规范
    return // 实现
}

public void testFunction(){
    // 测试
    input a ;
    output b;
}
```

## 系统化测试
- **正确** , 测试应该允许接受规范允许的所有合法实现
- **彻底** , 测试应该可以发现所有可能的错误
- **小型** , 测试应该越小越快越好

## 通过分区选择测试用例
原则:
- 不相交性(disjoint):子域之间不包含公共的测试数据
- 完整性(complete):子域覆盖了程序的每个分支
- 正确性(correct):子域符合了程序测试的要求

### 子域的边界问题
通常由于代码的思维不连贯性,引起边界问题.

例如abs函数在`MINVALUE`上无法取到正确的值,因为`MINVALUE`的绝对值比`MINVALUE`大1.(`MINVALUE` : -2^31 = [-2147483648] 而 `MAXVALUE` : 2^31 - 1 = [2147483647])

## 使用JUnit进行单元化的测试
将程序的模块分割为多个单元, 每个单元进行测试.称作单元测试
[Junit](https://junit.org/junit5/)

```java
assertEquals(expect , actual); // left ?= right
```

## Doc your testing strategy
随时将test_suite记录在函数文档中

## 黑盒与白盒测试

- **黑盒**意味着只从规范中选择测试样例,而不是从函数的实现
- **白盒**是基于代码的测试,需要根据函数的实际实现来选择分区.而未明确要求的实现行为不需要包含在测试用例中
```java
// 一个白盒测试实例

/**
 * Sort a list of integers in nondecreasing order.  Modifies the list so that 
 * values.get(i) <= values.get(i+1) for all 0<=i<values.length()-1
 */
public static void sort(List<Integer> values) {
    // choose a good algorithm for the size of the list
    if (values.length() < 10) {
        radixSort(values);
    } else if (values.length() < 1000*1000*1000) {
        quickSort(values);
    } else {
        mergeSort(values);
    }
```

## 覆盖范围
- 语句覆盖率：是否每个语句都被某个测试用例运行？
- 分支覆盖率：对于程序中的每一个或语句，是否都由某个测试用例进行了真假方向的测试？
- 路径覆盖率：每个可能的分支组合--程序中的每条路径--是否都被某个测试用例所采用？

## 集成测试
集成测试是对于整个程序或者整个模块的组合进行测试,因为程序可能在模块之间的连接上出错

## 自动化测试
自动进行代码测试

## 回归测试
修复错误后将原错误输回测试

## Summary
- 测试驱动编程。在编写代码之前，先编写测试。
- 系统地选择测试用例的分区和边界。
- 白盒测试和声明范围，用于填写测试套件。
- 尽可能孤立地对每个模块进行单元测试。
- 自动化的回归测试可防止错误再次出现。

优质程序的三个属性
- 避免错误
- 容易理解
- 准备好进行更改