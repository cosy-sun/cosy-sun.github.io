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
        
- springboot特定的条件化注解

        @ConditionalOnBean 配置了某个特定的Bean
        @ConditionalOnMissingBean 没有配置某个特定的Bean
        @ConditionalOnClass  classpath里面有指定的类
        @ConditionalOnMissingClass classpath里缺少指定的类
        @ConditionalOnExpression 给定的spel表达式的值是true
        @ConditionalOnJava java的版本匹配指定的值或者一个范围的值
        @ConditionalOnJndi 
        @ConditionalOnProperty 给定的配置属性要有一个明确的值
        @ConditionalOnResource classpath里有有指定的资源
        @ConditionalOnWebApplication 指定这是一个web应用程序
        @ConditionalOnNotwebApplication 指定这不是一个web应用程序
        