
---
layout: post
---
mysql

- jdbc

        driver:com.mysql.jdbc.Driver
        url:jdbc:mysql://localhost:3306/数据库?user=root&password=123456&useUnicode=true&characterEncoding=utf-8
 参数详解:
 
        user 数据库用户名
        password 密码
        useUnicode 是否使用Unicode字符集
        autoReconnect 是否自动重新连接
        autoReconnectForPools 是否使用针对数据库连接池的重连策略
        maxReconnects autoReconnect设置为true时,重试连接的次数
        failOverReadOnly 自动重连成功后,连接是否设置为只读
        connectTimeout 和数据库服务器建立socket连接时的超时,单位:毫秒
        

     springboot配置jdbc
     
        spring.datasource.driver-class-name
        spring.datasource.url
        spring.datasource.username
        spring.datasource.password
     
x
    不要使用data-username和data-password
        
        data-username 用于执行dml脚本的数据库的用户名
        data-password 用于执行dml脚本的数据库的密码
        

