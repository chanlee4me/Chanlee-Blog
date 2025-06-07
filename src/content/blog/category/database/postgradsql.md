---
title: "postgradsql笔记"
description: "postgradsql的一些常见命令，以及一些使用过程中遇到的问题"
pubDate: "Jul 06 2025"
image: "/image/category/database/postgresqlicon.png" 
categories:
  - database
tags:
  - Makrdown
badge: Pin
---



# 常用命令

## 连接与退出

```postgresql
psql -U username -d database -h host -p port  # 连接数据库
\q                                       # 退出psql
```



## 数据库操作

```postgresql
CREATE DATABASE dbname;                  # 创建数据库
DROP DATABASE dbname;                    # 删除数据库
\l                                      # 列出所有数据库
\c dbname                                # 切换到指定数据库
```



## 表操作

```postgresql
CREATE TABLE table_name (                # 创建表
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);

DROP TABLE table_name;                   # 删除表
\dt                                     # 列出当前数据库所有表
\d table_name                           # 显示表结构		
```



## 数据操作

```postgresql
INSERT INTO table_name (col1, col2)      # 插入数据
VALUES (val1, val2);

SELECT * FROM table_name;                # 查询数据
SELECT col1, col2 FROM table_name WHERE condition;

UPDATE table_name                        # 更新数据
SET col1 = val1, col2 = val2
WHERE condition;

DELETE FROM table_name WHERE condition;  # 删除数据
```



## 索引操作

```postgresql
CREATE INDEX index_name                  # 创建索引
ON table_name (column_name);

DROP INDEX index_name;                   # 删除索引
\di                                     # 列出索引
```



## 用户与权限

```postgresql
CREATE USER username WITH PASSWORD 'password';  # 创建用户
ALTER USER username WITH PASSWORD 'newpassword'; # 修改密码
DROP USER username;                      # 删除用户

GRANT privilege ON table_name TO username; # 授权
REVOKE privilege ON table_name FROM username; # 撤销权限

\du                                     # 列出所有用户和角色
```



## 实用命令

```postgresql
\?                                      # 查看所有psql命令帮助
\h                                      # SQL命令帮助
\timing                                 # 切换执行时间显示
\e                                      # 用编辑器编辑当前查询缓冲区
\i filename                             # 执行外部SQL文件
\o filename                             # 将查询结果输出到文件
```
