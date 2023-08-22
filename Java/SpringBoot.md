# SpirngBoot 脉络梳理

## 简介

Spring Boot是简化Spring应用开发的一个框架、整个Spring技术栈的一个大整合（Spring全家桶时代）、J2EE开发的一站式解决方案（Spring Cloud是分布式整体解决方案）。

* 快速创建独立运行的Spring项目以及与主流框架集成
* 使用嵌入式的Servlet容器，应用无需打成WAR包
* starters自动依赖与版本控制
* 大量的自动配置，简化开发，也可修改默认值
* 无需配置XML，无代码生成，开箱即用
* 准生产环境的运行时应用监控
* 与云计算的天然集成 ~~（呃呃呃）~~

### 项目目录

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

## 构建项目

### 初始化

暂时省略

### 配置POM

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

## 配置

Spring Boot使用全局配置文件，配置文件名是固定的；

* application.properties
* application.yml

配置文件作用：修改Spring Boot在底层封装好的默认值；

### YAML配置语言

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
#行内写法
pets: [cat,dog,pig]

```

### 配置文件注入

1. 编写配置文件

    `application.yml` 配置文件

    ```yaml
    person:
    age: 18
    boss: false
    birth: 2017/12/12
    maps: {k1: v1,k2: 12}
    lists:
    - lisi
    - zhaoliu
    dog:
        name: wangwang
        age: 2
    last-name: wanghuahua
    ```

    `application.properties` 配置文件（二选一）

    ```properties
    idea配置文件utf-8
    properties 默认GBK
    person.age=12
    person.boss=false
    person.last-name=张三
    person.maps.k1=v1
    person.maps.k2=v2
    person.lists=a,b,c
    person.dog.name=wanghuahu
    person.dog.age=15
    ```

    如果中文输出乱码，改进settings-->file encoding -->[property-->utf-8 ,勾选转成ascii]

2. 配置文件处理器

```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐configuration‐processor</artifactId>
    <optional>true</optional>
</dependency> 
```

#### @ConfigurationProperties

JavaBean中

```java
/**
* 将配置文件的配置每个属性的值，映射到组件中
* @ConfigurationProperties:告诉SpringBoot将文本的所有属性和配置文件中的相关配置进行绑定；
* prefix = "person" 配置文件爱你的那个属性进行一一映射
* *
只有这个组件是容器中的组件，才能提供到容器中
*/
//表明组件
@Component
//自动从全局配置文件中注入
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    private Integer age;
    private Boolean boss;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
    ...
}
```

#### @Value

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

#### @Value和@ConfigurationProperties的区别

||@ConfigurationProperties|@Value|
|:--:|:--:|:--:|
|功能|批量注入配置文件属性|单个指定|
|松散绑定(语法)|支持|不支持|
|spEL|不支持|支持|
|JSR303校验|支持|不支持|
|复杂类型|支持|不支持|

##### 松散绑定

* person.firstName：使用标准方式
* person.first-name：大写用-
* person.first_name：大写用_
* PERSON_FIRST_NAME：推荐系统属性使用这种写法

##### JSR303数据校验

* @Validated
* @NotNull

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated
public class Person {
    @Email
    private String lastName;
```

#### 加载指定的properties配置文件

使用`@PropertySource`注解

作用：加载指定的properties配置文件

---

新建一个person.properties文件

```properties
person.age=12
person.boss=false
person.last-name=李四
person.maps.k1=v1
person.maps.k2=v2
person.lists=a,b,c
person.dog.name=wanghuahu
person.dog.age=15
```

在javaBean中加入`@PropertySource`注解

```java
@PropertySource(value = {"classpath:person.properties"})
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String lastName;
    ...
}
```

#### 加载指定的xml（Spring）配置文件

使用`@ImportResource`注解

作用：导入Spring配置文件，并且让这个配置文件生效

---

新建一个Spring的配置文件，bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="HelloService" class="com.wdjr.springboot.service.HelloService"></bean>
</beans>
```

编写测试类，检查容器是否加载Spring配置文件写的bean

```java
@Autowired
ApplicationContext ioc;

