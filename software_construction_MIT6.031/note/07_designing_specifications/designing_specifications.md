# designing_specifications
## 如何设计你的规范
- 确定性规范和未确定规范: 
  deterministic:只要遵从规范即返回确定的结果
  nondeterministic:遵从规范但是根据输入运行不同的结果

- 声明型规范和操作型规范: 
  Declarative: 只给出最终输出不解释过程
  operational: 给出繁杂的实现过程

- 强规范与弱规范: 
  强规范的实现集应该是弱规范的实现集的严格子集

## 如何写好你的规范
- 连贯(coherent):  连贯意味着函数应该是一个单一完整的个体,而没有很多cases
- 翔实(informative): 输入输出都应该考虑所有的情况
- strong enough: 应当保证最基础的需求
- weak enough : 应当保证一定的细节

## 关于访问控制
更多更合理的设置方法的公有私有访问能让代码更加的RFC/ETU/SFB

## 关于静态与实例型方法
实例方法(不带static声明)意味着方法必须在特定实例上使用.
