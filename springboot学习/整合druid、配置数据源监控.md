application.yml中druid的配置：<https://blog.csdn.net/xingkongtianma01/article/details/81624313>

### 导入druid的依赖

​	百度---maven repository---搜索druid-----里面有相关的依赖

```xml
<!-- https://mvnrepository.com/artifact/com.alibaba/druid -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.20</version>
</dependency>
<!--引入log4j，durid配置里面filters有log4j,所以要加这个依赖-->
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>

```

### 在application.yml配置

~~~yml




spring:
  thymeleaf:
    cache: false   #禁用模板缓存
  datasource:
    name: answer     #数据库名
    url: jdbc:mysql://localhost:3306/answer?useUnicode=true&useJDBCCompliantTimezoneShift=true&serverTimezone=UTC&characterEncoding=utf8 #url
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver  #数据库驱动
    type: com.alibaba.druid.pool.DruidDataSource   #选择自定义的druid数据源
    filters: stat
    druid:
      #连接池的配置信息
      # 初始化大小，最小，最大
      initialSize: 5
      minIdle: 5
      maxActive: 20
      # 配置获取连接等待超时的时间
      maxWait: 60000
      # 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒
      timeBetweenEvictionRunsMillis: 60000
      # 配置一个连接在池中最小生存的时间，单位是毫秒
      minEvictableIdleTimeMillis: 300000
      validationQuery: SELECT 1 FROM DUAL
      testWhileIdle: true
      testOnBorrow: false
      testOnReturn: false
      # 打开PSCache，并且指定每个连接上PSCache的大小
      poolPreparedStatements: true
      maxPoolPreparedStatementPerConnectionSize: 20
      # 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
      #filters: stat,wall,log4j
      # 通过connectProperties属性来打开mergeSql功能；慢SQL记录
      connectionProperties: druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000
      # 合并多个DruidDataSource的监控数据
      useGlobalDataSourceStat: true


#mybatis:
#  mapper-locations: classpath:mapper/*.xml
#  type-aliases-package: com.hc.demo.bean
mybatis:
   type-aliases-package: com.hc.demo.bean
   #config-location:  classpath:mybatis/mybatis-config.xml  #指定全局配置文件的位置
   mapper-locations:  classpath:mybatis/mapper/*.xml  #指定sql映射文件的位置




~~~

### 建立一个DruidConfig.class

~~~java
@Configuration
public class DruidConfig {

    @ConfigurationProperties(prefix = "spring.datasource") //绑定spring.datasource下的所有属性
    @Bean
    public DataSource druid(){
        return new DruidDataSource();
    }

    // 配置Druid的监控
    // 1、配置一个管理后台的Servlet
    @Bean
    public ServletRegistrationBean statViewServlet() {
        ServletRegistrationBean bean = new ServletRegistrationBean(new StatViewServlet(), "/druid/*");
        Map<String, String> initParams = new HashMap<>();

        initParams.put("loginUsername", "admin");
        initParams.put("loginPassword", "123");
        initParams.put("allow", "");// 默认就是允许所有访问
        initParams.put("deny", "10.18.172.124");

        bean.setInitParameters(initParams);
        return bean;
    }

    // 2、配置一个web监控的filter
    @Bean
    public FilterRegistrationBean webStatFilter() {
        FilterRegistrationBean bean = new FilterRegistrationBean();
        bean.setFilter(new WebStatFilter());

        Map<String, String> initParams = new HashMap<>();
        initParams.put("exclusions", "*.js,*.css,/druid/*");

        bean.setInitParameters(initParams);

        bean.setUrlPatterns(Arrays.asList("/*"));

        return bean;
    }
}
~~~



### 途中遇到的错误

程序无法启动

解决办法：把默认的jre修改成自己下载的

![1576459494120](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576459494120.png)

![1576459510602](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576459510602.png)



用户名：admin

密码:123

自己已经在 statViewServlet 配置的用户名、密码

![1576460535828](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576460535828.png)

![1576460867534](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576460867534.png)



驼峰命名法：

![1576466241347](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1576466241347.png)

将url、username、password等文件放在druid下面，这样与mybatis整合时会找不到url。

~~~yml
 name: answer     #数据库名
    url: jdbc:mysql://localhost:3306/answer?useUnicode=true&useJDBCCompliantTimezoneShift=true&serverTimezone=UTC&characterEncoding=utf8 #url
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver  #数据库驱动
    type: com.alibaba.druid.pool.DruidDataSource   #选择自定义的druid数据源
    druid:
    	....
~~~



druid的SQL监控没有数据：

~~~yml

 # 配置监控统计拦截的filters，去掉后监控界面sql无法统计，'wall'用于防火墙
      #filters: stat,wall,log4j
  #将上面的那句话，放在spring.datasource下
  spring:
  	datasource:
  		 filters: stat,wall,log4j
 
~~~

