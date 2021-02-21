### MySQL的执行计划

​	为了知道sql语句的执行,需要查看sql语句的具体执行过程,以加快sql语句的执行效率。

​	可以使用explain/desc +sql语句来模拟优化器执行sql查询语句，从而知道mysql是如何处理sql语句的。

​	官网地址：https://dev.mysql.com/doc/refman/5.5/en/explain-output.html 

#### 执行计划包含的信息

|    Column     |                    Meaning                     |
| :-----------: | :--------------------------------------------: |
|      id       |            The `SELECT` identifier             |
|  select_type  |               The `SELECT` type                |
|     table     |          The table for the output row          |
|  partitions   |            The matching partitions             |
|     type      |                 The join type                  |
| possible_keys |         The possible indexes to choose         |
|      key      |           The index actually chosen            |
|    key_len    |          The length of the chosen key          |
|      ref      |       The columns compared to the index        |
|     rows      |        Estimate of rows to be examined         |
|   filtered    | Percentage of rows filtered by table condition |
|     extra     |             Additional information             |

##### id：

##### select_type: 

主要用来分辨查询的类型，是普通查询还是联合查询还是子查询

| `select_type` Value  |                           Meaning                            |
| :------------------: | :----------------------------------------------------------: |
|        SIMPLE        |        Simple SELECT (not using UNION or subqueries)         |
|       PRIMARY        |                       Outermost SELECT                       |
|        UNION         |         Second or later SELECT statement in a UNION          |
|   DEPENDENT UNION    | Second or later SELECT statement in a UNION, dependent on outer query |
|     UNION RESULT     |                      Result of a UNION.                      |
|       SUBQUERY       |                   First SELECT in subquery                   |
|  DEPENDENT SUBQUERY  |      First SELECT in subquery, dependent on outer query      |
|       DERIVED        |                        Derived table                         |
| UNCACHEABLE SUBQUERY | A subquery for which the result cannot be cached and must be re-evaluated for each row of the outer query |
|  UNCACHEABLE UNION   | The second or later select in a UNION that belongs to an uncacheable subquery (see UNCACHEABLE SUBQUERY) |

```sql
-- simple: 简单查询不包含union和子查询。

-- primary: 查询中若包含任何复杂的子查询，最外层查询则被标记为Primary

-- union: 若第二个select出现在union之后，则被标记为union

-- dependent union: 跟union类似，此处的depentent表示union或union all联合而成的结果会受外部表影响(取集合)

-- union result: 从union表获取结果的select(取值)

-- subquery:在select或者where列表中包含子查询 (取值)

-- dependent subquery:subquery的子查询要受到外部表查询的影响 (取集合)

-- derived: from子句中出现的子查询，也叫做派生类（虚拟表）。

-- uncacheable subquery：表示使用子查询的结果不能被缓存

-- uncacheable union:表示union的查询结果不能被缓存：sql语句未验证
```

##### table：

对应正在访问的那张表，表名或者别名，可能是临时表或者union合并结果集

​	1、如果是具体的表名，则表明从实际的物理表中获取数据，当然也可以是表的别名

​	2、表名是derivedN的形式，表示使用了id为N的查询产生的衍生表

​	3、当有union result的时候，表名是union n1,n2等的形式，n1,n2表示参与union的id







