### 02 Mysql数据库的安装

#### window下Mysql数据库的安装

##### Mysql5.7数据库安装

​	下载地址: https://dev.mysql.com/downloads/mysql/; 选择相关的版本进行下载

​	1. 进行安装:

​	2. 解压安装文件:

​	3. 在mysql目录中新建data目录.新建my.ini文件

​	4. 在my.ini文件内复制下边的内容:

```
[Client]
port = 3306

[mysqld]
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=E:\install_work\mysql
# 设置mysql数据库的数据的存放目录
datadir=E:\install_work\mysql\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB

[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
```

5. 以管理员身份启动命令行工具: 在mysql目录下边的bin目录

6. 执行下边命令:

```
mysqld --initialize --user=mysql --console
```

7. 执行成功之后,需要记录临时密码:

```
 A temporary password is generated for root@localhost: t3?fenS2yieo
```

8. 执行mysql安装命令:

```
mysqld --install mysql
```

9. 启动mysql:

```
net start mysql
```

10. 提示启动成功后,登录mysql

```mysql -u root -p 临时密码
mysql -u root -p 临时密码
```

11. 登录成功之后,修改临时密码:

```
set password=password('root');

**注意**
1.句尾的分号;
2.新密码设置一个简单的,我这里设置的root
```

12. 设置Mysql到环境变量下

    1. 新建MYSQL_HOME = mysql的安装目录
    2. 在Path新添加%MYSQL_HOME%\bin目录
    3. 点击一系列的确定按钮即可

    
    
    
    
    **可能遇到的问题**
    
    输入mysqld --initialize --user=mysql --console时出现：
    
    由于系统找不到MSVCR120.dll，无法继续执行代码。重新安装程序可能会解决此问
    
    解决方案：
    
    下载DirectX修复工具增强版，参考地址：`http://blog.csdn.net/vbcom/article/details/7245186`