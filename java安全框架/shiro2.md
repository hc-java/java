官网：<http://shiro.apache.org/>

mvn:查找想要的依赖   <https://mvnrepository.com/>



只参考了一部分，剩下的自己到处百度

千峰教育：<https://www.bilibili.com/video/av83878636?from=search&seid=11935439949256609996>

shiro框架详解：<https://blog.csdn.net/pengjwhx/article/details/84867112>

# shiro功能描述

主要功能：认证(Authentication)    授权(Authorization)   加密(Cryptography)   会话管理(Session Manager)

![1583983565134](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583983565134.png)



**Authentication**：身份认证/登录，验证用户是不是拥有相应的身份；
**Authorization**：授权，即权限验证，验证某个已认证的用户是否拥有某个权限；即判断用
户是否能做事情，常见的如：验证某个用户是否拥有某个角色。或者细粒度的验证某个用
户对某个资源是否具有某个权限；
**Session Manager**：会话管理，即用户登录后就是一次会话，在没有退出之前，它的所有信
息都在会话中；会话可以是普通 JavaSE 环境的，也可以是如 Web 环境的；
**Cryptography**：加密，保护数据的安全性，如密码加密存储到数据库，而不是明文存储；
**Web Support**：Web 支持，可以非常容易的集成到 Web 环境；
**Caching**：缓存，比如用户登录后，其用户信息、拥有的角色/权限不必每次去查，这样可以提高效率；
**Concurrency**：shiro 支持多线程应用的并发验证，即如在一个线程中开启另一个线程，能
把权限自动传播过去；
**Testing**：提供测试支持；
**Run As**：允许一个用户假装为另一个用户（如果他们允许）的身份进行访问；
**Remember Me**：记住我，这个是非常常见的功能，即一次登录后，下次再来的话不用登录
记住一点，Shiro 不会去维护用户、维护权限；这些需要我们自己去设计/提供；然后通过
相应的接口注入给 Shiro 即可。
了

# shiro原理理解

![1583984009211](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583984009211.png)

核心组件：Subject SecurityManager Realm



应用代码通过 Subject 来进行认证和授权，而 Subject 又委托给 SecurityManager； 

我们需要给 Shiro 的 SecurityManager 注入 Realm，从而让 SecurityManager 能得到合法的用户及其权限进行判断。

# 流程图

**shiro认证功能（Authentication）流程**

![1583984242954](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583984242954.png)



![1583984410558](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583984410558.png)





![1583936002836](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583936002836.png)





![1583936027691](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583936027691.png)

# shiro入门

## pom.xml

~~~xml

 <!-- shiro依赖 -->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-core</artifactId>
            <version>1.4.0</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.12</version>
        </dependency>
        <!-- 警告：Class path contains multiple SLF4J bindings. -->
   <!--     <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.12</version>
        </dependency>-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.16</version>
        </dependency>
        <dependency>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
            <version>1.2</version>
        </dependency>
        <!--使用这个依赖的原因： https://blog.csdn.net/wohaqiyi/article/details/81009689-->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.8</version>
            <!--排除这个slf4j-log4j12-->
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
~~~



## shiro.ini

~~~ini
#用户
[users]
zhangsan=123,admin
lisi=456,manager,seller
wangwu=789,clerk

#权限
[roles]
admin=*
clerk=user:query,user:detail:query
manager=user:*
~~~



## TestShiro.java

