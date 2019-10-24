---
layout: post
---
- spring batch

- 命名空间

        <beans xmlns="http://www.springframework.org/schema/beans"
	    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	    xmlns:batch="http://www.springframework.org/schema/batch"
	    xsi:schemaLocation="http://www.springframework.org/schema/beans 
	    http://www.springframework.org/schema/beans/spring-beans.xsd
	    http://www.springframework.org/schema/batch 
	    http://www.springframework.org/schema/batch/spring-batch.xsd">
	    
- 表

    - batch_job_instance
    - batch_job_execution
    - batch_job_execution_params
    - batch_job_execution_context
    - batch_job_seq
    - batch_job_execution_seq
    - batch_step_execution
    - batch_step_execution_context
    - batch_step_execution_seq
   
- job instance,作业实例, 
- jobparameters ,job通过jobparameters来区分不同的job, 

    - 支持的数据类型

        - string
        - long
        - date
        - double
        - jobparametersbuilder 来构建参数

    - org.springframework.batch.core.jobparameters
    - jobparameters 对应的数据库表,batch_job_execution_params

- job execution job执行的句柄, 只有job execution执行完成, job instance 才能执行完成

    - jobexecution 对应的数据库表 batch_job_execution
    - org.springframework.batch.core.jobexecution


- step 作业步, 一个job是由一个或者多个step组成

    - step execution step执行的句柄
    - step execution 对应的数据库表 batch_step_execution
    - org.springframework.batch.core.stepexecution

- execution context

    - batch_job_execution_context
    - batch_step_execution_context

- job repository

    - 基于内存的

            <bean id="jobRepository"
		class="org.springframework.batch.core.repository.support.MapJobRepositoryFactoryBean" />
		
    - 基于db的

            <bean id = "dataSource" class = "org.springframework.jdbc.datasource.DriverManagerDataSource">
		    <property name = "driverClassName" value = "com.mysql.jdbc.Driver"/>
		    <property name = "url" value = "jdbc:mysql://localhost:3306/test"/>
		    <property name = "username" value = "root"/>
		    <property name = "password" value = "123456"/>
	        </bean>
	        <bean id = "transactionManager" class = "org.springframework.jdbc.datasource.DataSourceTransactionManager">
		    <property name = "dataSource" ref = "dataSource"/>
	    </bean>

	    <batch:job-repository
	    	id = "jobRepository" 
	    	data-source = "dataSource" 
	    	transaction-manager = "transactionManager"
	    	isolation-level-for-create = "SERIALIZABLE"
	    	table-prefix = "BATCH_"
	    	max-varchar-length = "2500"
	    />
	    
- job launcher 作业调度器
- itemreader, step中对资源的读处理
- itemprocessor step中对读取的数据的处理, 用户可自己实现
- itemwriter 写资源

- job

    - 重启job , 添加属性restartable = "true", 需要注意jobinstance状态不是complied
    - job 拦截器, 实现jobexecutionlistener, 并配置在job元素中,
    - parameters校验

            <bean id = "validator" class = "org.springframework.batch.core.job.DefaultJobParametersValidator">
    		<property name = "requiredKeys">
    			<list>
    				<value>date</value>
    				<value>createtime</value>
    			</list>
    		</property>
    		<property name = "optionalKeys">
    			<set>
    				<value>name</value>
    				<value>hahaha</value>
    			</set>
    		</property>
    	</bean>
    	
    - job 抽象与继承, abstract属性和parent属性
    - step scope , 通过scope属性设定这些bean的生命周期,将bean和step绑定, 并注册到spring上下文中,
    - 属性后绑定late binding,"#{jobParameters['inputResource']}"
    - job launcher 作业运行, 通过名称和参数执行
    - job explore 作业状态查询类, 返回作业状态的复杂对象
    - job operator 作业状态查询,作业执行类, 返回简单对象
    - launcher的run方法是同步执行作业,
    - 异步launcher, 在joblauncher中加入taskexecutor属性,
    - 命令行执行

        - java -classpath "spring-batch-0.0.1-jar-with-dependencies.jar"
  			    org.springframework.org.batch.core.launch.support.CommandLineJobRunner
  			    ch02/job/job.xml
  			    billJob
  			    
        - java -classpath "xxx" CommandLineJobRunner jobpath <options> jobName <jobparameters>
  			<options>
  				-restart 重新执行
  				-stop 停止正在执行的任务
  				-abandon 废弃stop的任务
  				-next 根据JobParametersincrement 执行下一个任务
  				
         - jobparameters 

            - createtime(date)=2019/09/12

    - 定时调度

            ```java
            <task:schedule id = "scheduler" pool-size = "10">
            <task:scheduled-tasks schedule = "scheduler">
                <task:scheduled ref = "schedulerLauncher" method = "launch" fixed-rate = 
"1000">
            </>
            <schedulerlauncher>
                <job = "">
                <joblauncher = "">
                
            ```
            
    - 停止job

            <bean id = "jobOperator" class = "org.spr    ingframework.batch.core.launch.support.SimpleJobOperator">
		    <property name="jobRepository" ref = "jobRepository"></property>
		    <property name="jobRegistry" ref = "jobRegistry"></property>
		    <property name="jobLauncher" ref = "jobLauncher"></property>
		    <property name="jobExplorer" ref = "jobExplorer"></property>
	    </bean>
	
