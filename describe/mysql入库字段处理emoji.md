
**目录**

1 [问题](#1)  
2 [处理方式](#2)   
3 [实例](#3) 

**1问题**<a name="1"></a>
---
  当数据库表中存在一个字段，录入值时存在emoji的表情包时，就会报错 error code [1366]; Incorrect string value: '\xF0\x9F\x91\x8F\xF0\x9F...' for column 'content' at row 1;
 
**2处理方式**<a name="2"></a>
---
  1、首先要保证mysql的版本再5.5.1+以上的版本，select version();
  2、我们数据库最好把默认字符集由utf8 更改为utf8mb4（也可以不改），对应的表默认字符集也更改为utf8mb4（必须改），存储表情的字段默认字符集也要改成utf8mb4（必须改）。执行sql可以到实例看！
  3、如果是用jdbcTemplate进行数据库操作的话，要在配置连接池中添加属性spring.datasource.base.initSQL=set names utf8mb4;（被添加到连接池之前执行的sql）。注意不同版本的链接池set names utf8mb4方式不同。（有spring.datasource.base.initSQL、spring.datasource.connection-init-sql等...）
  
 **3实例**<a name="3"></a>
 ```
 # 修改数据库:  
2 ALTER DATABASE database_name CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;  
3 # 修改表:  
4 ALTER TABLE table_name CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;  
5 # 修改表字段:  
6 ALTER TABLE table_name CHANGE column_name column_name VARCHAR(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci; 
7 ALTER TABLE table_name CHANGE column_name column_name text CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