~~~java
public class TestShiro {
    public static void main(String[] args) {
        //创建SecurityFactory ，加载ini配置，并通过它创建SecurityManager
        Factory<SecurityManager> factory = new IniSecurityManagerFactory("classpath:shiro.ini");

        //shiro核心：SecurityManager
        //import org.apache.shiro.mgt.SecurityManager; 不导入这个包 会报错
        SecurityManager securityManager=factory.getInstance();

        //将SecurityManager 托管到 SecurityUtils工具类中（之后不必关心Security）
        SecurityUtils.setSecurityManager(securityManager);

        //Subject作用：直接由用户使用，调用功能简单，其底层调用SecurityManager的相关流程
        Subject subject=SecurityUtils.getSubject();

        //通过subject获取当前用户的登录状态（从session中同步信息）
        System.out.println(subject.isAuthenticated());
        System.out.println("principal:" + subject.getPrincipal());  //subject.getPrincipal() 获取你存储的principal

        //通过 subject 身份认证
        UsernamePasswordToken token = new UsernamePasswordToken("zhangsan", "123");

        subject.login(token);//登陆过程

        System.out.println("principal:" + subject.getPrincipal());
        System.out.println(subject.isAuthenticated());

        //角色校验，判断是不是 admin 角色
        boolean isAdmin=subject.hasRole("admin");
        System.out.println(isAdmin);

        //权限校验，判断是不是 a资源：b操作
        System.out.println(subject.isPermitted("a:b"));

        //登出：抹除全部信息（身份信息登陆状态信息、权限信息、角色信息、会话信息）
        subject.logout();


    }
}

~~~

## 总结

- UnknownAccountException 用户名不存在
- IncorrectCredentialsException  密码错误



![1583942558462](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583942558462.png)

![1583943100583](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583943100583.png)







# 表结构

到后面 加密模块 又会稍微修改一下表

用户表(t_user)、角色表(t_role)、权限表(t_permission)、用户-角色关联表(t_ser_role)、角色-权限关联表(t_role_permission)

![1584181065186](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584181065186.png)



![1584181105780](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584181105780.png)



![1584181142946](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584181142946.png)



![1584181166976](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584181166976.png)



![1584181220727](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584181220727.png)



注意：用户-角色关联表(t_ser_role)的 user_id 是用户表(t_user)中 id 的外键；

​	  用户-角色关联表(t_ser_role)的 role_id 是角色表(t_role)中 id 的外键；

​	 角色-权限关联表(t_role_permission)的 permission_id 是权限表(t_role) id 的外键；

 	角色-权限关联表(t_role_permission)的 role_id 是角色表(t_role)中 id 的外键；

外键相关信息在  数据库/mysql.md 文件中

# pom.xml

~~~xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.2.5.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>demo</name>
    <description>Demo project for Spring Boot</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>


        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.41</version>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
        <!--修改html，即使生效-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
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
        <!--使用日志-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
            <version>2.2.1.RELEASE</version>
            <scope>compile</scope>
        </dependency>

        <!--通用Mapper-->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper</artifactId>
            <version>4.0.3</version>
        </dependency>
        <!--mybatis-generator-->
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.7</version>
        </dependency>

        <!--gson-->
        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.6</version>
            <!--<scope>compile</scope>-->
        </dependency>

        <!--分页-->
        <!--导入pagehelper相关依赖-->
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper</artifactId>
            <version>5.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-autoconfigure</artifactId>
            <version>1.2.3</version>
        </dependency>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!--AOP-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>

        <!--shiro-->
        <dependency>
            <groupId>org.apache.shiro</groupId>
            <artifactId>shiro-spring</artifactId>
            <version>1.4.0</version>
        </dependency>
        <!--thymeleaf 整合 shiro 标签-->
        <dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.0.0</version>
        </dependency>




    </dependencies>

    <build>
        <plugins>
            <!--热部署插件-->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>

            </plugin>

            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.7</version>
                <configuration>
                    <overwrite>true</overwrite>
                </configuration>
                <dependencies>
                    <dependency>
                        <groupId>com.chrm</groupId>
                        <artifactId>mybatis-generator-lombok-plugin</artifactId>
                        <version>1.0-SNAPSHOT</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </build>

</project>

~~~





# 用户认证

原文链接：https://blog.csdn.net/zhaolulu916/article/details/95766461

