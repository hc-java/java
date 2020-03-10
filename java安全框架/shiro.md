入门视频：

<https://www.bilibili.com/video/av40342174?from=search&seid=8698743340264945136>

别人写的入门代码：

<https://blog.csdn.net/itzhuangjh/article/details/100063510>



www3c:

<https://www.w3cschool.cn/shiro/andc1if0.html>

# 简介

Apache Shiro 是一个强大易用的 **Java 安全框架**，提供了认证、授权、加密、会话管理、与 Web 集成、缓存等



`Shiro` 主要分为 **安全认证** 和 **接口授权** 两个部分，其中的核心组件为 `Subject`、`SecurityManager`、`Realms`，公共部分 `Shiro` 都已经为我们封装好了，我们只需要按照一定的规则去编写响应的代码即可…

- **Subject** **即表示主体，将用户的概念理解为当前操作的主体，因为它即可以是一个通过浏览器请求的用户，也可能是一个运行的程序，外部应用与 Subject 进行交互，记录当前操作用户。Subject 代表了当前用户的安全操作，SecurityManager 则管理所有用户的安全操作。**
- **SecurityManager** **即安全管理器，对所有的 Subject 进行安全管理，并通过它来提供安全管理的各种服务（认证、授权等）**
- **Realm** 充当了应用与数据安全间的 **桥梁** 或 **连接器**。当对用户执行认证（登录）和授权（访问控制）验证时，Shiro 会从应用配置的 Realm 中查找用户及其权限信息。

# 核心API

1、Subject：用户主体（把操作交给SecurityManager）

2、SecurityManager：安全管理器（关联Realm）

3、Realm：Shiro连接数据的桥梁（从数据库中获取数据）

# shiro内置过滤器

anon：无需认证（登录）可以访问

authc：必须认证才可以访问

user：如果使用RememberMe的功能可以直接访问

perms：该资源必须得到资源权限才可以访问

role：该资源必须得到角色权限才可以访问

# 用户认证

1.shiro配置

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

       /* filterMap.put("/add", "authc");
        filterMap.put("/update", "authc");*/
       //使用通配符，让所有的页面都必须认证才可以访问
        filterMap.put("/*","authc");


        //修改调整的登录页面,不然 默认跳到 xx.jsp 页面
        shiroFilterFactoryBean.setLoginUrl("login");

        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterMap);

        return shiroFilterFactoryBean;
    }
    /**
     * 创建DefaultWebSecurityManager安全管理器
     */
    @Bean(name = "defaultWebSecurityManager")
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager();
        //关联Reaml
        defaultWebSecurityManager.setRealm(userRealm);
        return defaultWebSecurityManager;
    }
    /**
     * 创建Realm
     */
    @Bean(name = "userRealm")
    public UserRealm getReaml(){
        return new UserRealm();
    }
}
~~~



2.输入localhost:8080/,进入login页面

~~~java
    @RequestMapping("/")
    public String tologinIn(){
        System.out.println("/");
        return "login";
    }
~~~



3.login页面中，点击“登录”，进入controller层

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
        用户名：<input type="text" name="name"><br>
        密码：<input type="password" name="password"><br>
        <input type="submit" value="登录">

    </form>
</body>
</html>

~~~

~~~java
 
@RequestMapping("login")
    public String login(String name, String password, Model model) {
        System.out.println("login....");
        //使用shiro编写认证操作

//        1.获取subject
        Subject subject = SecurityUtils.getSubject();
//        2.封装用户数据
        UsernamePasswordToken token=new UsernamePasswordToken(name,password);
//        3.执行登录方法
        try {
            subject.login(token);
            //登陆成功
            //跳转到页面main.html
            System.out.println("laile ");
            return "user/main";
  
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
~~~



subject.login(token);会执行UserRealm 中 doGetAuthenticationInfo 方法

~~~java
//Realm:用于连接数据
public class UserRealm extends AuthorizingRealm {

  @Autowired
    private UserMapper userMapper;

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection arg0){
        System.out.println("执行授权逻辑");
        return null;
    }
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken arg0) throws AuthenticationException {
        System.out.println("执行认证逻辑");
        //假设数据库的用户名和密码
        /*String name="admin";
        String password = "123";*/
        //编写shiro判断逻辑，判断用户名和密码
        //1.判断用户名
        UsernamePasswordToken token=(UsernamePasswordToken) arg0;

        User user = userMapper.findName(token.getUsername());

        if (user==null){
            //用户名不存在
            return null;//shiro底层会抛出UnknownAccountException
        }

        //2.判断密码
        return  new SimpleAuthenticationInfo("",user.getPassword(),"");
    }
}
~~~