@Test
public void testHelloService(){
    boolean b = ioc.containsBean("HelloService");
    System.out.println(b);
}
```

结果为false，没有加载配置的内容

使用@ImportResource注解,将`@ImportResource`标注在主配置类上

```java
@ImportResource(locations={"classpath:beans.xml"})
@SpringBootApplication
public class SpringBoot02ConfigApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringBoot02ConfigApplication.class, args);
    }
}
```

再次运行检测,结果为true

### 配置类注入

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


//测试
@Autowired
ApplicationContext ioc;

@Test
public void testHelloService(){
    boolean b = ioc.containsBean("helloService");
    System.out.println(b);
}
```

### 配置文件加载

#### Profile

Profile是Spring对不同环境提供不同配置功能的支持，可以通过激活、指定参数等方式快速切换环境。 多profile文件形式格式如：`application-{profile}.properties/yml`，如 `application-dev.properties`、`application-prod.properties`。帮助开发者便捷地切换不同的开发环境

选择配置文件的方式：

* 命令行 --spring.profiles.active=dev
* 配置文件 spring.profiles.active=dev
* jvm参数 –Dspring.profiles.active=dev

#### YAML文档分块

yml文件还支持文档分块:

```yaml
# 这个代表第一个文档块，也是默认环境配置
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

![file](https://pic2.zhimg.com/80/v2-82fed3dc047df18450d04703cfb59d09_720w.webp)

以上是按照优先级从高到低的顺序，所有位置的文件都会被加载，高优先级配置内容会覆盖低优先级配置内容。 可以通过配置spring.config.location来改变默认配置。项目打包好以后，可以使用命令行参数的形式，启动项目的时候来指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补配置：`java -jar spring-boot-02-config-02-0.0.1-SNAPSHOT.jar --spring.config.location=G:/application.properties`

#### 引入外部配置

SpringBoot也可以从以下位置加载配置；**优先级从高到低；高优先级覆盖低优先级，可以互补**

1. 命令行参数

   java -jar spring-boot-config-02-0.0.1-SNAPSHOT.jar --server.port=9005 --server.context-path=/abc
  
   中间一个空格
2. 来自java:comp/env的JNDI属性
3. java系统属性（System.getProperties()）
4. 操作系统环境变量
5. RandomValuePropertySource配置的random.*属性值
  
   ​
  
   **优先加载profile,    由jar包外到jar包内**
6. **jar包外部的application-{profile}.properties或application.yml(带Spring.profile)配置文件**
7. **jar包内部的application-{profile}.properties或application.yml(带Spring.profile)配置文件**
8. **jar包外部的application.properties或application.yml(带Spring.profile)配置文件**
9. **jar包内部的application.properties或application.yml(不带spring.profile)配置文件**
  
   ​
10. @Configuration注解类的@PropertySource
11. 通过SpringApplication.setDefaultProperties指定的默认属性

[官方文档](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#boot-features-external-config)

### 自动配置

配置文件到底怎么写？

[Spring的所有配置参数](https://docs.spring.io/spring-boot/docs/2.0.1.RELEASE/reference/htmlsingle/#common-application-properties)

#### 自动配置原理

SpringBoot启动的时候加载主配置类，开启自动配置功能，`@EnableAutoConfiguration`

在`@EnableAutoConfiguration`启动后，SpirngBoot会进行以下操作

* 利用AutoConfigurationImportSelector给容器中导入一些组件（可能）？
* 可以查看selectImports()方法的内容
* 获取候选的配置

```java
List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
```

* 扫描类路径下的`SpringFactoriesLoader.loadFactoryNames(）`
* 扫描所有jar包类路径下的`MATA-INF/spring.factories`把扫描到的这些文件的内容包装成properties对象
* 从properties中获取到`EnableAutoConfiguration.class`类（类名）对应的值，然后把他们添加到容器中

最后每一个自动配置类进行自动配置功能

下面以HttpEncodingAutoConfiguration 为例

```java
@Configuration //表示是一个配置类，以前编写的配置文件一样，也可以给容器中添加组件
@EnableConfigurationProperties({HttpEncodingProperties.class})//启动指定类的Configurationproperties功能；将配置文件中的值和HttpEncodingProperties绑定起来了；并把HttpEncodingProperties加入ioc容器中
@ConditionalOnWebApplication//根据不同的条件，进行判断，如果满足条件，整个配置类里面的配置就会失效，判断是否为web应用；
(
    type = Type.SERVLET
)
@ConditionalOnClass({CharacterEncodingFilter.class})//判断当前项目有没有这个类，解决乱码的过滤器
@ConditionalOnProperty(
    prefix = "spring.http.encoding",
    value = {"enabled"},
    matchIfMissing = true
)//判断配置文件是否存在某个配置 spring.http.encoding，matchIfMissing = true如果不存在也是成立，即使不配置也生效
public class HttpEncodingAutoConfiguration {
   //给容器添加组件，这个组件的值需要从properties属性中获取
    private final HttpEncodingProperties properties;
    //只有一个有参数构造器情况下，参数的值就会从容器中拿
    public HttpEncodingAutoConfiguration(HttpEncodingProperties properties) {
        this.properties = properties;
    }

    @Bean
    @ConditionalOnMissingBean
    public CharacterEncodingFilter characterEncodingFilter() {
        CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();
        filter.setEncoding(this.properties.getCharset().name());
        filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpEncodingProperties.Type.REQUEST));
        filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpEncodingProperties.Type.RESPONSE));
        return filter;
    }