设计login页面——>ShiroConfig（login放行）——>login.html(提交login的controller）-->Controller（subject.login(token)）->UserRealm进行mybtis数据库认证——>认证结果返回给Controller——>login.html页面显示结果

## ShiroConfig.java

~~~java

@Configuration
public class ShiroConfig {
    /**
     * 创建ShiroFilterFactoryBean
     *
     * anon：无需认证（登录）可以访问
     * authc：必须认证才可以访问
     * user：如果使用RememberMe的功能可以直接访问
     * perms：该资源必须得到资源权限才可以访问
     * role：该资源必须得到角色权限才可以访问
     */
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(@Qualifier("defaultWebSecurityManager") DefaultWebSecurityManager defaultWebSecurityManager){
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        //设置安全管理器
        shiroFilterFactoryBean.setSecurityManager(defaultWebSecurityManager);


        Map<String, String> filterMap = new LinkedHashMap<String, String>();
        //login 无需认证就可以访问
        filterMap.put("/", "anon");
        filterMap.put("/login","anon");

       //授权过滤器
        //注意：当前授权拦截后，shiro会自动跳转到未授权页面
        filterMap.put("/update","perms[student:yq]");  //加了这句，才会执行用户授权方法；如果 perm[] 里面的权限与数据库中不同，则跳转到未授权页面



       //使用通配符，让所有的页面都必须认证才可以访问
        filterMap.put("/**","authc");


        //修改调整的登录页面,不然 默认跳到 xx.jsp 页面
        shiroFilterFactoryBean.setLoginUrl("login");
        //设置未授权提示页面
        shiroFilterFactoryBean.setUnauthorizedUrl("/noAuth");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterMap);

        return shiroFilterFactoryBean;
    }
    /**
     * 创建DefaultWebSecurityManager安全管理器
     */
    @Bean(name = "defaultWebSecurityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("MyRealm") MyRealm myRealm){
        DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager();
        //关联Reaml
        defaultWebSecurityManager.setRealm(myRealm);
        return defaultWebSecurityManager;
    }
    /**
     * 创建Realm
     */
    @Bean(name = "MyRealm")
    public MyRealm getReaml(){
        return new MyRealm();
    }

    /*
    * 配置 ShiroDialect ,用于thymeleaf 和 shiro 标签配合使用
    * */
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
}

~~~

## login.html

~~~html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>login</title>
</head>
<body>

<h3 th:text="${msg}" style="color: red"></h3>
<form action="login" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit" value="登录">

</form>
</body>
</html>
~~~



## Controller

~~~java
@Controller
public class TUserController {
    /* @Autowired
     private TUserMapper tUserMapper;*/
    @Autowired
    private TRoleMapper tRoleMapper;


    @GetMapping("login")
    public String getLogin(){
        System.out.println("进来了，这是get的login");
        return "login";
    }

    @PostMapping("login")
    public String postLogin(String username, String password, Model model){
        System.out.println("进来了，这是post的login");

        //使用shiro编写认证操作

//        1.获取subject
        Subject subject = SecurityUtils.getSubject();
//        2.封装用户数据
        UsernamePasswordToken token=new UsernamePasswordToken(username,password);
//        3.执行登录方法
        try {
            
            subject.login(token);//会执行 MyRealm 中的 doGetAuthenticationInfo 方法
            //登陆成功
            //跳转到页面main.html
            System.out.println("用户认证成功 ");

            //角色校验
            //boolean isAdmin=subject.hasRole("banzhang");
            //System.out.println("角色验证"+isAdmin);

            //权限校验
            //System.out.println(subject.isPermitted("a:b"));


            return "user/main";
            //return "test";
        } catch (UnknownAccountException e) {
            //登录失败：用户名不存在
            System.out.println("用户不存在");
            model.addAttribute("msg","用户不存在");
            return "login";
        }catch (IncorrectCredentialsException e){
            //登录失败：密码错误
            System.out.println("密码错误");
            model.addAttribute("msg","密码错误");
            return "login";
        }
    }

    @RequestMapping("add")
    /*@RequiresPermissions("student:study")*/
    public String add() {
        System.out.println("add....");
        return "user/add";
    }

    @RequestMapping("update")
   /* @RequiresPermissions("student:yq")*/
    public String update() {
        System.out.println("update....");
        return "user/update";
    }
    @RequestMapping("noAuth")
    public String noAuth() {
        System.out.println("noAuth....，跳转到未授权页面");
        return "noAuth";
    }


}
~~~



## MyRealm.java

因为用不到 service，所以直接和 mapper 交互

~~~java

//Realm:用于连接数据
public class MyRealm extends AuthorizingRealm {


    @Autowired
    private TUserMapper tUserMapper;
    @Autowired
    private TRoleMapper tRoleMapper;
    @Autowired
    private TPermissionMapper tPermissionMapper;
    @Autowired
    private TUserRoleMapper tUserRoleMapper;