# 用户授权

ShiroConfig增加了

 filterMap.put("/add","perms[user:add]");

  //设置未授权提示页面
  shiroFilterFactoryBean.setUnauthorizedUrl("/noAuth");



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

       /* filterMap.put("/add", "authc");
        filterMap.put("/update", "authc");*/

       //授权过滤器
        //注意：当前授权拦截后，shiro会自动跳转到未授权页面；
        //添加授权信息，为每个请求地址设置访问权限
        //perms后括号内的参数设置：为该请求设置一个名称，后期将权限名称保存在数据库中，通过查询用户是否有这些名称来判断用户是否拥有此类权限
        filterMap.put("/add","perms[user:add]");
       //使用通配符，让所有的页面都必须认证才可以访问
        filterMap.put("/*","authc");


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
    public DefaultWebSecurityManager getDefaultWebSecurityManager(@Qualifier("userRealm") UserRealm userRealm){
        DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager();
        //关联Reaml
        defaultWebSecurityManager.setRealm(userRealm);
        return defaultWebSecurityManager;
    }
    /**
     * 创建Realm
     */
    @Bean(name = "userRealm")
    public UserRealm getReaml(){
        return new UserRealm();
    }
}
~~~

```



UserRealm：修改了 AuthorizationInfo
```

~~~java
//Realm:用于连接数据
public class UserRealm extends AuthorizingRealm {

    @Autowired
    private UserMapper userMapper;

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection arg0){
        System.out.println("执行授权逻辑");

        //给资源进行授权
        SimpleAuthorizationInfo info = new SimpleAuthorizationInfo();
        //添加资源的授权字符串
        //info.addStringPermission("user:add");
        
         //到数据库查询当前登录用户的授权字符串
        //获取当前登录用户
        Subject subject= SecurityUtils.getSubject();
        User user=(User) subject.getPrincipal();
        User dbUser =userMapper.findById(user.getId());
        info.addStringPermission(dbUser.getPerms());

        return info;
    }
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken arg0) throws AuthenticationException {
        System.out.println("执行认证逻辑");
        //假设数据库的用户名和密码
        /*String name="admin";
        String password = "123";*/
        //编写shiro判断逻辑，判断用户名和密码
        //1.判断用户名
        UsernamePasswordToken token=(UsernamePasswordToken) arg0;

        User user = userMapper.findName(token.getUsername());

        if (user==null){
            //用户名不存在
            return null;//shiro底层会抛出UnknownAccountException
        }

        //2.判断密码
        return  new SimpleAuthenticationInfo("",user.getPassword(),"");
    }
}
~~~



# shiro标签

用户登录后，只显示自己可以访问的内容（其它没有权限的，不显示）

## pom.xml

~~~xml
   <!--thymeleaf 整合 shiro 标签-->
        <dependency>
            <groupId>com.github.theborakompanioni</groupId>
            <artifactId>thymeleaf-extras-shiro</artifactId>
            <version>2.0.0</version>
        </dependency>
~~~



## ShiroConfig.java

~~~java

    /*
    * 配置 ShiroDialect ,用于thymeleaf 和 shiro 标签配合使用
    * */
    @Bean
    public ShiroDialect getShiroDialect(){
        return new ShiroDialect();
    }
~~~



## 使用

![1583857501749](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583857501749.png)



总结：可以根据用户的权限，显示对应的功能

# springboot整合shiro

新建一个web项目

![1583773106925](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583773106925.png)

修改maven

![1583773236014](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583773236014.png)



自动生成的.xml的mybatis命名显红

URI is not registered ( Setting | Project Settings | Schemas and DTDs

解决：

![1583771483309](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583771483309.png)



在@SpringBootApplication下面加个@MapperScan(value = "com.example.demo.dao")



@Autowired注解引入的DAO或者Service有红色波浪线

![1583853602738](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1583853602738.png)