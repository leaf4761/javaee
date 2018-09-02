# maven 简介
Maven是一个项目管理和综合工具。Maven提供了开发人员构建一个完整的生命周期框架。开发团队可以自动完成项目的基础工具建设，Maven使用标准的目录结构和默认构建生命周期。

Maven主要目标是提供给开发人员： 
1. 项目是可重复使用，易维护，更容易理解的一个综合模型。 
2. 插件或交互的工具，这种声明性的模式。 

Maven项目的结构和内容在一个XML文件中声明，pom.xml 项目对象模型（POM），这是整个Maven系统的基本单元。

# maven 安装
1. 确保系统中已安装jdk，并配置好环境变量
2. 进入[maven](maven.apache.org/download.cgi "maven官网")官网下载maven，并解压到指定目录
3. 添加 M2\_HOME 和 MAVEN\_HOME 环境变量到 Windows 环境变量，并将其指向你的 Maven 文件夹。
4. 更新 PATH 变量，添加 Maven bin 文件夹到 PATH 的最后，如： %M2_HOME%\bin, 这样就可以在命令中的任何目录下运行 Maven 命令了。 
5. 执行 mvn –version 进行验证

# maven 配置文件
maven的配置文件在 {M2_HOME}/conf/settings.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  
  <localRepository>C:/java/.m2/repository</localRepository>

  <!-- Default: true
  <interactiveMode>true</interactiveMode>
  -->

  <!-- Default: false
  <offline>false</offline>
  -->

  <pluginGroups>
    <!-- 
    <pluginGroup>com.your.plugins</pluginGroup>
    -->
  </pluginGroups>

  <proxies>
    <!-- 
    <proxy>
      <id>optional</id>
      <active>true</active>
      <protocol>http</protocol>
      <username>proxyuser</username>
      <password>proxypass</password>
      <host>proxy.host.net</host>
      <port>80</port>
      <nonProxyHosts>local.net|some.host.com</nonProxyHosts>
    </proxy>
    -->
  </proxies>

  <servers>
    <!-- 
    <server>
      <id>deploymentRepo</id>
      <username>repouser</username>
      <password>repopwd</password>
    </server>
    -->

    <!--
    <server>
      <id>siteServer</id>
      <privateKey>/path/to/private/key</privateKey>
      <passphrase>optional; leave empty if not used.</passphrase>
    </server>
    -->
  </servers>

  <mirrors>
    <!--
    <mirror>
      <id>mirrorId</id>
      <mirrorOf>repositoryId</mirrorOf>
      <name>Human Readable Name for this Mirror.</name>
      <url>http://my.repository.com/repo/path</url>
    </mirror>
     -->
  </mirrors>

  <profiles>
    <!-- 
    <profile>
      <id>jdk-1.4</id>

      <activation>
        <jdk>1.4</jdk>
      </activation>

      <repositories>
        <repository>
          <id>jdk14</id>
          <name>Repository for JDK 1.4 builds</name>
          <url>http://www.myhost.com/maven/jdk14</url>
          <layout>default</layout>
          <snapshotPolicy>always</snapshotPolicy>
        </repository>
      </repositories>
    </profile>
    -->

    <!--
     | <plugin>
     |   <groupId>org.myco.myplugins</groupId>
     |   <artifactId>myplugin</artifactId>
     |
     |   <configuration>
     |     <tomcatLocation>${tomcatPath}</tomcatLocation>
     |   </configuration>
     | </plugin>
    <profile>
      <id>env-dev</id>

      <activation>
        <property>
          <name>target-env</name>
          <value>dev</value>
        </property>
      </activation>

      <properties>
        <tomcatPath>/path/to/tomcat/instance</tomcatPath>
      </properties>
    </profile>
    -->
  </profiles>

  <!-- 
  <activeProfiles>
    <activeProfile>alwaysActiveProfile</activeProfile>
    <activeProfile>anotherAlwaysActiveProfile</activeProfile>
  </activeProfiles>
  -->
</settings>
```
### 代理服务器
如果公司正在建立一个防火墙，并使用HTTP代理服务器来阻止用户直接连接到互联网。如果使用代理，Maven将无法下载任何依赖。 

为了使它工作，必须声明在 Maven 的配置文件中设置代理服务器：

    去掉proxies的注释，填写代理服务器的详细信息，即可配置好maven代理服务器。

### maven本地仓库
Maven的本地资源库是用来存储所有项目的依赖关系(插件jar和其他文件，这些文件被Maven下载)到本地文件夹。很简单，当建立一个Maven项目，所有相关文件将被存储在Maven本地仓库。 

默认情况下，Maven的本地资源库默认为 .m2 目录文件夹： 
    
    Unix/Mac OS X – ~/.m2 
    Windows – C:\Documents and Settings\{username}\.m2 

更改Maven的本地库：

      <localRepository>C:/java/.m2/repository</localRepository>

### maven中央仓库
当建立一个 Maven 的项目，Maven 会检查你的 pom.xml 文件，以确定哪些依赖下载。首先，Maven 将从本地资源库获得 Maven 的本地资源库依赖资源，如果没有找到，然后把它会从默认的[Maven中央存储库](https://search.maven.org/ "maven中央仓库")查找下载。 

在Maven中，当声明的库不存在于本地存储库中，也不存在于Maven中心储存库，该过程将停止并将错误消息输出到 Maven 控制台

可以添加远程仓库，修改settings.xml：

    <profiles>  
        <id>dev</id> 
        <repositories>
            <repository>
                <id>maven-net-cn</id>  
                <name>Maven China Mirror</name>  
                <url>http://maven.net.cn/content/groups/public/</url>
            </repository>
        </repositories>
    </profiles>  
    <activeProfiles>  
        <activeProfile>dev</activeProfile>  
    </activeProfiles>  

我们定义一个id为dev的profile，将所有repositories以及pluginRepositories元素放到这个profile中，然后，使用、、\<activeProfiles\>元素自动激活该profile。这样，就不用再为每个POM重复配置仓库。

使用profile为settings.xml添加仓库提供了一种用户全局范围的仓库配置。

至此，Maven的依赖库查询顺序更改为： 
1. 在 Maven 本地资源库中搜索，如果没有找到，进入第 2 步，否则退出。 
2. 在 Maven 中央存储库搜索，如果没有找到，进入第 3 步，否则退出。 
3. 在java.net Maven的远程存储库搜索，如果没有找到，提示错误信息，否则退出。 

### 镜像
镜像（mirror）为Maven提供的一个映射机制，来实现远程仓库的切换，使用时要配置该镜像的拦截目标对象，可以是一个或多个，*为全部拦截，被拦截的请求，会从之前的仓库切到该镜像中的仓库地址进行查找获取文件，从而达到切换远程仓库的目的。

    <mirrors>
        <mirror>
            <!--该镜像的id-->
            <id>nexus-aliyun</id>
            <!--该镜像用来取代的远程仓库，central是中央仓库的id-->
            <mirrorOf>central</mirrorOf>
            <name>Nexus aliyun</name>
            <!--该镜像的仓库地址，这里是用的阿里的仓库-->
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>
    </mirrors>

参考：<https://www.yiibai.com/maven/>