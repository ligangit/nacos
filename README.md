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

关于oracle驱动，目前是将一个oracle的驱动包，打到了个人本地maven仓库，所以引入的依赖是:
```xml
      <dependency>
        <groupId>com.oracle</groupId>
        <artifactId>ojdbc6</artifactId>
        <version>1.0</version>
      </dependency>
```
后续实际使用时，请切换oracle驱动包。

nacos源码打包：
```shell
mvn -Prelease-nacos -Dmaven.test.skip=true -Dpmd.skip=true -Drat.skip=true -Dcheckstyle.skip=true clean install -U
```
执行完打包命令之后，“nacos-distrubution”模块中，target/nacos-server-2.2.0.zip 文件就是nacos相关的包

项目切换为oracle，参考的博客“https://blog.csdn.net/qq_37279783/article/details/129383362”。
具体的nacos相关操作，请参考nacos官方文档


# 源码启动
1.使用idea 开发工具上，在 nacos-consistency 的 lifecycle 下执行 compile 即可（也可以在命令行中执行 mvn compile，这是官方提供的解决方法）
如果不执行，那么本地源码启动时，会提示“com.alibaba.nacos.consistency.entity.ReadRequest”等不存在
2.执行
```shell
mvn clean package -Dmaven.test.skip=true -Dcheckstyle.skip=true
```
3.需要启动前配置 JVM 参数
-Dnacos.standalone=true

# 源码打包
nacos源码打包：
```shell
mvn -Prelease-nacos -Dmaven.test.skip=true -Dpmd.skip=true -Drat.skip=true -Dcheckstyle.skip=true clean install -U
```
执行完打包命令之后，“nacos-distrubution”模块中，target/nacos-server-2.2.0.zip 文件就是nacos相关的包
