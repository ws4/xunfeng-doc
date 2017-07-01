# 巡风之搜索功能

查询语法：

```
1. 按端口： port:端口号 eg. port:22
2. 按banner： banner:banner内容关键词 eg. banner:ftp
3. 按ip(支持c段，b段模糊查询)： ip:ip地址 eg. ip:192.168.1.1／ip:192.168.1.
4. 按服务名： server:服务名 eg. server:iis
5. 按标题： title:标题内容关键词 eg. title:xxx管理系统
6. 按服务类型标签： tag:服务类型 eg. tag:apache
7. 按主机名： hostname:主机名 eg. hostname:server001
8. 全局模糊： all:查询内容 eg. all:tongcheng
9. 多条件： 条件1:内容1;条件2:内容2 eg. ip:192.168.1.1;port:22
```

步骤：

```
1. 在搜索框中输入符合查询语法的查询语句
2. 点击搜索
3. 获得查询结果
```