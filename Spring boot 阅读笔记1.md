#Spring boot 阅读笔记1

标签（空格分隔）： Spring boot 笔记

---
Part2
1.可以使用Spring Boot创建可以使用的Java应用程序 java jar
或更多的传统的war部署。我们还提供了一个运行“spring script”的命令行工具。
2.spring boot 使用最好是在JAVA 8,Spring Framework 4.3.12.RELEASE 及以上
3.spring boot 支持Tomcat7及以上
4.在确定使用spring boot前要确定你的JDK版本。最好是Java 8
5.还要确定Maven 版本 ，Maven 3.2 or above. 

##创建第一个Spring boot Application
1.create the POM.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>com.example</groupId>
	<artifactId>web-demo</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>jar</packaging>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>1.5.8.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
		<java.version>1.8</java.version>
	</properties>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>
</project>

```
2.添加依赖
```
<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
</dependencies>
```
3.写代码
```
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.*;
import org.springframework.stereotype.*;
import org.springframework.web.bind.annotation.*;
@RestController
@EnableAutoConfiguration
public class Example {
    @RequestMapping("/")
    String home() {
        return "Hello World!";
    }
public static void main(String[] args) throws Exception {
        SpringApplication.run(Example.class, args);
    }
}
```
@Controller 与@ResponseBody 进行了合并成一个新的注解 @RestController。
@EnableAutoConfiguration：根据您所拥有的jar依赖项推测你要如何配置Spring


