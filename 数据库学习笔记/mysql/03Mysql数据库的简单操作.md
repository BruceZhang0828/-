### 03Mysql数据库的简单操作

#### 启动和关闭Mysql数据库

```
net start mysql
net stop mysql
```



#### 数据库连接

```
mysql -h(ip) -P(端口) -u(用户) -p(密码)
```

如果是访问本地数据库,h可以省略:mysql -u(用户) -p



#### 数据库查看版本

未登陆的情况下: mysql -- version 或是mysql -V

登陆之后: select version();

#### 显示操作:show关键字;use关键字

```
show databases;显示所有的数据库
use database(数据库名);切换使用那个数据库
show tables;展示当前数据库中的所有表
show tables from database;查看指定数据库中的表
show columns from table;= desc table; 展示表中的字段信息
show create table func; 展示建表语句
show engines; 展示数据库支持的存储引擎
show variables; 展示系统变量
SHOW VARIABLES like '变量名';展示某个系统变量
```

#### mysql的语法规范

不区分大小写,但建议关键字大写,表名,列名小写;

每条命令最好用';'结尾;

每条命令根据需要进行缩进或换行;

注释

​	单行注释: #注释文字

​	单行注释: -- 注释 有空格

​	多行注释: /* 注释文件 */

​	