    /*
     * 作用：查询权限信息，并返回即可，不用任何比对
     * 何时触发： /user/query = roles["admin"]  /user/insert =perms["user:insert"]
     * 查询方式：通过用户名查询，该用户的 权限/角色 信息
     *
     * */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行授权逻辑");
		return null;
    }

    /*
    * 作用：查询身份信息，并返回即可，不用做比对
    * 查询方式：通过用户名查询用户信息
    * 何时触发：subject.login(token);
    * */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken arg0) throws AuthenticationException {
        System.out.println("执行认证逻辑");

        //编写shiro判断逻辑，判断用户名和密码
        //1.判断用户名
        UsernamePasswordToken token=(UsernamePasswordToken) arg0;

        TUser user = tUserMapper.findName(token.getUsername());

        if (user==null){
            //用户名不存在
            return null;//shiro底层会抛出UnknownAccountException
        }

        //2.判断密码；                         数据库中用户名     数据库中密码         realm标识
        return  new SimpleAuthenticationInfo(user.getUsername(),user.getPassword(),this.getName());


    }
}
~~~



# 用户授权

原文链接：https://blog.csdn.net/zhaolulu916/article/details/95957696

ShiroConfig （设置  filterMap.put("/update","perms[student:yq]")）——》单击href链接——》UserRealm （先调用认证获取Principal,然后调用doGetAuthorizationInfo中info授权信息）——》有权限:   执行Conroller(update) ——》没权限：执行Conroller（noAuth）

## main.html

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>main</title>
</head>
<body>



 <a href="add">add</a><br>

//点击 update 会进行用户授权逻辑，因为在 ShiroConfig 设置了 filterMap.put("/update","perms[student:yq]");
<a href="update">update</a>

</body>
</html>
~~~

## MyRealm.java

~~~java
//Realm:用于连接数据
public class MyRealm extends AuthorizingRealm {


    @Autowired
    private TUserMapper tUserMapper;
    @Autowired
    private TRoleMapper tRoleMapper;
    @Autowired
    private TPermissionMapper tPermissionMapper;
    @Autowired
    private TUserRoleMapper tUserRoleMapper;

    /*
     * 作用：查询权限信息，并返回即可，不用任何比对
     * 何时触发： /user/query = roles["admin"]  /user/insert =perms["user:insert"]
     * 查询方式：通过用户名查询，该用户的 权限/角色 信息
     *
     * */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行授权逻辑");

        //给资源进行授权认证
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();

        //到数据库查询当前登录用户的授权字符串
        //获取当前登录用户
        Subject subject= SecurityUtils.getSubject();
        /*报错：java.lang.ClassCastException: java.lang.String cannot be cast to com.example.demo.model.TUser
        TUser user= (TUser) principalCollection.getPrimaryPrincipal();*/
        String username=(String)subject.getPrincipal();
        System.out.println(username);

        TUser tUser = tUserMapper.findName(username);
        System.out.println("输出当前用户信息");
        System.out.println(tUser);

        //获得当前登录用户角色
        List<String> roles=tRoleMapper.queryAllRolenameById(tUser.getId());
        System.out.println("输出当前用户角色");
        //转成set
        Set<String> setroles = new HashSet<>(roles);
        System.out.println(setroles);

        //获取当前登录用户权限
        List<String> perms= tPermissionMapper.queryAllPermissionById(tUser.getId());
        System.out.println("输出当前用户权限");
        //转成set
        Set<String> setperms = new HashSet<>(perms);
        System.out.println(setperms);


        info.setStringPermissions(setperms);
        info.setRoles(setroles);

        System.out.println("完成授权逻辑");

        return info;
    }

    /*
    * 作用：查询身份信息，并返回即可，不用做比对
    * 查询方式：通过用户名查询用户信息
    * 何时触发：subject.login(token);
    * */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken arg0) throws AuthenticationException {
        System.out.println("执行认证逻辑");

		.......
    }
}
~~~



## mybatis

//TUser tUser = tUserMapper.findName(username);

