#文件结构
```
│  Config.py  # 配置文件
│  README.md  # 说明文档
│  Run.bat  # Windows启动服务
│  Run.py  # webserver
│  Run.sh    # Linux启动服务，重新启动前需把进程先结束掉
│  
├─aider
│      Aider.py  # 辅助验证脚本
│      
├─db  # 初始数据库结构
│      
├─masscan  # 内置编译好的Masscan程序，若无法使用请自行编译安装
│          
├─nascan
│  │  NAScan.py # 网络资产信息抓取引擎
│  │  
│  ├─lib
│  │      common.py 其他方法
│  │      icmp.py  # ICMP发送类
│  │      log.py  # 日志输出
│  │      mongo.py  # 数据库连接
│  │      scan.py  # 扫描与识别
│  │      start.py  # 线程控制
│  │      
│  └─plugin
│          masscan.py  # 调用Masscan脚本
│          
├─views
│  │  View.py  # web请求处理
│  │  
│  ├─lib
│  │      Conn.py  # 数据库公共类
│  │      CreateExcel.py  # 表格处理
│  │      Login.py  # 权限验证
│  │      QueryLogic.py  # 查询语句解析
│  │      
│  ├─static #静态资源目录
│  │              
│  └─templates #模板文件目录
│          
└─vulscan
    │  VulScan.py  # 漏洞检测引擎
    │  
    └─vuldb # 漏洞库目录
```