```

所有在配置文件中能配置的属性都是在xxxProperties类中封装着；配置文件能配置什么就可以参照某个功能对应的这个属性类

```java
@ConfigurationProperties(prefix = "spring.http.encoding")//从配置文件中的值进行绑定和bean属性进行绑定
public class HttpEncodingProperties {
```

#### 所有的自动配置组件

每一个xxxAutoConfiguration这样的类都是容器中的一个组件，都加入到容器中；

作用：用他们做自动配置

```Properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
org.springframework.boot.autoconfigure.admin.SpringApplicationAdminJmxAutoConfiguration,\
org.springframework.boot.autoconfigure.aop.AopAutoConfiguration,\
org.springframework.boot.autoconfigure.amqp.RabbitAutoConfiguration,\
org.springframework.boot.autoconfigure.batch.BatchAutoConfiguration,\
org.springframework.boot.autoconfigure.cache.CacheAutoConfiguration,\
org.springframework.boot.autoconfigure.cassandra.CassandraAutoConfiguration,\
org.springframework.boot.autoconfigure.cloud.CloudAutoConfiguration,\
org.springframework.boot.autoconfigure.context.ConfigurationPropertiesAutoConfiguration,\
org.springframework.boot.autoconfigure.context.MessageSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.context.PropertyPlaceholderAutoConfiguration,\
org.springframework.boot.autoconfigure.couchbase.CouchbaseAutoConfiguration,\
org.springframework.boot.autoconfigure.dao.PersistenceExceptionTranslationAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.cassandra.CassandraRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.couchbase.CouchbaseRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.elasticsearch.ElasticsearchRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.ldap.LdapRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoReactiveRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.mongo.MongoRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jDataAutoConfiguration,\
org.springframework.boot.autoconfigure.data.neo4j.Neo4jRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.solr.SolrRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.data.redis.RedisRepositoriesAutoConfiguration,\
org.springframework.boot.autoconfigure.data.rest.RepositoryRestMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.data.web.SpringDataWebAutoConfiguration,\
org.springframework.boot.autoconfigure.elasticsearch.jest.JestAutoConfiguration,\
org.springframework.boot.autoconfigure.flyway.FlywayAutoConfiguration,\
org.springframework.boot.autoconfigure.freemarker.FreeMarkerAutoConfiguration,\
org.springframework.boot.autoconfigure.gson.GsonAutoConfiguration,\
org.springframework.boot.autoconfigure.h2.H2ConsoleAutoConfiguration,\
org.springframework.boot.autoconfigure.hateoas.HypermediaAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastAutoConfiguration,\
org.springframework.boot.autoconfigure.hazelcast.HazelcastJpaDependencyAutoConfiguration,\
org.springframework.boot.autoconfigure.http.HttpMessageConvertersAutoConfiguration,\
org.springframework.boot.autoconfigure.http.codec.CodecsAutoConfiguration,\
org.springframework.boot.autoconfigure.influx.InfluxDbAutoConfiguration,\
org.springframework.boot.autoconfigure.info.ProjectInfoAutoConfiguration,\
org.springframework.boot.autoconfigure.integration.IntegrationAutoConfiguration,\
org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JdbcTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.JndiDataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.XADataSourceAutoConfiguration,\
org.springframework.boot.autoconfigure.jdbc.DataSourceTransactionManagerAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JmsAutoConfiguration,\
org.springframework.boot.autoconfigure.jmx.JmxAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.JndiConnectionFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.activemq.ActiveMQAutoConfiguration,\
org.springframework.boot.autoconfigure.jms.artemis.ArtemisAutoConfiguration,\
org.springframework.boot.autoconfigure.groovy.template.GroovyTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.jersey.JerseyAutoConfiguration,\
org.springframework.boot.autoconfigure.jooq.JooqAutoConfiguration,\
org.springframework.boot.autoconfigure.jsonb.JsonbAutoConfiguration,\
org.springframework.boot.autoconfigure.kafka.KafkaAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.embedded.EmbeddedLdapAutoConfiguration,\
org.springframework.boot.autoconfigure.ldap.LdapAutoConfiguration,\
org.springframework.boot.autoconfigure.liquibase.LiquibaseAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderAutoConfiguration,\
org.springframework.boot.autoconfigure.mail.MailSenderValidatorAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.embedded.EmbeddedMongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration,\
org.springframework.boot.autoconfigure.mongo.MongoReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.mustache.MustacheAutoConfiguration,\
org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration,\
org.springframework.boot.autoconfigure.quartz.QuartzAutoConfiguration,\
org.springframework.boot.autoconfigure.reactor.core.ReactorCoreAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.UserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.security.servlet.SecurityFilterAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveSecurityAutoConfiguration,\
org.springframework.boot.autoconfigure.security.reactive.ReactiveUserDetailsServiceAutoConfiguration,\
org.springframework.boot.autoconfigure.sendgrid.SendGridAutoConfiguration,\
org.springframework.boot.autoconfigure.session.SessionAutoConfiguration,\
org.springframework.boot.autoconfigure.security.oauth2.client.OAuth2ClientAutoConfiguration,\
org.springframework.boot.autoconfigure.solr.SolrAutoConfiguration,\
org.springframework.boot.autoconfigure.thymeleaf.ThymeleafAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.TransactionAutoConfiguration,\
org.springframework.boot.autoconfigure.transaction.jta.JtaAutoConfiguration,\
org.springframework.boot.autoconfigure.validation.ValidationAutoConfiguration,\
org.springframework.boot.autoconfigure.web.client.RestTemplateAutoConfiguration,\
org.springframework.boot.autoconfigure.web.embedded.EmbeddedWebServerFactoryCustomizerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.HttpHandlerAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.ReactiveWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.WebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.error.ErrorWebFluxAutoConfiguration,\
org.springframework.boot.autoconfigure.web.reactive.function.client.WebClientAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.ServletWebServerFactoryAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.error.ErrorMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.HttpEncodingAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.MultipartAutoConfiguration,\
org.springframework.boot.autoconfigure.web.servlet.WebMvcAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.reactive.WebSocketReactiveAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketServletAutoConfiguration,\
org.springframework.boot.autoconfigure.websocket.servlet.WebSocketMessagingAutoConfiguration,\
org.springframework.boot.autoconfigure.webservices.WebServicesAutoConfiguration
```

#### @Condition条件化注解

1、@Conditional派生注解

|@Conditional派生注解|作用（判断是否满足当前指定条件）|
|--|--|
|@ConditionalOnJava|系统的java版本是否符合要求|
|@ConditionalOnBean|容器中存在指定Bean|
|@ConditionalOnMissBean|容器中不存在指定Bean|
|@ConditionalOnExpression|满足spEL表达式|
|@ConditionalOnClass|系统中有指定的类|
|@ConditionalOnMissClass|系统中没有指定的类|
|@ConditionalOnSingleCandidate|容器中只有一个指定的Bean,或者这个Bean是首选Bean|
|@ConditionalOnProperty|系统中指定的属性是否有指定的值|
|@ConditionalOnResource|类路径下是否存在指定的资源文件|
|@ConditionalOnWebApplication|当前是web环境|
|@ConditionalOnNotWebApplication|当前不是web环境|
|@ConditionalOnJndi|JNDI存在指定项|

#### 总结

1. SpringBoot启动会加载大量的自动配置类
2. 我们看我们需要的功能有没有SpringBoot默认写好的默认配置类；
3. 如果有在看这个自动配置类中配置了哪些组件；（只要我们要用的组件有，我们需要再来配置）
4. 给容器中自动配置添加组件的时候，会从properties类中获取属性。我们就可以在配置文件中指定这些属性的值
5. 我们可以通过启用`debug=true`属性，配置文件，打印自动配合报告，这样就可以知道自动配置类生效

```properties
debug=true
```

## 日志框架

### 常用的日志框架

市面上的日志框架； JUL、JCL、Jboss-logging、logback、log4j、log4j2、slf4j…、

|日志抽象层|日志实现|
|--|--|
|~~JCL(Jakarta Commons Logging)~~ SLF4j(Simple Logging Facade for Java) ~~jboss-logging~~|Log4j ~~JUL(java.util.logging)~~ Log4j2 Logback|

在大部分开发中都使用**slf4j**作为日志框架，下面简单介绍使用方法

### slf4j使用方法

以后开发的时候，日志记录方法的调用，不应该来直接调用日志的实现类，而是调用日志抽象层里面的方法； 给系统里面导入slf4j的jar和 logback的实现jar

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class HelloWorld {
  public static void main(String[] args) {
    Logger logger = LoggerFactory.getLogger(HelloWorld.class);
    logger.info("Hello World");
  }
}
```

每个日志框架的实现框架都有自己的配置文件。使用slf4j以后，**配置文件还是做成日志实现框架本身的配置文件**；

## Web开发

### SpringMVC

Spring MVC（Model-View-Controller）是 Spring 框架用于构建 Web 应用程序的一部分。它通过将应用程序分解为模型、视图和控制器来实现松耦合的架构。在 Spring Boot 中，默认情况下会自动配置 Spring MVC，您只需要编写相应的控制器即可。

* **模型（Model）**：代表应用程序的数据和业务逻辑。通常使用 POJO（Plain Old Java Object）表示。
* **视图（View）**：负责渲染模型的数据，生成最终的 HTML、JSON 等响应。
* **控制器（Controller）**：处理用户请求，调用业务逻辑并返回合适的视图或数据响应。

更多内容请**详见SpringMVC文档**

### 前端页面整合

SpringBoot也可以将前后端整合到一起，但目前前后端分离是大趋势。说一在此简单讲下，我们只聊SpringBoot在后端MVC的开发

如果需要构建动态的 HTML 视图，可以集成模板引擎。Spring Boot 默认支持多种模板引擎，其中 Thymeleaf 是一种流行的选择。

* 添加 Thymeleaf 依赖到项目中。
* 创建 Thymeleaf 模板文件（位于 `src/main/resources/templates` 目录）。
* 在控制器中返回模型数据，Thymeleaf 会将数据填充到模板中并渲染。

### RESTful API开发

在 Spring Boot 中，您可以轻松地创建和管理 RESTful API。使用 @RestController 注解，您可以将一个类标记为 RESTful 控制器，并在方法上使用 @RequestMapping 或更具体的 HTTP 方法注解来定义请求的处理逻辑和路径。

```java
@RestController
@RequestMapping("/api")
public class ApiController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, World!";
    }

    // 更多方法...
}

```

### 数据绑定

Spring Boot 提供了强大的表单处理机制，使您能够轻松地处理表单数据的提交和验证。

* 在表单对象上使用注解（如 `@ModelAttribute` 和`@Valid`）进行数据绑定和验证。
* 使用 `BindingResult` 对象处理绑定和验证错误。
* 返回适当的视图或数据响应以显示验证错误信息。

### 文件上传和下载

Spring Boot 使文件上传和下载变得简单，您可以处理用户上传的文件，并生成文件下载响应。

* 使用 `@RequestParam("file")` 注解来接收上传的文件。
* 使用 `MultipartFile` 对象来处理文件内容。
* 使用 `ResponseEntity` 来创建文件下载响应。

### 异常处理

在 Web 开发中，异常处理是一个重要的方面。Spring Boot 允许您定义全局异常处理器，以处理应用程序中的异常并返回合适的响应。

* 创建一个异常处理类，并在方法上使用 `@ExceptionHandler` 注解来处理特定类型的异常。
* 使用自定义异常类来标识不同的异常情况。
* 返回适当的错误响应，如 JSON 格式的错误信息。

### 安全性和认证

最近流行Spring Shiro，**详见Spring Shiro文档**

Spring Boot 通过集成 Spring Security 来提供安全性和认证支持。您可以配置安全规则、定义用户角色、进行用户认证等。