~~~xml
<select id="findName" resultType="com.example.demo.model.TUser">
    select * from  t_user where username=#{username}
  </select>
~~~



// List<String> roles=tRoleMapper.queryAllRolenameById(tUser.getId());

~~~xml
    <select id="queryAllRolenameById" parameterType="int" resultType="string">
       select t_role.role_name
    from t_user
    JOIN t_user_role ON t_user.id=t_user_role.user_id
    JOIN t_role ON t_role.id=t_user_role.role_id
    where t_user.id = #{id}
    </select>
~~~



//List<String> perms= tPermissionMapper.queryAllPermissionById(tUser.getId());

~~~xml
  <select id="queryAllPermissionById" parameterType="int" resultType="string">

    select  distinct t_permission.permission_name
    from t_user
    JOIN t_user_role ON t_user.id=t_user_role.user_id
    JOIN t_role ON t_role.id =t_user_role.role_id
    JOIN t_role_permission ON t_role.id=t_role_permission.role_id
    JOIN t_permission ON t_role_permission.permission_id=t_permission.id
    where t_user.id=#{id}

    </select>
~~~



# 加密

## 

![1584199662391](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584199662391.png)

![1584199832883](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584199832883.png)



简单的例子

~~~java
 public static void main(String[] args) {
        String password="123";//明文密码
        //没有加盐的情况下加密
        String s = new Md5Hash(password).toBase64();
        String s2 = new Md5Hash(password).toString();
        System.out.println(s);
        System.out.println(s2);

        System.out.println("===========");
        //加盐的情况下加密且迭代10000次
        String salt = UUID.randomUUID().toString();
        String salt2 = UUID.randomUUID().toString();
        String s3 = new Md5Hash((password), salt, 10000).toBase64();
        String s4 = new Sha256Hash((password), salt2, 10000).toBase64();
        System.out.println(s3);
        System.out.println(s4);

    }
~~~



小结：~~Sha256Hash 更稳健，因此使用它加密~~，使用MD5

​	  因为Base64  产生的密文更短，所以建议使用Base64

​	  

## 注册加密

执行：正常的 MVC 流程

### regiest.html

~~~html
<!DOCTYPE html>

<html>
<head>
    <meta charset="UTF-8">
    <title>regist</title>
</head>
<body>


<form action="regist" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit" value="注册">

</form>
</body>
</html>
~~~



### Controller

~~~java
  @GetMapping("regist")
    public String getregist() {
        System.out.println("get regist");
        return "regist";
    }

    @PostMapping("regist")
    public String postregist(TUser tUser) {
        System.out.println("post regist");
        tUserService.register(tUser);

        return "login";
    }
~~~

### Service

~~~java
public interface TUserService {

    Integer register(TUser tUser);

}
~~~

### ServiceImpl

~~~java
@Service
public class TUserServiceImpl implements TUserService {
    @Autowired
    private TUserMapper tUserMapper;

    @Override
    public Integer register(TUser tUser) {

        //设置随机盐
        String salt = UUID.randomUUID().toString();

        //设置加密属性： Sha256算法，随机盐，迭代10000次
        //Sha256Hash sha256Hash = new Sha256Hash(tUser.getPassword(), salt, 10000);
        
        //将用户信息（包括密码的密文、盐）存入数据库
        //tUser.setSalt(salt);
        //tUser.setPassword(sha256Hash.toBase64());
        
        //上面的加密 会导致后面 登录解密不了； 猜测：Sha256Hash 加密 和  SHA-256 解密 不一样
         SimpleHash md5=new SimpleHash("MD5",tUser.getPassword(),salt,1024);
        tUser.setPassword(String.valueOf(md5));
        tUser.setSalt(salt);

        return tUserMapper.insertSelective(tUser);
    }
}
~~~

### Mapper

~~~java
  int insertSelective(TUser record);
~~~

### Mapper.xml

mybatis-generator 自动生成的，你可以自己写xml，只要是 添加 的就可以

