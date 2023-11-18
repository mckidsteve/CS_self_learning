# 解析器
## 解析生成器
> 根据语法接收字符序列并进行匹配

通过生成解析树,分析语法解析器的运行过程
[ParserLib](http://6031.mit.edu/parserlib/3.2.0/doc/)

### parserlib语法
每条parserlib规则都由`名称` , `::=` , `定义` , `;`组成
还可以包含java注释
eg :`text ::= [^<>]+; /*`

按照惯例使用小写来表示非终结符,使用带引号的字符串表示终结符.与普通的正则不同,parserlib要求字面的字符必须使用引号或者字符括号括起来: `(alpha|beta|[c-z])*` => `('alpha'|'beta'|[c-z])*`

### 空白
可以改写语法来判定空格是否需要
但parserlib允许使用速记法来表示应该跳过的字符
```java
@skip whitespace{
    expr ::= sum;
    sum ::= prim ('+' prim)*;
    prim ::= num | '(' sum ')';
}
whitespace ::= [ \t\r\n]+ ;
num ::= [0-9]+;
```
@skip表示与whitespace非终端的匹配将被忽略,因此以上语法实际变成了:
```java
expr ::= whitespace* sum whitespace* ;
sum ::= whitespace* primary whitespace* (whitespace* '+' whitespace* primary whitespace*)* whitespace* ;
primary ::= whitespace* number whitespace* | whitespace* '('  whitespace* sum  whitespace* ')' whitespace* ;
```


## 生成解析
