<center><font size="5">spring boot</center>

- �ų���������

        <depenency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <exclusions>
                <exclusion>
                    <groupId>com.fasterxml.jackon.core\</exclusion>
            <exclusions>
        <dependency>
- ��������汾������(maven����ʹ�����������)

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.4.3</version>
        <dependency>
        
- springboot�ض���������ע��

        @ConditionalOnBean ������ĳ���ض���Bean
        @ConditionalOnMissingBean û������ĳ���ض���Bean
        @ConditionalOnClass  classpath������ָ������
        @ConditionalOnMissingClass classpath��ȱ��ָ������
        @ConditionalOnExpression ������spel���ʽ��ֵ��true
        @ConditionalOnJava java�İ汾ƥ��ָ����ֵ����һ����Χ��ֵ
        @ConditionalOnJndi 
        @ConditionalOnProperty ��������������Ҫ��һ����ȷ��ֵ
        @ConditionalOnResource classpath������ָ������Դ
        @ConditionalOnWebApplication ָ������һ��webӦ�ó���
        @ConditionalOnNotwebApplication ָ���ⲻ��һ��webӦ�ó���
        