title: Oracle遇到的问题
date: 2015-10-26 20:40:30
tags: oracle
---

## 一.  ORA-27102: out of memory

<!-- more -->

![](http://7xire1.com1.z0.glb.clouddn.com/QQ截图20151026130452.png)

### 在/etc/security/limits.conf最后添加，oracle memlock

```
oracle soft nproc 2047
oracle hard nproc 16384
oracle soft nofile 1024
oracle hard nofile 65536
oracle soft memlock 10485760
oracle hard memlock 10485760
```

### 要是/etc/security/limits.conf生效，必须要要使相应的进程重新启动

```
SQL> shutdown immediate 
SQL> startup
```
### 查看

```
ulimit -a
```

### 参考:

[http://www.databaseskill.com/1834993/](http://www.databaseskill.com/1834993/)

[http://jingyan.baidu.com/article/86fae346870c503c49121a96.html](http://jingyan.baidu.com/article/86fae346870c503c49121a96.html)
