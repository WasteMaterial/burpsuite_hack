# burpsuite_hack
一款代理扫描器

- 适配python3.6+ 

- 支持 GET/POST/无限嵌套json、list的漏洞探测

- 扫描请求去重

- 基本不会触发WAF，最小化探测

详细请见：https://www.cnblogs.com/depycode/p/17079397.html
# 整体架构
![image](https://github.com/depycode/burpsuite_hack/blob/master/p2.png)

# 使用方法
- burpsuite 插件加载：BurpExtender_ALL_UI.py ，修改socks host、port 为扫描端对应的ip和端口，然后点击set
![image](https://github.com/depycode/burpsuite_hack/blob/master/p1.png)
![image](https://github.com/depycode/burpsuite_hack/blob/master/p4.png)

- 扫描端启动
```
nohup python3 MyUDPHandler_Threads.py &
```

# 创建数据库

```
+--------------------+
| Tables_in_burphack |
+--------------------+
| sql_bool           |
| sql_error          |
| ssrf               |
+--------------------+
```

```
+----------+-------------------------------------------------------------------+
| Database | Create Database                                                   |
+----------+-------------------------------------------------------------------+
| burphack | CREATE DATABASE `burphack` /*!40100 DEFAULT CHARACTER SET utf8 */ |
+----------+-------------------------------------------------------------------+
```

```
+----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table    | Create Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
+----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sql_bool | CREATE TABLE `sql_bool` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `host` varchar(255) NOT NULL,
  `risk` int(11) NOT NULL,
  `bool_true_resp` mediumtext NOT NULL,
  `bool_true_req` mediumtext NOT NULL,
  `bool_false_resp` mediumtext,
  `bool_false_req` mediumtext,
  `first_resp` mediumtext NOT NULL,
  `payload` varchar(255) NOT NULL,
  `first_req` mediumtext NOT NULL,
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 COMMENT='bool型sql注入'    |
+----------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```
+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table     | Create Table                                                                                                                                                                                                                                                                                                                              |
+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| sql_error | CREATE TABLE `sql_error` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `request_data` mediumtext,
  `response` mediumtext,
  `host` varchar(255) DEFAULT NULL,
  `dbms` varchar(255) DEFAULT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 |
+-----------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

```
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| ssrf  | CREATE TABLE `ssrf` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `payload` varchar(255) DEFAULT NULL,
  `request_data` mediumtext,
  `response` mediumtext,
  `host` varchar(255) DEFAULT NULL,
  `is_vul` int(11) DEFAULT '0' COMMENT '0 默认值\n1 存在漏洞',
  `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP,
  `vuType` int(11) DEFAULT NULL COMMENT '1  ssrf\n2  rce',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=16625 DEFAULT CHARSET=utf8 COMMENT='历史ssrf探测请求'              |
+-------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```
python包：
```
my python version 3.12
python3 -m pip install --upgrade pip
pip install -r requirements.txt
pip install pymysql scikit-learn colorlog watchdog
```

# 实战成果
- TSRC

![image](https://github.com/depycode/burpsuite_hack/blob/master/p3.png)

# 参考
- https://github.com/w-digital-scanner/w13scan
