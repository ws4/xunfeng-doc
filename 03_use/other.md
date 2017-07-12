# 巡风之使用小技巧

## 技巧一、如何无需等待直接扫描？

> 
笔者在安装完巡风系统时，迫不期待就想着学习如何使用巡风。但蛋疼的事情来了，巡风没有资产其他功能都用不了。更蛋疼的是巡风在探测资产时，都是到配置里设置好的时间点才会去探测。还好笔者会点python,写了个直接扫描无需等待的py脚本。完美解决~
>

步骤：

1. 在巡风系统nascan目录下，创建一个脚本文件NowNAScan.py


2. 复制以下源码到创建的脚本里

 ```py
 # coding:utf-8
 # author:wolf@YSRC
import thread
from lib.common import *
from lib.start import *
if __name__ == "__main__":
    try:
        CONFIG_INI = get_config()  # 读取配置
        log.write('info', None, 0, u'获取配置成功')
        STATISTICS = get_statistics()  # 读取统计信息
        MASSCAN_AC = [0]
        NACHANGE = [0]
        thread.start_new_thread(monitor, (CONFIG_INI,STATISTICS,NACHANGE))  # 心跳线程
        thread.start_new_thread(cruise, (STATISTICS,MASSCAN_AC))  # 失效记录删除线程
        socket.setdefaulttimeout(int(CONFIG_INI['Timeout']) / 2)  # 设置连接超时

        log.write('info', None, 0, u'扫描规则: ' + str(CONFIG_INI['Cycle']))
      
        NACHANGE[0] = 0
        log.write('info', None, 0, u'开始扫描')
        s = start(CONFIG_INI)
        s.masscan_ac = MASSCAN_AC
        s.statistics = STATISTICS
        s.run()
    except Exception, e:
        print e

 ```

3. 运行脚本NowNAScan.py

 ```bash
root@gvx:~/xunfeng/nascan# python NowNAScan.py 
[20:31:01] 获取配置成功
[20:31:01] 扫描规则: 1|11
[20:31:01] 开始扫描
[20:31:02] 10.0.9.5 active
[20:31:02] 192.168.199.1 active
[20:31:02] 192.168.1.94 active
[20:31:02] 192.168.1.98 active
[20:31:02] 192.168.2.3 active
[20:31:02] 192.168.1.121 active
[20:31:02] 192.168.1.127 active
[20:31:02] 192.168.1.128 active
[20:31:02] 192.168.1.129 active
[20:31:02] 192.168.1.19 active
[20:31:02] 192.168.1.29 active
[20:31:03] 192.168.204.44 active
[20:31:03] 192.168.204.43 active
[20:31:04] 192.168.222.1 active
[20:31:04] 192.168.204.1 active
[20:31:04] 192.168.1.83 active
[20:31:04] 192.168.204.77 active
[20:31:04] 192.168.111.69 active
[20:31:04] 192.168.15.2 active
[20:31:04] 192.168.113.17 active
[20:31:04] 192.168.1.56 active
[20:31:04] 192.168.2.247 active
[20:31:04] 192.168.199.237 active
[20:31:04] 192.168.13.1 active
[20:31:05] 192.168.228.130 active
[20:31:06] 192.168.228.1 active
[20:31:07] 192.168.117.2 active
[20:31:07] 192.168.168.1 active
[20:31:07] 192.168.228.2 active
[20:31:07] 192.168.1.72 active
[20:31:07] 192.168.83.246 active
[20:31:07] 192.168.66.17 active

 Starting masscan 1.0.3 (http://bit.ly/14GZzcT) at 2017-07-12 12:31:12 GMT
  -- forced options: -sS -Pn -n --randomize-hosts -v --send-eth
 Initiating SYN Stealth Scan
 Scanning 31 hosts [65535 ports/host]
 [20:33:13] 192.168.1.121:80 open                                             
 [20:33:43] 192.168.1.121:80 is web
 [20:33:43] 192.168.1.121:80 web info {'tag': [], 'title': 'Not Found'}

 ```

至此，重新去浏览器登录巡风，发现统计栏目已经有我们扫描出的资产，爽歪歪~~~

