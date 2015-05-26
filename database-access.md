# Lua 数据库访问  

简单的数据操作，我们用文件就可以处理。但是，某些时候文件操作存在性能、扩展性等问题。这时候，我们就需要使用数据库。LuaSQL 是一个提供数据库操作的库，它支持多种 SQL 数据库的操作。包括：  

<ul>
	<li>SQLite</li>
	<li>MySQL</li>
	<li>ODBC</li>
</ul>  
在本教程中，我们会讲解用 Lua 语言对 MySQL 数据库与 SQLite 数据库进行操作。这些操作具有一般性，它们也可以移植到其它类型 SQL 数据库中。首先让我们看一下如何操作 MySQL 数据库。  

## MySQL 数据库环境设置 

为了下面的例子可以正确演示，我们需要首先初始化数据库设置。我们假设你已经完成了如下的工作：  


<ul>
	<li>安装 MySQL 数据库，使用默认用户名 root， 默认密码为： 123456。</li>
	<li>已经创建数据库 test。</li>
	<li>已经阅读过关于 MySQL 的基本教程，并掌握了 MySQL 的基本知识。</li>
</ul>  

## 导入 MySQL  

假设你已经安装配置正确了，那么我们可以使用 require 语句导入 sqlite 库。安装过程中会产生一个存储数据相关文件的目录 libsql。  

```
mysql = require "luasql.mysql"
```  

我们可以通过 mysql 变量访问 luasql.mysql 中的 mysql 表，该表中存存储数据库操作相关的函数。

### 建立连接 

先初始化 MySQL 的环境，再建立一个连接。如下所示：  

```
local env  = mysql.mysql()
local conn = env:connect('test','root','123456')
```  

上面的程序会与已存在的 MySQL 数据库 test 建立连接。

### 执行函数

LuaSQL 库中有一个 execute 函数，此函数可以完成所有数据加操作，包括创建、插入、更新等操作。其语法如下所示：  

```
conn:execute([[ 'MySQLSTATEMENT' ]])
```  

执行上面的语句这前，我们需要保证与 MySQL 数据库的连接 conn 是打开的，同时将 MySQLSTATEMENT 更改为合法的 SQL 语句。  

### 创建表

下面的示例演示如何创建一个数据库表。例子中为表创建了两个属性分别为 id 和 name，其类型分别为整数和 vchar。  

```
mysql = require "luasql.mysql"

local env  = mysql.mysql()
local conn = env:connect('test','root','123456')
print(env,conn)

status,errorString = conn:execute([[CREATE TABLE sample2 (id INTEGER, name TEXT);]])
print(status,errorString )
```  

运行上面的程序后，数据库中创建了一个表 sample，该表有两列，属性名分别为 id 和 name。

```
MySQL environment (004BB178)	MySQL connection (004BE3C8)
0	nil
```

如果发生错误，则函数将返回一个错误消息，成功执行则返回 nil。下面是错误消息的一个例子：  

```
LuaSQL: Error executing query. MySQL: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '"id INTEGER, name TEXT)' at line 1
```

### 插入语句  

ＭySQL 插入语句的示例如下所示：  

```
 conn:execute([[INSERT INTO sample values('11','Raj')]])
```  

### 更新语句  

ＭySQL 更新语句的示例如下所示：  

```
conn:execute([[UPDATE sample3 SET name='John' where id ='12']])
```

### 删除语句  

ＭySQL 删除语句的示例如下所示：  

```
conn:execute([[DELETE from sample3 where id ='12']])
```  

### 查找语句   

成功查找返回后，我们需要循环遍历返回的所有行以取得我们需要的数据。查找语句的示例如下：  

```
cursor,errorString = conn:execute([[select * from sample]])
row = cursor:fetch ({}, "a")
while row do
  print(string.format("Id: %s, Name: %s", row.id, row.name))
  -- reusing the table of results
  row = cursor:fetch (row, "a")
end
```  

上面的代码中，我们先打开了一个 MySQL 连接。通过 execute 函数返回的游标(cursor)，我们可以使用游标遍历返回的表，取得我们查找的数据。

### 完整示例  

下面这个例子用到了所有上面提到的数据的操作函数，请看下面这个完整的例子：  

```
mysql = require "luasql.mysql"

local env  = mysql.mysql()
local conn = env:connect('test','root','123456')
print(env,conn)

status,errorString = conn:execute([[CREATE TABLE sample3 (id INTEGER, name TEXT)]])
print(status,errorString )

status,errorString = conn:execute([[INSERT INTO sample3 values('12','Raj')]])
print(status,errorString )

cursor,errorString = conn:execute([[select * from sample3]])
print(cursor,errorString)

row = cursor:fetch ({}, "a")
while row do
  print(string.format("Id: %s, Name: %s", row.id, row.name))
  row = cursor:fetch (row, "a")
end
-- close everything
cursor:close()
conn:close()
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
MySQL environment (0037B178)	MySQL connection (0037EBA8)
0	nil
1	nil
MySQL cursor (003778A8)	nil
Id: 12, Name: Raj
```  

## 执行事务  

