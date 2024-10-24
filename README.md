# 简介
分支说明：
master：nacos官网源码的分支
nacos-2.2.0：nacos官方2.2.0版本的源码分支
nacos-2.2.0-oracle：是本仓库中根据2.2.0版本源码改造，适配oracle数据库处理的
nacos-2.3.2：nacos官方2.3.2版本的源码分支
nacos-2.3.2-oracle：是本仓库中根据2.3.2版本源码改造，适配oracle数据库处理的，注意oracle的数据库执行脚本不一样


在nacos-2.3.2-oracle分支版本中：
其中“nacos-config”模块中，src/main/resources/META-INF/nacos-oracle.sql文件为oracle的初始化sql。
修改的配置项：主要:
```properties
spring.sql.init.platform=oracle
### Count of DB:
db.num=1

### Connect URL of DB:
db.url.0=jdbc:oracle:thin:@192.168.1.200:1521:ORCL
db.user.0=naocs232
db.password.0=naocs232
db.jdbcDriverName=oracle.jdbc.OracleDriver
db.testQuery=select 1 from dual

# 需要开启权限认证，2.2.3版本开始默认是没有权限认证的
nacos.core.auth.enabled=true
nacos.core.auth.server.identity.key=serverAuthKey
nacos.core.auth.server.identity.value=server@v2.2.0AtuhValue
nacos.core.auth.plugin.nacos.token.secret.key=这是配置的秘钥，需要自己生成
```
生成秘钥可以用这个方式：
```java

    public static void main(String[] args) {
        String bac = RandomUtil.randomString(64);
        System.out.println(bac);
        String secretKey = Base64.getEncoder().encodeToString(bac.getBytes(StandardCharsets.UTF_8));
        System.out.println("nacos.core.auth.plugin.nacos.token.secret.key: " + secretKey);
    }
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
执行完打包命令之后，“nacos-distrubution”模块中，target/nacos-server-2.3.2.zip 文件就是nacos相关的包 

2.2.0版本切换为oracle，参考的博客“https://blog.csdn.net/qq_37279783/article/details/129383362”。 

2.3.2版本切换，参考的博客“https://blog.csdn.net/Retsyo_/article/details/138312421” 

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
执行完打包命令之后，“nacos-distrubution”模块中，target/nacos-server-2.3.2.zip 文件就是nacos相关的包
