---
title: 散漫linux学习笔记
date: 2021-08-23_Mon
status: 成长期
tags: 
- linux
- psql
- postgreSQL
source: 
aliases: []
---

# 散漫LINUX学习笔记
快捷通道：
- [[#1 查看当前目录磁盘使用情况]]
- [[#2 psql基础操作]]
- [[#3 删除重复数据 得到列名]]


### 1.查看当前目录磁盘使用情况
`df -h`
`du -sh *`

- `df`和`du`的区别
Linux df（英文全拼：disk free） 命令用于显示目前在 Linux 系统上的文件系统磁盘使用情况统计。
Linux du （英文全拼：disk usage）命令用于显示目录或文件的大小。
du 会显示指定的目录或文件所占用的磁盘空间。

- /dev/vdal 是什么
在dev目录下可看到vdal文件，文件类型为本地磁盘。通过`df -h`可以看到后面“挂载点”一栏显示该磁盘挂载点为"/", `cd /`移动到该目录即可详细查看占用情况。

- 补充阅读：[df与du不一致情况分析](https://blog.csdn.net/carolzhang8406/article/details/7228248)


### 2. psql基础操作
```
su - postgres
psql 
```

- 查看所有数据库
`\l`
- 切换
`\c <database_name>;`
- 退出
`\q`

- 查看所有数据库大小
`select pg_database.datname, pg_database_size(pg_database.datname) AS size from pg_database;`
- 以MB等形式显示某一数据库大小
`select pg_size_pretty(pg_database_size('<database_name>'));`

- 查看数据库的所有表
`\d`
- 查看表的信息
`\d <table_name>;`
- 查看表的大小
`select pg_relation_size('<table_name>');`
- 以MB等形式查看表的大小
`select pg_size_pretty(pg_relation_size('<table_name>'));`
- 查看表的总大小，包括索引的大小
`select pg_size_pretty(pg_total_relation_size('<table_name>'));`

- 索引与表空间请查看[postgresql查看数据库、表、表空间（位置大小）、索引的方法](https://blog.csdn.net/silenceray/article/details/55198537)


### 3. 删除重复数据&得到列名
`delete from <table_name> where ctid not in (select max(ctid) from <table_name> group by tableColumn);`

注意：
ctid是postgres中的关键字，不可替换；
max代表保存最新插入的数据，若要保留最初的数据，替换为min；
tableColumn是判断重复与否的列名。

- 得到所有表的列名：
`select column_name from information_schema.columns where table_schema='public' and table_name<>'pg_stat_statements';`

- 得到单独一表的列名：
`select column_name from information_schema.columns where table_schema='public' and table_name='<table_name>';`

### 4.权限设置
修改目录的所属组与所有者：
`chown root:root /home/XXX`
修改目录权限：
`chmod 755 /home/XXX`

bin文件的第一个属性用 d 表示。d 在 Linux 中代表该文件是一个目录文件,r 代表可读(read)(4)、 w 代表可写(write)(2)、 x 代表可执行(execute)(1)。排序：(owner/group/others)
（如id_rsa文件权限为`-rw-------`就是600）

[chmod指令](https://www.runoob.com/linux/linux-comm-chmod.html)

### 5.移动
`mv`
-   -f：强制覆盖，如果目标文件已经存在，则不询问，直接强制覆盖；
-   -i：交互移动，如果目标文件已经存在，则询问用户是否覆盖（默认选项）；
-   -n：如果目标文件已经存在，则不会覆盖移动，而且不询问用户；
-   -v：显示文件或目录的移动过程；
-   -u：若目标文件已经存在，但两者相比，源文件更新，则会对目标文件进行升级；


### 6.手动释放缓存
一般情况下不需要手动释放，但若之前进行频繁操作停止后内存大量被占用以至于开始使用SWAP，停止后也没有被释放二十一直存储在缓存区（Cache），可能就需要手动恢复了。
```
free -m  # 查看目前内存情况
sync  # 把内存同步到硬盘
echo 1 > /proc/sys/vm/drop_caches  # 释放页缓存
# echo 0 >/proc/sys/vm/drop_caches  # 释放完内存后改回去让系统重新自动分配内存（我这里不知为何失败了）
free -m  # 再次查看内存情况
```

>drop_caches的值可以是0-3之间的数字，代表不同的含义：  
0：不释放（系统默认值）  
1：释放页缓存  
2：释放dentries和inodes  
3：释放所有缓存

[linux如何手动释放linux内存](https://www.cnblogs.com/yulia/p/9154600.html)


