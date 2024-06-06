# 安装
1. 解压zip
2. 配置本地仓库|
   在conf文件夹下找到settings.xml，找到下面标签的位置，添加
```
<localRepository>F:\java\maven\apache-maven-3.9.6\mvn_repo</localRepository>
```
3. 配置阿里云私服
    ```
     <mirror>
	    <id>alimaven</id>
	    <name>aliyun maven</name>
	    <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	    <mirrorOf>central</mirrorOf>       
	  </mirror>
	```
4. 配置环境变量
配置MAVEN_HOME及bin目录


# 创建maven项目
1. 单个项目配置
2. 全局配置 all settings