事务是数据库中保证数据一致性的一种机制。事务有以下四个性质：  
<ul>
	<li>原子性：一个事务要么全部执行要么全部不执行。</li>
	<li>一致性：事务开始前数据库是一致状态，事务结束后数据库状态也应该是一致的。</li>
	<li>隔离性：多个事务并发访问时，事务之间是隔离的，一个事务的中间状态不能被其它事务可见。</li>
	<li>持久性： 在事务完成以后，该事务所对数据库所做的更改便持久的保存在数据库之中，并不会被回滚。</li>	
</ul>
事务以 START_TRANSACTION 开始，以 提交（commit）或 回滚（rollback）语句结束。  

### 事务开始  

为了初始化一个事务，我们需要先打开一个 MySQL 连接，再执行如下的语句：  

```
conn:execute([[START TRANSACTION;]])
```  

### 事务回滚  

当需要取消事务执行时，我们需要执行如下的语句回滚至更改前的状态。

```
conn:execute([[ROLLBACK;]])
```  

### 提交事务  

开始执行事务后，我们需要使用 commit 语句提交完成的修改内容。

```
conn:execute([[COMMIT;]])
```

前面我们已经了解了 MySQL 的基本知识。接下来，我们将解释一下基本的  SQL 操作。请记住事务的概念，虽然我们在 SQLite3 中我们不在解释它，但是它的概念在 SQLite3 中同样适用。  

## 导入 SQLite 

假设你已经安装配置正确了，那么就可以使用 require 语句导入 sqlite 库。安装过程中会产生一个存储数据相关文件的目录 libsql。  

```
 sqlite3 = require "luasql.sqlite3"
```  

通过 sqlite3 变量可以访问提供的所有数据库操作相关函数。  

### 建立连接  

我们先初始化 sqlite 环境，然后为该环境创建一个连接。语法如下：  

```
local env  = sqlite3.sqlite3()
local conn = env:connect('mydb.sqlite')
```  

上面的代码会与一个 sqlite 文件建立连接，如果文件不存在则创建新的 sqlite 文件并与该新文件建立连接。  

### 执行函数  

LuaSQL 库中有一个 execute 函数，此函数可以完成所有数据加操作，包括创建、插入、更新等操作。其语法如下所示：  

```
conn:execute([[ 'SQLite3STATEMENT' ]])
```  

执行上面的语句这前，我们需要保证与 MySQL 数据库的连接 conn 是打开的，同时将 SQLite3STATEMENT 更改为合法的 SQL 语句。  

### 创建表

下面的示例演示如何创建一个数据库表。例子中为表创建了两个属性分别为 id 和 name，其类型分别为整数和 vchar。  

```
sqlite3 = require "luasql.sqlite3"

local env  = sqlite3.sqlite3()
local conn = env:connect('mydb.sqlite')
print(env,conn)

status,errorString = conn:execute([[CREATE TABLE sample ('id' INTEGER, 'name' TEXT)]])
print(status,errorString )
```  

运行上面的程序后，数据库中创建了一个表 sample，该表有两列，属性名分别为 id 和 name。

```
SQLite3 environment (003EC918)	SQLite3 connection (00421F08)
0	nil
```  

如果发生错误，则函数将而一个错误消息；若成功执行则返回 nil。下面是错误消息的一个例子：  

```
LuaSQL: unrecognized token: ""'id' INTEGER, 'name' TEXT)"
```  

### 插入语句  

插入语句的示例如下所示：  

```
 conn:execute([[INSERT INTO sample values('11','Raj')]])
```  

### 查找语句   

查找返回后，我们需要循环遍历每行以取得我们需要的数据。查找语句的示例如下：  

```
cursor,errorString = conn:execute([[select * from sample]])
row = cursor:fetch ({}, "a")
while row do
  print(string.format("Id: %s, Name: %s", row.id, row.name))
  -- reusing the table of results
  row = cursor:fetch (row, "a")
end
```  

上面的代码中，我们先打开了一个 sqlite3 连接。通过 execute 函数返回的游标(cursor)，我们可以遍历返回的表，以取得我们查找的数据。

### 完整示例  


下面这个例子用到了所有上面提到的数据的操作函数，请看下面这个完整的例子： 

```
sqlite3 = require "luasql.sqlite3"

local env  = sqlite3.sqlite3()
local conn = env:connect('mydb.sqlite')
print(env,conn)

status,errorString = conn:execute([[CREATE TABLE sample ('id' INTEGER, 'name' TEXT)]])
print(status,errorString )

status,errorString = conn:execute([[INSERT INTO sample values('1','Raj')]])
print(status,errorString )

cursor,errorString = conn:execute([[select * from sample]])
print(cursor,errorString)

row = cursor:fetch ({}, "a")
while row do
  print(string.format("Id: %s, Name: %s", row.id, row.name))
  row = cursor:fetch (row, "a")
end
-- close everything
cursor:close()
conn:close()
env:close()
```  

运行上面的程序，我们可以得到如下的输出结果：  

```
SQLite3 environment (005EC918)	SQLite3 connection (005E77B0)
0	nil
1	nil
SQLite3 cursor (005E9200)	nil
Id: 1, Name: Raj
```  

使用 libsql 库我们可以执行所有的数据库操作。所以，看完这些例子后，请自己多做一些练习。
