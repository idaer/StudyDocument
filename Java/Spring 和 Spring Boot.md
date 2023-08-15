# SpringBoot

---

## 一. 工程目录

工程文件结构

```java
-src
    -main
        -java
            -SpringbootApplication
        -resouces
            - statics
            - templates
            - application.y
```

## 简介

Spring Boot是简化Spring应用开发的一个框架、整个Spring技术栈的一个大整合（Spring全家桶时代）、J2EE开发的一站式解决方案（Spring Cloud是分布式整体解决方案）。

* 快速创建独立运行的Spring项目以及与主流框架集成
* 使用嵌入式的Servlet容器，应用无需打成WAR包
* starters自动依赖与版本控制
* 大量的自动配置，简化开发，也可修改默认值
* 无需配置XML，无代码生成，开箱即用
* 准生产环境的运行时应用监控
* 与云计算的天然集成 ~~（呃呃呃）~~

## 配置

### 项目配置

`POM.xml`是整个项目构建的核心。其中父项目是Spring Boot的版本仲裁中心（他来真正管理Spring Boot应用里面的所有依赖版本），以后我们导入依赖默认是不需要写版本（没有在dependencies里面管理的依赖自然需要声明版本号）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 项目组名 -->
    <groupId>com.example</groupId>
    <!-- 项目名称 -->
    <artifactId>myproject</artifactId>
    <!-- 项目版本 -->
    <version>0.0.1-SNAPSHOT</version>
    <!-- 表明父项目（Maven的项目包继承） -->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.0</version>
        <relativePath/>
        <!-- lookup parent from repository（从存储库中查找父级项目） -->
    </parent>
    <!-- 表明子项目 -->
    <modules>
        <!--管理的子项目模块,注意名称和子模块名称保持一致-->
        <module>springboot-hello-01</module>
    </modules>
```

启动器 spring-boot-starter（spring-boot场景启动器），spring-boot-starter-web 帮我们导入了web模块正常运行所依赖的组件。

```xml
<!-- 在pom.xml的 <project> 之内 -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### Bean配置

SpringBoot使用一个全局的配置文件，配置文件名固定：`application.properties` 或者 `application.yml`。配置文件放在 `src/main/resources`目录 或者 `类路径/config` 下。作用是修改SpringBoot自动配置的默认值。

#### YAML

```YAML
# 基础写法： K:V
name:123
# 对象写法，通常用缩进表示
friends:
    lastName: zhangsan
    age: 20
# 或者
friends: {lastName:zhangsan,age:18}
# 数组
pets:
    - cat
    - dog
    - pig
pets: [cat,dog,pig]
```

#### 配置文件注入(Bean)

##### @ConfigurationProperties

1. 导入配置文件处理器

```xml
<dependency>
 <groupId>org.springframework.boot</groupId>
 <artifactId>spring-boot-configuration-processor</artifactId>
 <optional>true</optional>
</dependency>
```

2. javaBean对象

通过`@ConfigurationProperties(prefix = "person")`将配置文件和类进行绑定

```java
/**
 * 将配置文件中配置的每一个属性的值，映射到这个组件中
 * @ConfigurationProperties：告诉SpringBoot将本类中的所有属性和配置文件中相关的配置进行绑定;
 *      prefix = "person":配置文件中哪个下面的所有属性进行一一映射
 * 只有这个组件是容器中的组件，才能容器提供的@ConfigurationProperties功能;
 */
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Date birth;
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
...
}
```

3. 配置文件application.yml

```YAML
    person:
    lastName: haha
    age: 18
    boss: false
    birth: 2022/01/01
    maps: {k1: v1,k2: v2}
    lists:
        - lisi
        - wangwu
    dog:
        name: 芒果
        age: 1
```

或者配置文件application.properties

```properties
#解决乱码问题
spring.message.encoding=UTF-8
#person
person.last-name=haha
person.age=20
person.birth=2022/01/02
person.boss=true
person.maps.k1=v1
person.maps.k2=v2
person.lists=a,b,c
person.dog.name=丸子
person.dog.age=5
```

4. 再使用`@Autowired`对实例化对象进行注入

##### 与@Value的区别

