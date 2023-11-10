# Debugging
## 系统调试
- 在将整个系统连接在一起时才发现错误
- 在系统部署后由用户报告该错误
- 书 《为什么程序失败》 http://www.whyprogramsfail.com/toc.php

## 重现bug
- 找到产生错误的尽可能小的测试用例
- 将其添加入回归测试以修复

## 科学的寻找bug
- 研究数据(study the data) : 检查错误结果,失败断言,堆栈跟踪
- 提出假设(Hypothesize) : 就错误提出一般性假设
- 实验(experiment) : 验证
- 重复(repeat) : 缩小范围并排除


## 修复bug
- 查找相关的和新产生的错误,think your code
- 撤销调试 , 将debug加入的代码还原
- 回归测试 , 再次测试错误样例

## Summary
科学的发现错误