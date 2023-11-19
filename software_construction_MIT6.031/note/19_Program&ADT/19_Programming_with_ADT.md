# Writing a program with ADTs
> Goals : 了解如何在软件开发中的不同层面应用规范,测试和迭代: 从单一方法到抽象类型,再到整个程序

## 编程方法小则
1. **编写方法** : 
    1. 规范. javadoc:方法名,参数,返回,异常
    2. 测试. 编写系统的测试用例
    3. 实现. 编写方法的主体使得所有测试都被通过

2. **编写抽象数据类型的方法**:
    1. 规范. 编写数据类型的操作规范
    2. 测试. 为ADT的操作编写测试用例
    3. 实现. 分为以下几步:
        a. 选择rep : 将rep不变量和抽象函数记录在注释里
        b. 断言rep不变量 : 实现一个强制执行rep不变量的方法.
        c. 实现操作 : 编写操作方法

3. **编写程序的方法的总结**:
    1. 选择数据类型 : 决定可变与不可变数据类型
    2. 选择静态方法 : 从最顶层的main开始分解执行步骤
    3. 规范. 指定ADT和方法的规范.
    4. 测试. 编写测试样例
    5. 简单实现. 跳过高级实现先将程序跑起来
    6. 迭代. 优化

# 例题 : 矩阵乘法
> 我们希望设计一种优化计算的方式来计算矩阵的乘法
> 设 a , b是标量常数 , X是一个很大的矩阵 , 那么`(aX)b`的计算会很慢.因为他会在X上循环两次(一次乘a一次乘b)

## Slove
### 规范
```
//定义对数据类型MatExp的操作

@unc1
make : double -> MatExp
effects : 返回一个只包含了给定标量的表达式

@func2
make : double[][] array -> MatExp
requires : 数组非空,且数组元素的长度相等且非0 , 适用于所有合法索引i
effects : 返回一个只含有给定矩阵的表达式

@func3
times : MatExp1 * MatExp2 -> MatExp
requires : m1和m2是相容合法的乘法运算
effects : 返回m1*m2

@func4
isIdentity : MatExp -> boolean
effects : 若表达式符合乘法表达则返回true

@func5
optimize : MatExp -> MatExp
effects : 返回一个等价但更高效的表达式
```

### 测试
Partition:
- 分区1 : 0 , 1 , 2 , >2
- 分区2 : 标量位于树的 左,右,左左,右左,左右,右右,超过2层.

| [](http://web.mit.edu/6.031/www/sp21/classes/19-programming-with-adts/#@test_case_partitions)Test case |  | Partitions covered |
| --- | --- | --- |
| X ⇒ X | covers | 0 scalars |
| aX ⇒ aX |  | 1 scalar, left child |
| a(Xb) ⇒ (ab)X |  | 2 scalars, left child, right-child-of-right-child |
| (aX)b ⇒ (ab)X |  | 2 scalars, right child, left-child-of-left-child |
| (Xa)(bY) ⇒ (((ab)X)Y) |  | 2 scalars, left-child-of-right-child, right-child-of-left-child |

### 实现
#### 选择rep
矩阵 => 本体 + 常量参数 + 矩阵 + 结果
``MatExp = Identity + Scalar(value : double) + Matrix(array : double[][]) + Product(m1 , m2)``

```java
// 表示矩阵和标量乘积的不可变表达式
public interface MatrixExp {

}

class Identity implements MatrixExp {
    public Identity(){

    }
}

class Scalar implements MatrixExp {
    private final double value;
    // RI (代表性不变量): true
    // AF (value) = 真实标量

    public Scalar(double value){
        this.value = value;
    }
}

class Matrix implements MatrixExp {
    private final double[][] array ;

    //Spec
    public Matrix(double[][] array){
        this.array = array; //rep的暴露破坏了Martrix的不变性, 因为该可变数组被引用了
    }
}

class Product implements MatrixExp {
    private final MatrixExp m1;
    private final MatrixExp m2;
    // RI: m1's column count == m2's row count, or m1 or m2 is scalar
    // AF(m1, m2) = the matrix product m1*m2

    public Product(MatrixExp m1 , MatrixExp m2){
        this.m1 = m1;
        this.m2 = m2;
    }
}
```

#### 选择Identity
```java
// 所有矩阵计算的统一遵从
public static final MatrixExp I = new Identity();
```

#### 实现make函数(通过使用静态工厂的方法)
```java
// 返回只有标量值的表达式
public static MatrixExp make(double value) {
    return new Scalar(value);
}

// 返回只有给定矩阵的表达式
public static MatrixExp make(double[][] array){
    return new Matrix(array);
}
```

#### 实现isIdentity(不使用instanceof)
在面向对象的编程中不适合使用instanceof

```java
class Identity :
    ...
    public boolean isIdentity(){
        return true; // a identity always 
    }

class Scalar :
    ...
    public boolean isIdentity(){
        return value == 1; // when scalar is 1
    }

class Matrix :
    ...
    public boolean isIdentity(){
        if (array.length != array[0].length){
            return false;
        }
        for(int row = 0; row < array.length; row ++){
            for (int col = 0;col < array[row].length ; col ++){
                double expected = (row == col) ? 1 : 0;
                if (array[row][col] != expected){
                    return false;
                }
            }
        }
        return true;
    }

class Product:
    ...
    public boolean isIdentity(){
        return m1.isIdentity() && m2.isIdentity();
    }
```
Q : 对于值相同的product不一定返回true
A : 建议弱化isIdentity的规格

#### 实现optimize(不适用instanceof)
```java
class Identity:
    ...
    public MatrixExp scalars(){ return this;}
    public MatrixExp matrices(){ return this;}
    public MatrixExp optimize(){ return this;}

class Scalar :
    ...
    public MatrixExp scalars(){ return this;}
    public MatrixExp matrices(){ return I;}
    public MatrixExp optimize(){ return this;}

class Matrix :
    ...
    public MatrixExp scalars(){ return I;}
    public MatrixExp matrices(){ return this;}
    public MatrixExp optimize(){ return this;}

class Product implements MatrixExp {
    ...
    public MatrixExp scalars(){
        return times(m1.scalars() , m2.scalars());
    }
    public MatrixExp Matrices(){
        return times(m1.matrices() , m2.matrices());
    }
    public MatrixExp optimize(){ return times(scalars() , matrices());}
}
```

