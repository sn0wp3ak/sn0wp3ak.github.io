---
layout: post
title: MyISAM和InnoDB的区别
date: 2019-12-14
categories:
- MySQL
---
MyISAM: 基于ISAM;<br>
ISAM--Indexed Sequential Access Method 有索引的顺序访问方法<br>
不支持外键, 不是事务安全的, 适合执行大量的 select 和 insert 操作;<br>
InnoDB: 支持事务安全, 支持外键, 支持行锁、事务, 适合大量的 update和insert 操作, 适合高并发和QPS较高的情况;<br>
QPS: 每秒查询率, 高吞吐<br>
