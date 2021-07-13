# SQL Group by 语句的注意点

使用Group By子句的时候，一定要记住下面的一些规则：

（1）不能Group By非标量基元类型的列，如不能Group By text，image或bit类型的列

（2）Select指定的每一列都应该出现在Group By子句中，除非对这一列使用了聚合函数；

（3）不能Group By在表中不存在的列；

（4）进行分组前可以使用Where子句消除不满足条件的行；

（5）使用Group By子句返回的组没有特定的顺序，可以使用Order By子句指定次序