* @ConfigurationProperties 是批量注入配置文件中的属性，@Value 是一个个指定
* @ConfigurationProperties 支持松散绑定(松散语法) 、不支持SpEL(表达式如#{2*4})、支持JSR303数据校验 、支持复杂类型封装(如map)
* @Value 不支持松散绑定(松散语法) 、支持SpEL、不支持JSR303数据校验 、不支持复杂类型封装

使用`@value`绑定

```java
@Component
// @ConfigurationProperties(prefix = "person")
public class Person {
    @Value("${person.last-name}")
    private String lastName;
    @Value("#{2*4}")
    private Integer age;
    @Value("true")
    private Boolean boss;
    @Value("${person.birth}")
    private Date birth;
    ...
```

松散绑定:

* person.firstName：使用标准方式
* person.first-name：大写用-
* person.first_name：大写用_
* PERSON_FIRST_NAME：推荐系统属性使用这种写法

JSR303数据校验:

* @Validated
* @NotNull

##### 加载外部配置

**@PropertySource**：加载指定的配置文件

```java
@PropertySource("classpath:person.properties")
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    ...
}
```

**@ImportResource**：导入Spring的配置文件，让配置文件里面的内容生效--标注在一个配置类上

xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

  <bean id="helloService" class="com.stydyspring.spring_boot_demo0.service.HelloService"></bean>
</beans>
```

标注在主类上

```java
@SpringBootApplication
// 导入Spring的配置文件让其生效
@ImportResource(locations = {"classpath:beans.xml"})
public class SpringBootDemo0Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringBootDemo0Application.class, args);
    }
}
```

SpringBoot中推荐使用`@Configuration`来配置Bean

```java
/**
 * @Configuration：指明当前类是一个配置类，就是来替代之前的Spring配置文件
 * 
 * 在配置文件中用<bean><bean/>标签添加组件。在配置类中使用@Bean注解
 */
@Configuration
public class MyAppConfig {
    // 将方法的返回值添加到容器中;容器中这个组件默认的id就是方法名
    @Bean
    public HelloService helloService(){
        System.out.println("配置类@Bean给容器中添加组件了");
        return new HelloService();
    }
    //可以同时配置多个bean
    ...
}
```

#### Profile

Profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境。 多profile文件形式格式如：`application-{profile}.properties/yml`，如 `application-dev.properties`、`application-prod.properties`。帮助开发者便捷地切换不同的开发环境

选择配置文件的方式：

* 命令行 --spring.profiles.active=dev
* 配置文件 spring.profiles.active=dev
* jvm参数 –Dspring.profiles.active=dev

yml文件还支持文档分块:

```yaml
# 这个代表第一个文档块
server:
  port: 8085

spring:
  profiles:
    active: dev # 当需要哪个环境配置的时候，只需要在这里修改值。

---
# 这个代表第二个文档块
server:
  port: 8086

spring:
  profiles: dev # 指定dev，代表开发环境
  
---
# 这个代表第三个文档块
server:
  port: 8087

spring:
  profiles: pro # 指定pro，代表生产环境

```

代码中一共有三个yml文档块，其中在没有配置spring:profiles:active的情况下，默认会使用第一个文档块的配置。当在配置的情况下，会根据配置的值去使用哪个yml文档块的配置。

#### 配置文件加载位置

spring boot 启动会扫描以下位置的`application.properties`或者`application.yml`文件作为Spring boot的默认配置文件

1. file:`./config/`
2. file:`./`
3. classpath:`/config/`
4. classpath:`/`

以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，高优先级配置内容会覆盖低优先级配置内容。 可以通过配置spring.config.location来改变默认配置。项目打包好以后，可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置：`java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties`

#### 外部配置加载顺序

Spring Boot支持多种外部配置方式，优先级从高到低。高优先级的配置覆盖低优先级的配置，所有的配置会形成互补配置：

1. 命令行参数

所有的配置都可以在命令行上进行指定：`java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --server.port=8087 --server.context-path=/abc`
多个配置用空格分开; --配置项=值

2. 来自java:comp/env的JNDI属性
3. Java系统属性(System.getProperties())
4. 操作系统环境变量
5. RandomValuePropertySource配置的random.*属性值

**由jar包外向jar包内进行寻找。优先加载带profile：**

6. jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件
7. jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件

**再来加载不带profile：**

8. jar包外部的application.properties或application.yml(不带spring-profile)配置文件
9. jar包内部的application.properties或application,.yml(不带spring-profile)配置文件
10. @Configuration注解类上@PropertySource

11. 通过SpringApplicationsetDefaultProperties指定的默认属性