- 配置step

    - step抽象与继承
    - step执行拦截器, org.springframework.batch.core.stepexecutionlistener

        - 拦截器处理异常,否则为failed
        - 顺序执行
        - @beforestep
        - @afterstep
        - 通过meger属性, 可以合并父step的拦截器

- 配置tasklet, step的子元素

    - 重启step

        - start-limit 属性, step启动次数
        - allow-start-if-complete = "true"执行完成之后可以重启

    - 事务

        - transaction-manager = ""属性
        - transaction-attributes子属性    isolation = 隔离级别 propagation = 传播方式 timeout
    - 事务回滚, no-rollback-exception-classes子元素, 配置事务碰见那些异常,不会滚
    - 多线程step, task-executor属性指定, org.springframework.scheduling.concurrent.threadpooltaskexecutor
    - 自定义tasklet, org.springframework.batch.core.step.tasklet.tasklet
    - org.springframework.batch.core.step.tasklet.methodinvokingtaskletadater, 通过代理方法,

- 配置chunk

    - 提交间隔,commit-interval, 处理多少条提交一次
    - 异常跳过, skippable-exception-classes子元素
    - step重试, retrylistener, retry-limit
    - chunk完成策略, 

- itemreader
    - org.springframework.batch.item.file.flatfileitemreader
    - flat 文件格式

        - 分隔符类型文件
        - 定长类型文件
        - json格式文件
        - 记录跨多行文件
    - 属性

        - resource, 读取文件
        - linesToSkip, 跳过开始的几行
        - skippedLinesCallback, 跳过开始的几行之后, 需要执行什么回调操作,
        - recordSeparatorPolicy, 义文件如何区分记录,是按照当行还是多行, 默认使用的是simplerecordseparatorpolicy, 可以使用下面定义的multRecordSeparatorPolicy 定义多行读取记录

            - simplerecordseparatorpolicy, 单行
            - defaultrecordseparatorpolicy, 
            - suffixrecord..., 后缀
            - jsonrecord..., json文件行分割策略
        - lineMapper , 将一行记录转化为java对象
            
            
            - lineTokenizer

                - delimitedlinetokenizer
                
            - fieldSetMapper

                - beanwrapperfieldsetmapper
                - 自定义filedsetmapper

    - 读定长文件

        - linemapper =  org.springframework.batch.item.file.mapping.defaultlinemapper
        - linetokenizer = org.springframework.batch.item.file.transform.fixedlengthtokenizer
    - 读json文件

        - recordseparatorpolicy = org.srpingframework.batch.item.file.separator.jsonrecordseparatorpolicy
    - 读多行

        - recordseparatorpolicy = multRecordSeparatorPolicy

    - 混合记录

        - linemapper = org.springframework.batch.item.file.mapping.patternmatchingcompositelinemapper
        - tokenzers

    - xml

        ```xml
        <bean id = "csvItemReader" class = "org.springframework.batch.item.xml.StaxEventItemReader">
        //将节点credit转换为java对象
		<property name="fragmentRootElementName" value = "credit"/>
		<property name = "unmarshaller" ref = "creditMarshaller" />
		<property name="resource" value = "classpath:ch02/data/credit-card-201905.xml"></property>
	</bean>
	
    //序列化组件
	<bean id = "creditMarshaller" class = "org.springframework.oxm.xstream.XStreamMarshaller">
		<property name="aliases" >
			<map>
				<entry key = "credit" value = "com.sun.bean.CreditBill"></entry>
			</map>
		</property>
	
	</bean>
	
	```
	
    - 读多文件

        ```xml
       <bean id = "multItemReader" class = "org.springframework.batch.item.file.MultiResourceItemReader">
		<property name = "resources" value = "classpath:ch02/data/credit-card-*.xml"></property>
		<property name = "delegate" ref = "csvItemReader"></property>
	    </bean>
        ```
        - resources, 表示读取的文件
        - delegate, 处理每个文件的itemreader

    - jdbc游标读取

        - org.springframework.batch.item.database.jdbccursoritemreader
        - 属性

            - datasource
            - sql
            - rowmapper, 可以自定义或者使用org.springframework.jdbc.core.beanpropertyrowmapper
            - fetchsize ,每次取多少数据
            - querytimeout, 查询超时事件
            - preparedstatementsetter, 设置sql中的参数,org.springframework.batch.core.resource.listpreparedstatementsetter,有一个parameters属性,
    - jdbc分页读取

        - org.springframework.batch.item.database.jdbcpagingitemreader

            - datasource, 数据源
            - queryProvider,

                - org.springframework.batch.item.database.support.sqlpagingqueryproviderfactorybean
                - datasource
                - selectClause
                - fromClause
                - whereClause
                - sortKey
            - parameterValues,参数, 使用map类型
            - pagesize, 每一页的大小
            - rowmapper,将结果集转化为pojo


    - reader服用
