# 使用流程
经测试巡风系统在搭建好后，访问时使用IE和chorme内核的浏览器显示有问题，推荐使用firefox浏览器


## 启动巡风

```bash
sh Run.sh
```

## 关闭巡风
停止服务

```bash
kill -9 $(pid of mongod)
```

或

```bash
killall mongod
killall python
```