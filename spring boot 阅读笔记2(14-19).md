# spring boot 阅读笔记2(14-19)

标签（空格分隔）： Spring boot 笔记

---

##14.2


我们通常建议您将main application class放在根包其他类的之上。
在@EnableAutoConfiguration注释通常放在您的（main class）主类上
隐式定义了对某些项目的基本“search package”。
例如，如果您正在编写一个JPA Application，包中带有注解
@EnableAutoConfiguration的类将用于搜索@ entity的items

使用根包也允许@ComponentScan注释不需要使用就可以使用
指定一个basePackage属性。如果你的main类在the root package中你也可以使用@SpringBootApplication注释。

##15

 
许多已经发布Internet上Spring配置示例是使用XML来进行配置。
如果可能的话，总是尝试使用等效的基于java的配置。
搜索@Enable*注解可以是一个很好的起点。

###15.1  Importing additional configuration classes

您不需要将所有的@Configuration放到一个类中。

可以使用@Import注释导入额外的配置类。或者，您可以使用@ComponentScan自动接收所有Spring组件，包括@Configuration类。

###15.2 Importing XML configuration

如果你一定要使用基于xml配置，我们推荐在@Configuratio的类里面使用，同时使用@ImportResource注解要加载的xml 配置文件
##16
Spring  boot 自动装配(auto-configuration)会尝试指定的根据你添加的jar依赖去配置你的spring Application

例如,如果HSQLDB位于您的类路径中，您没有手动配置任何数据库连接bean，然后我们将自动配置内存数据库。

在你的某一个@Configuration的类里面，你需要去选择性的通过@EnableAutoConfiguration 或者@SpringBootApplication注解去自动装配（auto-configuration）
 
###16.1
 auto-configuration是非侵入式的，你可以定义自己的configuration去替换auto-configuration中特定的部分
 如果你想要知道auto-configuration中哪些特定的部分被应用，你可以使用debug模式启动application。
 This will enable debug logs for a selection of core loggers and log an auto-configuration report to the console.
 
###16.2
如果你发现哪个auto-configuration的被应用的类是你不想要的，你可使用
@EnableAutoConfiguration的exclude属性去排除
```
import org.springframework.boot.autoconfigure.*;
import org.springframework.boot.autoconfigure.jdbc.*;
import org.springframework.context.annotation.*;
@Configuration
@EnableAutoConfiguration(exclude={DataSourceAutoConfiguration.class})
public class MyConfiguration {
 }
```

##17Spring Beans and dependency injection
您可以自由使用任何标准Springframework技术来定义您的bean以及依赖项注入。
例如，可以使用@ConfigurationScan发现beans，再结合@Autowired 构造方法注入。
如果按照上面建议的方式构造代码(locating your application class in a root package)，则可以添加不带任何参数的@ComponentScan。您的所有应用程序组件(@ component，@service，@Repository，@controller将自动注册为beans。
例子:使用@Service的Bean通过构造方法注入获取RiskAssessor的Bean
```
package com.example.service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
@Service
public class DatabaseAccountService implements AccountService {
    private final RiskAssessor riskAssessor;
    @Autowired
    public DatabaseAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }
// ...
}
```
如果Bean只有一个构造器时。可以省略@Autowired
```
@Service
public class DatabaseAccountService implements AccountService {
    private final RiskAssessor riskAssessor;
    public DatabaseAccountService(RiskAssessor riskAssessor) {
        this.riskAssessor = riskAssessor;
    }
// ...
}
```
注意：
使用构造方法注入的riskAssessor允许被final所修饰，表明之后不能再被修改。

##18使用@SpringBootApplication 注解
@SpringBootApplication注解等价于@Configuration,@EnableAutoConfiguration and @ComponentScan 和他们的默认属性。

 1. @Configuration：提到@Configuration就要提到他的搭档@Bean
使用这两个注解就可以创建一个简单的spring配置类，可以用来替代相应的xml配置文件。@Configuration的注解类标识这个类可以使用Spring IoC容器作为bean定义的来源。@Bean注解告诉Spring，一个带有@Bean的注解方法将返回一个对象，该对象应该被注册为在Spring应用程序上下文中的bean。
 2. @EnableAutoConfiguration：能够自动配置spring的上下文，试图猜测和配置你想要的bean类，通常会自动根据你的类路径和你的bean定义自动配置。
 3. @ComponentScan：会自动扫描指定包下的全部标有@Component的类，并注册成bean，当然包括@Component下的子注解@Service,@Repository,@Controller。
```
package com.example.myproject;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
@SpringBootApplication
// same as @Configuration @EnableAutoConfiguration @ComponentScan
public class Application {
public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
##19 Running your application


将Application打为jar包和使用嵌入式HTTP服务器的最大优点之一是，您可以像其他任何程序一样运行应用程序。
debug spring boot Application也很简单， 不需要任何特殊的IDE插件或扩展。

注意：
这些是基于jar包的形式，如果你要将application打成war包，你需要涉及到你的server和IDE的文档。
###19.1 Running from an IDE
If you accidentally run a web application twice you will see a “Port already in use” error. STS users can use the Relaunch button rather than Run to ensure that any existing instance is closed
如果你运行web application两次，将会出现“Port already in use”--“端口被占用”的错误，sts用户使用“Relaunch”按钮，而不是“Run”,以确保现有实例被关闭。
###19.2 Running as a packaged application

如果你使用spring boot
将项目打成一个jar包
```
    $ mvn -Pnexus package -DskipTests
```
maven创建一个可执行的jar，你可以使用命令行执行：
```
    $ java -jar target/myproject-0.0.1-SNAPSHOT.jar
```


使用maven命令执行，application运行结果像在IDE中一样

```
    $ mvn spring-boot:run
 ```
 
 
   

23.5-23.8

