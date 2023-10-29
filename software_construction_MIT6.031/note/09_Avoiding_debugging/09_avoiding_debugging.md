# 09_avoiding_debugging
## First : make bugs impossible
- static checking
- immutability

## Second : localize bugs
- localize (program to a smal part)
- fail fast (closer to cause)
- defensive programming


## assert
断言: 为防御性检查定义函数,抽象出断言失败时发生了什么并且返回给维护者
> 在java中是默认不启用断言的,因为这会增加复杂度,建议断言只在测试时使用
- [学习JUnit](https://junit.org/junit5/)

### 哪些需要断言/哪些不需要断言
- 需要: 方法参数 / 方法返回值
- 不需要: 操作型语句

## 渐进式的开发
开发一部分即测试一部分
- unit testing
- regression testing

## 模块化与封装


## Summary

[](http://web.mit.edu/6.031/www/sp21/classes/09-avoiding-debugging/#@reading_looked_some)In this reading, we looked at some ways to minimize the cost of debugging:

- [](http://web.mit.edu/6.031/www/sp21/classes/09-avoiding-debugging/#@avoid_debugging_make)Avoid debugging
    - make bugs impossible with techniques like static typing, automatic dynamic checking, and immutability
- Keep bugs confined
    - failing fast with assertions keeps a bug’s effects from spreading
    - incremental development and unit testing confine bugs to your recent code
    - scope minimization reduces the amount of the program you have to search

[](http://web.mit.edu/6.031/www/sp21/classes/09-avoiding-debugging/#@thinking_about_our)Thinking about our three main measures of code quality:

- [](http://web.mit.edu/6.031/www/sp21/classes/09-avoiding-debugging/#@safe_bugs_trying)**Safe from bugs.** We’re trying to prevent them and get rid of them.
- **Easy to understand.** Techniques like static typing, final declarations, and assertions are additional documentation of the assumptions in your code. Variable scope minimization makes it easier for a reader to understand how the variable is used, because there’s less code to look at.
- **Ready for change.** Assertions and static typing document the assumptions in an automatically-checkable way, so that when a future programmer changes the code, accidental violations of those assumptions are detected.