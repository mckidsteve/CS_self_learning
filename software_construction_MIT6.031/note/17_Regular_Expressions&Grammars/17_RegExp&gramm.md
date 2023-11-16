# 正则表达式及其语法
## Grammars
> 语法 :通过规定一种特殊的字符语法来描述信息

语法由一组生成符描述,每一个生成符定义了一个非终结符(nonterminal).
终结符(terminal)是指字面上的字符串独立存在不能再被扩展.
非终结符(nonterminal)则是意味着代表一组字符串的变量.生成符则是该变量在其它变量(nonterminal),运算符(operators)还有常量(terminal)的定义.

语法生成形式: `nonterminal ::= exp of terminal & nonterminal & operators`

### Grammar operators
- 重复符 * : `x ::= y* // x对应0~n个y`
- 连接符 '\space' : `x ::= y z // x对应y和z`
- 联合符 | : `x ::= y | z // x对应y或者z`

- 出现0或1次 ? : `x::= y? // y出现1次(x对应y) 或y出现0次(x对应空字符串)`
- 出现1次或者更多 + : `x ::= y+ // x对应一个或多个y`
- 出现确切次数 {n}:`x::= y{3} // 一个x代表三个y`
- 