~~~xml
  <insert id="insertSelective">
    <!--
      WARNING - @mbg.generated
      This element is automatically generated by MyBatis Generator, do not modify.
      This element was generated on Sun Mar 15 00:13:34 CST 2020.
    -->
    insert into t_user
    <trim prefix="(" suffix=")" suffixOverrides=",">
      <if test="id != null">
        id,
      </if>
      <if test="username != null">
        username,
      </if>
      <if test="password != null">
        password,
      </if>
      <if test="salt != null">
        salt,
      </if>
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides=",">
      <if test="id != null">
        #{id,jdbcType=INTEGER},
      </if>
      <if test="username != null">
        #{username,jdbcType=VARCHAR},
      </if>
      <if test="password != null">
        #{password,jdbcType=VARCHAR},
      </if>
      <if test="salt != null">
        #{salt,jdbcType=VARCHAR},
      </if>
    </trim>
  </insert>
~~~

### 结果

第四条数据

![1584203872228](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1584203872228.png)





## 登录解密(认证)

执行： ShiroConfig 先加载，然后进入login.html，点击“登录”进入 Controller 中 login 方法，执行到  subject.login(token);  时，跳转到 MyRealm 的认证(AuthenticationInfo)方法。如果密码比对错误，则返回login.html;如果密码比对正确，则进入 user/main.html

### ShiroConfig.java

和用户认证的差不多，就加了个密码凭证器，然后在 自定义realm中加入了 密码凭证器

```java
/*======================== 安全管理器 ===================================  */
@Bean
public DefaultWebSecurityManager  securityManager(MyRealm myRealm) {
    DefaultWebSecurityManager securityManager = new DefaultWebSecurityManager();
   securityManager.setRealm(myRealm);
    //使用了密码凭证使用，并注调上一行
    //securityManager.setRealm(myRealm(hashedCredentialsMatcher()));
    return securityManager;
}
    /*======================== 密码凭证器 ===================================  */
	//由于我们的密码校验交给Shiro的SimpleAuthenticationInfo进行处理了
    @Bean
    public HashedCredentialsMatcher hashedCredentialsMatcher() {
        HashedCredentialsMatcher hashedCredentialsMatcher = new HashedCredentialsMatcher();
        hashedCredentialsMatcher.setHashAlgorithmName("MD5");//散列算法:MD2、MD5、SHA-1、SHA-256、SHA-384、SHA-512等。
        hashedCredentialsMatcher.setHashIterations(1024);//散列的次数，默认1次， 设置两次相当于 md5(md5(""));
        return hashedCredentialsMatcher;
    }


    /*=================== 自定义 realm=====================*/
 /*   @Bean
    public MyRealm myRealm() {
        MyRealm myRealm = new MyRealm();
        return myRealm;
    }*/
    //使用了密码凭证使用下列方法
    @Bean
    public MyRealm myRealm(HashedCredentialsMatcher hashedCredentialsMatcher) {
        MyRealm myRealm = new MyRealm();
        myRealm.setCredentialsMatcher(hashedCredentialsMatcher);
        return myRealm;
    }

    /*================= Filter工厂，设置对应的过滤条件和跳转条件 =========================  */
    //
    @Bean
    public ShiroFilterFactoryBean shiroFilterFactoryBean(DefaultWebSecurityManager securityManager) {
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        //设置安全管理器
        shiroFilterFactoryBean.setSecurityManager(securityManager);

        Map<String, String> filterMap = new LinkedHashMap<String, String>();
        //login 无需认证就可以访问
        filterMap.put("/", "anon");
        filterMap.put("/login","anon");

        //授权过滤器
        //注意：当前授权拦截后，shiro会自动跳转到未授权页面
        filterMap.put("/update","perms[student:yq]");  //加了这句，才会执行用户授权方法；如果 perm[] 里面的权限与数据库中不同，则跳转到未授权页面

        //使用通配符，让所有的页面都必须认证才可以访问
        filterMap.put("/**","authc");

        //修改调整的登录页面,不然 默认跳到 xx.jsp 页面
        shiroFilterFactoryBean.setLoginUrl("login");
        //设置未授权提示页面
        shiroFilterFactoryBean.setUnauthorizedUrl("/noAuth");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterMap);

        return shiroFilterFactoryBean;
    }

    /*================= 加入注解的使用，不加入这个注解不生效 =========================  */
	//启shiro aop注解支持.  使用代理方式;所以需要开启代码支持;
    @Bean
    public AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor(DefaultWebSecurityManager securityManager) {
        AuthorizationAttributeSourceAdvisor authorizationAttributeSourceAdvisor = new AuthorizationAttributeSourceAdvisor();
        authorizationAttributeSourceAdvisor.setSecurityManager(securityManager);
        return authorizationAttributeSourceAdvisor;
    }
```



