#Spring framework 读书笔记1

标签（空格分隔）： 未分类

---

##7.1Spring IOC
IOC,控制反转。
DI，依赖注入
###7.1.1
DI的实现方法：
 1. set  根据类型自动匹配，与名称无关
 2. 构造方法  根据类型进行装配，但是必须存在相应的构造器。
 3. 工厂 BeanFactory
 4. 注解 Annotation
###7.1.2
Spring Framework’s IoC container的基础包：
org.springframework.beans
org.springframework.context
###7.1.3
BeanFactory接口提供了一个先进的能够管理任何类型的对象的配置机制。
ApplicationContext是BeanFactory的子接口
在Spring中，构成应用程序主干的对象和由Spring IoC管理的对象容器被称为Beans
##7.2
###7.2.1ApplicationContext 
ApplicationContext的主要实现类是ClassPathXmlApplicationContext和FileSystemXmlApplicationContext,前者默认从类路径加载配置文件，后者默认从文件系统加载文件。

    //eg1.  
    ApplicationContext ctx = new ClassPathXmlApplicationContext("bean.xml");  
      
    //eg2.   
    String[] locations = {"bean1.xml", "bean2.xml", "bean3.xml"};  
    ApplicationContext ctx = new ClassPathXmlApplication(locations);  
    
###7.2.2获取Bean
通过T getBean(String name, Class<T>requiredType)

```
// create and configure beans
ApplicationContext context = new ClassPathXmlApplicationContext("services.xml", "daos.xml");
// retrieve configured instance
PetStoreService service = context.getBean("petStore",PetStoreService.class
);
// use configured instance
List<String> userList = service.getUsernameList();
```
##7.3Bean概述
一个Spring IoC容器管理一个或多个Beans
这些bean是通过配置以XML的形式提供给容器的< bean / >定义。
##7.4依赖
当一个对象的创建涉及到其他对象的创建时，比如一个对象A的成员变量持有着另一个对象B的引用，这就是依赖，A依赖于B。IOC机制既然负责了对象的创建，那么这个依赖关系也就必须由IOC容器负责起来。负责的方式就是DI——依赖注入，通过将依赖关系写入配置文件，然后在创建有依赖关系的对象时，由IOC容器注入依赖的对象。
###7.4.1构造器注入
####示例1
```
package x.y;
public class Foo {
public
 Foo(Bar bar, Baz baz) {
// ...
    }
}
```
XML
```
<beans>
    <bean id="foo" class="x.y.Foo">
        <constructor-arg ref="bar"/>
        <constructor-arg ref="baz"/>
    </bean>
    <bean id="bar" class="x.y.Bar"/>
    <bean id="baz" class="x.y.Baz"/>
</beans>
```
####示例2
```
package examples;
public class ExampleBean {
    // Number of years to calculate the Ultimate Answer
    privat int years;
    // The Answer to Life, the Universe, and Everything
    private String ultimateAnswer;
    public ExampleBean(int years, String ultimateAnswer) {
     this.years = years;
     this.ultimateAnswer = ultimateAnswer;
    }
}
    
```
构造器参数类型为基本类型时。
```
<bean id="exampleBean" class="examples.ExampleBean">
		<constructor-arg type="int" value="7500000" />
		<constructor-arg type="java.lang.String" value="42" />
</bean>
```
####示例3
使用index属性指定构造函数参数的索引
```
<bean id="exampleBean" class="examples.ExampleBean">
		<constructor-arg index="0" value="7500000" />
		<constructor-arg index="1" value="42" />
</bean>
```
####示例4
构造方法参数列表名字与bean的name属性相同时使用
```
<bean id="exampleBean" class="examples.ExampleBean">
		<constructor-arg name="years" value="7500000" />
		<constructor-arg name="ultimateAnswer" value="42" />
</bean>
```
###7.4.1属性注入（set注入）
```
<bean id="exampleBean" class="examples.ExampleBean">
		<!-- setter injection using the nested ref element -->
		<property name="beanOne">
			<ref bean="anotherExampleBean" />
		</property>
		<!-- setter injection using the neater ref attribute -->
		<property name="beanTwo" ref="yetAnotherBean" />
		<property name="integerProperty" value="1" />
</bean>
<bean id="anotherExampleBean" class="examples.AnotherBean" />
<bean id="yetAnotherBean" class="examples.YetAnotherBean" />
```
```
public class ExampleBean {
    private AnotherBean beanOne;
    private YetAnotherBean beanTwo;
    private int i;
    public void setBeanOne(AnotherBean beanOne) {
        this.beanOne = beanOne;
    }
    public void setBeanTwo(YetAnotherBean beanTwo) {
        this.beanTwo = beanTwo;
    }
    public void setIntegerProperty(int i) {
        this.i = i;
    }
}
```

##7.5 Bean scopes
1.Singleton
当一个bean的作用域为singleton, 那么Spring IoC容器中只会存在一个共享的bean实例,也就是Bean是单例的
2.Prototype
Prototype作用域的bean会导致在每次对该bean请求（将其注入到另一个bean中，或者以程序的方式调用容器的getBean()方法）时都会创建一个新的bean实例
3.request
 针对每次HTTP请求，Spring容器会根据bean定义创建一个全新的bean实例， 且该loginAction bean实例仅在当前HTTP request内有效。 当处理请求结束，request作用域的bean实例将被销毁。
4.session
针对某个HTTP Session，Spring容器会根据 bean定义创建一个全新的 bean实例， 且该 bean仅在当前HTTPSession内有效。
当HTTP Session最终被废弃的时候，在该HTTP Session作用域内的bean也会被废弃掉。
 