# 简介
当前项目是使用的nacos-2.2.0版本源码改造，切换为oracle数据库。
其中“nacos-config”模块中，src/main/resources/META-INF/nacos-oracle.sql文件为oracle的初始化sql。
修改的配置项：主要:
```properties
spring.sql.init.platform=oracle
### Count of DB:
db.num=1

### Connect URL of DB:
db.url.0=jdbc:oracle:thin:@192.168.1.200:1521:ORCL
db.user.0=nacos
db.password.0=nacos
db.jdbcDriverName=oracle.jdbc.OracleDriver
db.testQuery=select 1 from dual
```
项目切换为oracle，参考的博客“https://blog.csdn.net/qq_37279783/article/details/129383362”。
具体的nacos相关操作，请参考nacos官方文档