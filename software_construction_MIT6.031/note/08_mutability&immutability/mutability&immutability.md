# mutability&immutability
## 可变的风险
- 传递可变值
- 返回了可变对象
- 使用对同一对象的多个引用可能导致风险(别名风险)

## 突变方法规范
在使用可变对象时应当在规范中明确指出

## test:从迭代器源码看可变
[iter in 6.031](http://web.mit.edu/6.031/www/sp21/classes/08-immutability/)


## 不可变性与性能
当只需要访问某值时,使用一个不变的类型显然优于可变类型.
因为可变类型常常需要进行防御性复制.
但当我们需要对值进行编辑时,情况会出现变化: 
采用一种只copy值内部变化的部分进行编辑,而大部分仍然不变的方式.
这要比将值全部copy为可变值要更好一些,例如git

## 有用的不可变类型


## summary
- [](http://web.mit.edu/6.031/www/sp21/classes/08-immutability/#@safe_bugs_immutable)**Safe from bugs**. Immutable objects aren’t susceptible to bugs caused by aliasing. Unreassignable variables always point to the same object.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/08-immutability/#@easy_understand_because)**Easy to understand**. Because an immutable object or unreassignable variable always means the same thing, it’s simpler for a reader of the code to reason about — they don’t have to trace through all the code to find all the places where the object or variable might be changed, because it can’t be changed.
    
- [](http://web.mit.edu/6.031/www/sp21/classes/08-immutability/#@ready_change_if)**Ready for change**. If an object or a variable can’t be changed at runtime, then code that depends on that object or variable won’t have to be revised when the program changes.