- itemwriter

    - flatfileitemwriter

        - org.springframework.batch.item.file.flatfileitemwriter

            - resource
            - lineAggregator

                - delimiter,分隔符
                - fieldExtractor
                    - org.springframework.batch.item.file.transform.beanwrapperFieldextractor

                    - names:

    - 自定义lineaggregator, 实现org.springframework.batch.item.file.transform.ExtractorLineAggregator
    - 回调操作

        - org.springframework.batch.item.file.flatfileheadercallback
        - org.springframework.batch.item.file.faltfilefootercallback
        - 配置, 在writer中分别配置属性

            - headerCallback
            - footerCallback

    - xml文件写

        - org.springframework.batch.item.xml.staxeventitemwriter

            - rootTagName, 根节点名称
            - marshaller, 序列化
            - resource, 

    - 写多文件

    - 写数据库


- itemprocess

    - 数据过滤, 实现itemprocessor, 不合适的返回null
    - 数据过滤统计, stepexecution.getfiltercount(),stepexecution.getskipcount(),通过step拦截器,可以获取到stepexecution
    - 数据校验

        - org.springframework.batch.item.validator.validatingitemprocessor
        - 属性,filter 是否过滤, validator, 实现了validator接口的过滤器
    - 组合处理器

        - org.springframework.batch.item.support.CompositeItemProcessor
        - 属性, delegates, list类型,注入处理器
    - 
- step flow

    - 顺序flow, 在job中定义多个step,按照定义的顺序执行, next元素
    - 条件flow, <next on = "" to = "">, on 条件,或者返回错误码, to代表下一步
    - decision条件,需要实现org.springframwork.batch.core.job.flow.jobexecutiondecider

        - return flowexecutionstatus("");
        - <next on = "" to = ""> on中的是返回的状态码

    - 并行flow, split元素,<split id = "split" task-executor = "" next = "下一个step">
    - 外部flow,<flow>元素中包含step
    - 外部flow的引用,<flow id = "" parent = "外部flow的id" next= "下一个作业步">
    - flowstep,step中使用flow, 此step包含flow中定义的step,但是被包成了一个大的step

        - <flow parent = "外部定义的flow">
    - jobstep, 在step中使用外部定义的job

        - <job ref = "外部定义的job">

    - step共享数据,通过executioncontext上下文中的key/value传递数据

        - chunkcontext().getstepcontext().getstepexecution().getexecutioncontext().
    - 终止job

        - <end on = "" exit-code = "">
        - <stop on = "" restart = "下一次重启从哪一步开始">
        - <fail on = "" exit-code = "">

- 健壮job

    - skippolicy, 跳过策略, 实现org.springframework.batch.core.step.skip.skipPolicy,同时在chunk中配置skip-policy
    - 跳过拦截器,skiplistener
    - 重试

        - retry-listeners, retryable-exception-classes
        - retry-policy, 

    - 重启job,

        - 只能重启状态为失败的job
        - 重新执行job会从上次的端点开始执行,
        - 已经完成的step, 通过特殊的表示也可以重新执行
        - 属性

            - restartable, 是否可重启
            - allow-start-ifcomplete, 如果完成,是否可重启
            - start-limit, 最大重启限制