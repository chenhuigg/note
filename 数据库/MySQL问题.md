**问题描述**

![](https://user-gold-cdn.xitu.io/2019/6/29/16b9efb19c12bc75?w=454&h=42&f=png&s=2436)

ERROR 1111 (HY000): Invalid use of group function

<font color=gray >在where句子中使用聚合函数max()，出现了上面的错误。</font>

**问题原因**

SQL语句的执行过程

from→where→group by→having→order by→select

聚集函数是针对结果集进行的，但where条件并不是在查询出结果集之后运行的，所以聚合函数放在where中，会报错。

**解决方案**

在这里，可以使用having，having对查询出的结果进行筛选，一般情况下，having与group by共同使用。

**优化建议**

`select * from blog_table group by id having tag=max(status); `

**学习总结**

本次错误，我明白了SQL语句的大概的执行流程，我把它们分为两个部分

第一部分是通过SQL命令获取结果集（获取结果集之前运行），

第二部分是对结果集进行筛选（获取结果集之后运行）。

对于上面给出的SQL语句执行过程（from→where→group by→having→order by→select），group by之前（包括group by）都是对处理前的数据进行筛选，group by之后是对获取结果集之后的数据进行筛选。