### login.html

```html
<!DOCTYPE html>

<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>login</title>
</head>
<body>

<h3 th:text="${msg}" style="color: red"></h3>
<form action="login" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit" value="登录">

</form>
</body>
</html>
```



### Controller

和用户认证的一样，没修改

~~~java
  @PostMapping("login")
    public String postLogin(TUser tUser,Model model) {
        System.out.println("进来了，这是post的login");

        //使用shiro编写认证操作

//        1.获取subject
        Subject subject = SecurityUtils.getSubject();
//        2.封装用户数据
        UsernamePasswordToken token = new UsernamePasswordToken(tUser.getUsername(), tUser.getPassword());
    
//        3.执行登录方法
        try {
            subject.login(token);
            //登陆成功
            //跳转到页面main.html
            System.out.println("用户认证成功 ");

            return "user/main";
            //return "test";
        } catch (UnknownAccountException e) {
            //登录失败：用户名不存在
            System.out.println("用户不存在");
            model.addAttribute("msg", "用户不存在");
            return "login";
        } catch (IncorrectCredentialsException e) {
            //登录失败：密码错误
            System.out.println("密码错误");
            model.addAttribute("msg", "密码错误");
            return "login";
        }
    }
~~~





### MyRealm.java

跟用户认证的差不多，返回的 SimpleAuthenticationInfo  加了 salt

~~~java

//Realm:用于连接数据
public class MyRealm extends AuthorizingRealm {


    @Autowired
    private TUserMapper tUserMapper;
    @Autowired
    private TRoleMapper tRoleMapper;
    @Autowired
    private TPermissionMapper tPermissionMapper;
    @Autowired
    private TUserRoleMapper tUserRoleMapper;

    /*
     * 作用：查询权限信息，并返回即可，不用任何比对
     * 何时触发： /user/query = roles["admin"]  /user/insert =perms["user:insert"]
     * 查询方式：通过用户名查询，该用户的 权限/角色 信息
     *
     * */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        System.out.println("执行授权逻辑");
		return null;
    }

 /*
    * 作用：查询身份信息，并返回即可，不用做比对
    * 查询方式：通过用户名查询用户信息
    * 何时触发：subject.login(token);
    * */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken arg0) throws AuthenticationException {
        System.out.println("执行认证逻辑");

        UsernamePasswordToken token = (UsernamePasswordToken) arg0;
        String username = String.valueOf(token.getUsername());
        // 从数据库获取对应用户名的用户
        TUser user =  tUserMapper.findName(username);
        if (user == null) {
            return null;
        }
        // 获取盐值
        ByteSource salt = ByteSource.Util.bytes(user.getSalt());
        //注意，数据库中的user的密码必须是要经过md5加密，不然还是会抛出异常！！！！！
         return new SimpleAuthenticationInfo(user,   //登录识别串信息，user.getUsername()
                                             user.getPassword(),//数据库中的密码，加了密的密文
                                             salt,  //盐值
                                             getName());//自定义 realm 的名字
        
    }
}
~~~



小结：注册加密 和 登录解密(认证) 选择的散列算法和迭代的次数 一定要一样！！！

之前注册加密 SHA256Hash 和 登陆解密(认证) 的 SHA-256，不一样。



# 会话管理

会话管理器管理着应用中所有Subject的会话的创建、维护、删除、失效、验证等工作。

是 Shiro 的核心组件，顶层组件 SecurityManager 直接继承了 SessionManager，且提供了 SessionsSecurityManager 实现直接把会话管理委托给相应的 SessionManager，DefaultSecurityManager 及 DefaultWebSecurityManager 默认 SecurityManager 都继承了 SessionsSecurityManager。





# 记住我

<https://blog.csdn.net/xslde_com/article/details/81137904>

~~~java
// 			
token.setRememberMe(true);
            subject.login(token);
~~~

