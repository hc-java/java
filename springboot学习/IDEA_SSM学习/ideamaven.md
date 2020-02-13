[TOC]







# SSM笔记

## IDEA创建一个maven Web项目

​	所需软件：IDEA旗舰版+apache-maven-3.6.0+Tomcat8.5（下载请自行百度） 另外IDEA还要配置maven 和 tomcat

​	参考：（必须先看：）<https://blog.csdn.net/czc9309/article/details/80304074>

​			该视频40：00；<https://www.bilibili.com/video/av29071483?from=search&seid=2557781280223897540>

​	以下是需要修改的地方：

​	1. maven需要修改的东西：![1554284690369](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1554284690369.png)

~~~
添加本地仓库：
<localRepository>E：\reposi</localRepository>

添加镜像：
	<mirror>
      <id>nexus-aliyun</id>
      <mirrorOf>*</mirrorOf>
      <name>Nexus aliyun</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public</url>
    </mirror>



~~~

~~~ 
2. 在maven里面的pom.xml中导入依赖包：
~~~

```
<dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.0.1</version>
      <scope>provided</scope>
</dependency>

<!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>5.1.38</version>
</dependency>

```

## 添加依赖

<https://mvnrepository.com/>

## 下载插件



![1554727240688](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1554727240688.png)

![1554726492789](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1554726492789.png)

## IDEA新建mybatis-config.xml配置文件

<https://blog.csdn.net/Doctor_LY/article/details/82986230>



## idea中mapper.xml文件中id=“方法名”报红色错误

https://www.imooc.com/wenda/detail/390195