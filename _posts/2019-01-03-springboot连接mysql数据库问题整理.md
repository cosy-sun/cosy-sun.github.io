
---
layout: post
---
mysql

- jdbc

        driver:com.mysql.jdbc.Driver
        url:jdbc:mysql://localhost:3306/���ݿ�?user=root&password=123456&useUnicode=true&characterEncoding=utf-8
 �������:
 
        user ���ݿ��û���
        password ����
        useUnicode �Ƿ�ʹ��Unicode�ַ���
        autoReconnect �Ƿ��Զ���������
        autoReconnectForPools �Ƿ�ʹ��������ݿ����ӳص���������
        maxReconnects autoReconnect����Ϊtrueʱ,�������ӵĴ���
        failOverReadOnly �Զ������ɹ���,�����Ƿ�����Ϊֻ��
        connectTimeout �����ݿ����������socket����ʱ�ĳ�ʱ,��λ:����
        

     springboot����jdbc
     
        spring.datasource.driver-class-name
        spring.datasource.url
        spring.datasource.username
        spring.datasource.password
     
x
    ��Ҫʹ��data-username��data-password
        
        data-username ����ִ��dml�ű������ݿ���û���
        data-password ����ִ��dml�ű������ݿ������
        

