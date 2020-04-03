同步任务是指事情需要一件一件的做，做完当前的任务，才能开始做下一任务；

异步任务是指做当前任务的同时，后台还可以在执行其他任务，可理解为可同时执行多任务，不必一件一件接着去做

# 同步任务

## Controller

```java
@RestController
public class TestController {
    @Autowired
    private AsyncService asyncService;

    @RequestMapping("/hello")
    public String hello() throws InterruptedException {
        asyncService.hello();
        return "success";
    }
}
```



## Service

~~~java
@Service
public class AsyncService {
    @Async
    public void hello() throws InterruptedException {
        try {
            Thread.sleep(3000);

        }catch (InterruptedException e){
            e.printStackTrace();
        }
        System.out.println("处理数据中。。。");
    }
}
~~~

输入网址后，页面会等待三秒钟才显示内容，控制台也要等待三秒才会输出。

执行到 asyncService.hello()时 会执行 Service的hello方法，等hello方法执行完后，才会继续执行Controller中下面的代码



# 异步任务

## Controller

~~~java
@RestController
public class TestController {
    @Autowired
    private AsyncService asyncService;

    @RequestMapping("/hello")
    public String hello() throws InterruptedException {
        asyncService.hello();
        return "success";
    }
}
~~~



## Service

~~~java
@Service
public class AsyncService {
    @Async
    public void hello() throws InterruptedException {
        try {
            Thread.sleep(3000);

        }catch (InterruptedException e){
            e.printStackTrace();
        }
        System.out.println("处理数据中。。。");
    }
}

~~~

主配置xxxApplication类上面要加：

 @EnableAsync//开启异步注解功能



输入网址后，页面会立刻显示，控制台要等待三秒输出内容。

执行到 asyncService.hello() 时，controller 可以继续执行下去，同时执行 Service 里面的hello方法，因为hello方法要睡眠3000毫秒，所以控制台输出的比页面慢。



# 定时任务

@EnableScheduling  //开启定时任务

@Scheduled   //给要定时的任务 加上这个注解



![1585898771312](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1585898771312.png)



Cron表达式是一个字符串，字符串以5或6个空格隔开，分为6或7个域，每一个域代表一个含义，Cron有如下两种语法格式： 

Seconds(秒)， Minutes(分)， Hours(时) DayofMonth(日) ,Month(月)， Day of Week(周几) 



Cron表达式范例：

​                 每隔5秒执行一次：*/5 * * * * ?

​                 每隔1分钟执行一次：0 */1 * * * ?

​                 每天23点执行一次：0 0 23 * * ?

​                 每天凌晨1点执行一次：0 0 1 * * ?

​                 每月1号凌晨1点执行一次：0 0 1 1 * ?

​                 每月最后一天23点执行一次：0 0 23 L * ?

​                 每周星期天凌晨1点实行一次：0 0 1 ? * L

​                 在26分、29分、33分执行一次：0 26,29,33 * * * ?

​                 每天的0点、13点、18点、21点都执行一次：0 0 0,13,18,21 * * ?

![1585899984004](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1585899984004.png)

## Service

~~~java
@Service
public class ScheduledService {
    @Scheduled(cron="0 * * * * MON-SAT")
    public  void hi(){
        System.out.println("hi....");
    }
}

~~~

```
主配置xxxApplication类上面要加：
@EnableScheduling //开启定时任务注解
```



定时任务，只要开启服务器，设置好执行时间，每隔一段规定的时间都会执行。



# 简单邮件任务

要先开启 POP3/SMTP服务 和 IMAP/SMTP服务

![1585900716931](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1585900716931.png)

点击 "生成授权码"，复制到appliacation.yml



## pom.xml

先加依赖

~~~xml
    <!--邮件任务依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
~~~

## application.yml

~~~yml
spring:
  mail:
    username: 528792797@qq.com  #自己的邮箱
    password: fhmwbgsnmfvsbjhi  #刚刚粘贴的授权码
    host: smtp.qq.com
    properties.mail.smtp.ssl.enable: true  #qq 的额外配置
~~~



## Test

~~~java
@SpringBootTest
class MyblogApplicationTests {

    @Autowired
    JavaMailSenderImpl mailSender;
    @Test
    public void contextLoads() {
        SimpleMailMessage message = new SimpleMailMessage();
        //邮件设置
        message.setSubject("通知-1");  //邮件标题
        message.setText("好好学习啊");  //邮件内容

        message.setTo("528792797@qq.com");  //收件人
        message.setFrom("528792797@qq.com");//发件人

        mailSender.send(message);
    }

}
~~~





# 复杂邮件任务

## Test

~~~java
@SpringBootTest
class MyblogApplicationTests {

    @Autowired
    JavaMailSenderImpl mailSender;
   
    @Test
    public void contextLoads2() throws Exception{
        //创建一个复杂的消息邮件
        MimeMessage mimeMessage=mailSender.createMimeMessage();
        MimeMessageHelper helper = new MimeMessageHelper(mimeMessage,true);  //要上传文件

        //邮件设置
        helper.setSubject("通知-1");
        helper.setText("<b style='color:red'>好好学习啊</b>",true); //设置为true，可以将html标签生效

        helper.setTo("906593688@qq.com");//收件人
        helper.setFrom("528792797@qq.com");//发件人

        //上传文件
        helper.addAttachment("1.jpg",new File("C:\\Users\\HC\\Desktop\\images\\photo.jpg")); //图片名字，文件流(本地的图片路径)
        helper.addAttachment("2.jpg",new File("C:\\Users\\HC\\Desktop\\images\\vegetable.png")); //图片名字，文件流

        mailSender.send(mimeMessage);
    }

}
~~~

