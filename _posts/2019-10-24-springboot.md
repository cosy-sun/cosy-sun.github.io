
---
layout: post
---
<center><font size="5">spring boot</center>

- 排除传递依赖

        <depenency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>com.fasterxml.jackon.core\</exclusion>
            <exclusions>
        <dependency>
- 覆盖另外版本的依赖(maven总是使用最近的依赖)

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.4.3</version>
        <dependency>
- springboot关于自动配置的源码在spring-boot-autoconfiguration-*.jar包内       
- springboot特定的条件化注解

        @ConditionalOnBean 配置了某个特定的Bean/当容器中有指定的bean
        @ConditionalOnMissingBean 没有配置某个特定的Bean/当容器中没有指定的bean
        @ConditionalOnClass  classpath里面有指定的类/当类路径下有指定的class
        @ConditionalOnMissingClass classpath里缺少指定的类/当类路径下没有指定的class
        @ConditionalOnExpression 给定的spel表达式的值是true/基于el表达式作为判断条件
        @ConditionalOnJava java的版本匹配指定的值或者一个范围的值/基于jvm版本作为判断条件
        @ConditionalOnJndi 在jndi存在的条件下查找指定的位置
        @ConditionalOnProperty 给定的配置属性要有一个明确的值/指定的属性是否有指定的值
        @ConditionalOnResource classpath里有有指定的资源/类路径是否有指定的值
        @ConditionalOnWebApplication 指定这是一个web应用程序
        @ConditionalOnNotwebApplication 指定这不是一个web应用程序
        @ConditionalOnSIngleCandidate 当指定bean在容器中只有一个,或者有多个但是指定首选的bean
        
        
- 入口类和@sprngbootapplication

    - 组合了几个注解, 
        - @configuration,
        - @enableautoconfiguration, 根据类路径中的jar包进行自动配置,
        - @componentscan
    - 关闭自动配置, @springbootapplication(exclude = {***.class})
    - 关闭banner, springapplication.setshowbanner(false);
- 使用xml配置, springboot支持零配置, 可以使用*@inportresource({"classpath:some-context.xml", "classpath:some-context.xml"})*加载xml配置文件,
- 属性值配置, 可以在properties中配置, 然后通过@value注解获取*@value("${name}")*
 
- 类型安全的配置,基于properties

    - 添加配置文件,例如,author.properties
    - 创建bean, 同时使用*@configurationproperties(prefix = "properties中的前缀", locations = {"classpath:config/.xml"})*

- 日志配置, 默认情况下springboot使用logback配置

    - logging.file =文件地址
    - logging.level.org.springframework.web = logging.level.包名

- profile配置

    - 通过spring.profile.active  = test指定使用哪一个profile,

- springboot的数据访问

    - spring-data-jpa

        - jdbc的自动配置,通过spring.datasource配置, 自动开启了注解事务, 同时配置了一个jdbctemplate,
        - springboot提供了一个初始化数据的功能, 放置在类路径下的schema.sql文件会自动用来初始化表结构, 放置在类路径下的data.sql文件会自动填充数据
        - 对jpa的自动配置

springboot默认的jpa实现式hibernate,

jpabaseconfiguration还有一个getPackagesToScan方法,可以自动扫描注解有@entity的实体类

springboot为我们自动配置了openEntityManagerInViewInterceptor这个bean,并注册到spirngmvc拦截器中,
    - 声明式事务

        - spring事务机制

通过使用platformtransactionmanager接口, 通过不同的实现来匹配不同的数据库

        - spring声明式事务,*@enabletransactionmanager*开启声明式事务, 会扫描添加了@transaction的注解,

        - springboot的事务支持

1  自动配置的事务管理器,使用jdbc或者jpa, 会自动定义bean,
2  自动开启注解事务的支持, datasourcetransactionmanagerautoconfiguration中添加了@enabletransactionmanagerment
    - 缓存
    
    针对不同的缓存技术, 需要实现不同的cachemanager,
    
    ```
    simplecachemanager 使用简单的collection来实现,主做测试用
    concurrentmapcachemanager, 
    noopcachemanager 不实际缓存
    ehcachecachemanager ehcache
    guavacachemanager
    hazelcastcachemanager
    jcachecachemanager jcache
    rediscachemanager
    ```
        - 声明式缓存, @enablecaching开启缓存,@cacheable,缓存有数据直接返回数据, 没有数据,将返回值填充到缓存中, @cacheput, 无论 怎样都会将返回值填充到缓存, @cacheevict, 删除缓存,
        - springboot支持,

- 安全控制

    - 