# 在linux上安装巡风扫描器

在一下Linux发行版本上部署巡风
 
* Ubuntu 16.04 Server 或 16.10
* Debian 8.x 
* Kali Linux 2016.2

## 1.系统设置
时区

```bash
cp -f /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

同步时间

```bash
apt-get install ntpdate
ntpdate cn.pool.ntp.org
```

安装依赖

```bash
apt-get install git gcc libssl-dev libffi-dev python-dev libpcap-dev
```

下载巡风

```bash
git clone https://code.aliyun.com/ysrc/xunfeng.git
```

安装 pip

```bash
apt-get install python-pip
```

更新 pip

```bash
pip install -U pip 
```

使用pip安装 python 依赖库, 这里使用了豆瓣的 pypi 源。

```bash
cd xunfeng
pip install -r requirements.txt -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

Debian 运行

```bash
pip install -r requirements.txt -i http://pypi.douban.com/simple/
pip install markupsafe
```

## 2.安装MongoDB数据库
设置源 Ubuntu

```bash
echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list  
Debian

echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/3.4 main" | tee /etc/apt/sources.list.d/mongodb-org-3.4.list
```

Kali 直接安装

```bash
apt-get install mongodb
service mongodb start
```

添加秘钥

```bash
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv A15703C6
```

安装 MongoDB

```bash
apt-get update
apt-get install -y mongodb-org --allow-unauthenticated
```

启动数据库

```bash
service mongod start
```

添加认证

```bash
mongo
> use xunfeng     \\切换数据库xunfeng
> db.createUser({user:'scan',pwd:'xxxxxx',roles:[{role:'dbOwner',db:'xunfeng'}]})     \\添加用户 pwd 字段可以更改为你需要的密码
> exit     \\退出
```

导入数据库

```bash
mongorestore -h 127.0.0.1 --port 27017 -d xunfeng /root/xunfeng/db/
```

## 3.启动巡风
编辑 系统数据库配置脚本 Config.py

```bash
cd xunfeng/
vim Config.py
```

修改 PASSWORD 和 DBPASSWORD 字段内的密码，以及PORT=65521改为27017

```py
class Config(object):
    ACCOUNT = 'admin'
    PASSWORD = 'xxxxxx'     \\web登录密码
class ProductionConfig(Config):
    DB = '127.0.0.1'
    PORT = 65521
    DBUSERNAME = 'scan'
    DBPASSWORD = 'xxxxxx'      \\前面设置的数据库用户密码
    DBNAME = 'xunfeng'
```

将Run.sh中--port 65521端口改为27017

```bash
#!/bin/bash
CURRENT_PATH=`dirname $0`
cd $CURRENT_PATH

XUNFENG_LOG=/var/log/xunfeng
XUNFENG_DB=/var/lib/mongodb

[ ! -d $XUNFENG_LOG ] && mkdir -p ${XUNFENG_LOG}
[ ! -d $XUNFENG_DB ] && mkdir -p ${XUNFENG_DB}

nohup mongod --port 65521 --dbpath=${XUNFENG_DB} --auth  > ${XUNFENG_LOG}/db.log &
nohup python ./Run.py > ${XUNFENG_LOG}/web.log &
nohup python ./aider/Aider.py > ${XUNFENG_LOG}/aider.log &
nohup python ./nascan/NAScan.py > ${XUNFENG_LOG}/scan.log &
nohup python ./vulscan/VulScan.py > ${XUNFENG_LOG}/vul.log &
```

启动

```bash
service mongod stop
sh Run.sh
```
>
注意：如果启动不成功，请按一下步骤检查
1.查看80,8088,27017端口是否都打开了
2.查看/var/log/xunfeng/下的log,找出错误
3.查看/var/lib/mongodb/下数据库的日志
4.数据库的配置文件路径：/etc/mongodb.conf
>

kali 运行

```bash
service mongodb stop
sh Run.sh
```

## 4.安装Masscan

```bash
git clone https://github.com/robertdavidgraham/masscan
cd masscan
make
```

登录巡风 修改 启用 MASSCAN 点上右边的蓝点 /root/masscan/bin/masscan

> 
参考文章：
《Ubuntu Debian Kali 部署 巡风》 https://www.yagami.info/ubuntu-debian-kali-bu-shu-xun-feng/
>


