---
layout: post
---
spring data 

- spring data jpa

    spring data 子项目
         
         spring data commons
         spring data gemfire
         spring data jpa
         spring data keyvalue
         spring data ldap
         spring data mongodb
         spring data rest
         spring data redis
         
    spring data jpa 顶层类 repository
    
七个reposiroty接口
        
        org.springframework.data.repository.Repository
        org.springframework.data.repository.CrudReposritory
        org.springframework.data.reposiroty.PagingAndSortingRepository
        org.springframework.data.repository.query.QueryByExampleExector
        org.springframeowrk.data.jpa.repository.JpaRepository
        org.springframework.data.jps.repository.JpaSpecificationExecutor
        org.springframework.data.data.querydsl.QueryDslPredicateExecutor
        两个实现类
        org.springframework.data.jpa.reposiroty.support.SimpleJpaRepository, 是jpa整个关联数据库的所有reposiroty的接口实现类,
        org.springframework.data.jpa.repository.support.QueryDslJpaRepository
    
定义查询方法的配置方法

        spirng jpa repository是通过动态代理实现的,

方法的查询策略的设置

    @EnableJpaRepositories(queryLookupStrategy=QueryLookupStrategy.Key.(..))
    key值一共有三个
    create:直接根据方法名创建, 如果方法名不符合规则, 那么会抛出异常,
    use_declared_query:声名方式创建, 也就是注解是方式, 如果没有找到则抛出异常,
    create_if_not_found:上面两种的结合版,先使用声明式查找,若没有找到就使用方法名创建.
    
查询方法的创建

    find...By
    read...By
    query...By
    count...By
    get...By
    delete...By
    remove...By
    
    
关键字

    and      findByNameAndEmail
    Or       findByNameOrEmail
    between  findByDateBetween(x1, x2)
    after    findByDateAfter > date
    before   find...before   < date
    isNull   find...isNull   is null
    like
    startWith
    endingWith
    

    
查询结果的处理

    在查询方法中使用特定类型的参数,可以达到分页或者排序的目的,例如:Pageable
    Page<User> findByLastname(String lastname, pageable page);
    List<User> findByLastname(String lastname, sort sort);
    
    
限制查询结果

    可以在方法名上加上关键字first或者top
    list<user> findFirst10ByLstname();
    
查询结果的不同形式

    page
    list       
    stream     stream.foreach()
    future     @Async
    
注解查询大全

<font size = "5"><b>一定注意jpql中的实体不是数据库中的表,而是自己创建的实体类,时区分大小写的</font></b>

    @Query , 不使用value,直接添加sql是jpql的sql, 或者使用value,
    value 指定jpql的查询语法
    countQuery 指定count的jpql语句, *select count(1) from ...*
    nativeQuery 默认false,表示value里面是不是原生的sql, 
    nativequery 不支持直接sort的参数查询
    如果要支持排序, 应该直接在value的sql中添加order by ?2

    @Query排序
    直接使用jpql,在参数中加入sort, 不适用nativequery,
    

    @Query分页
    直接用page对象接受接口,在参数中加入pageable
    
@param
    
    在方法的参数上加上@param("字段名") String name
    在查询语句中使用参数时,:参数名
    
```
	@Query(value = "select u from User u where u.name = :name")
	User findByName2(@Param("name") String name);
```

@Query注解中使用spel表达式
<table>
<th>变量名</th><th>使用方式</th><th>描述</th><tr><td>entityName</td><td>select x from  #{#entityName} x</td><td>根据指定的repository插入相关的entityName(1)如果定义了entity注解,直接使用entity属性名,(2)如果没有定义,使用实体类的名称
</td></tr></table>

@Modifying修改查询

    clearAutomatically(), 其中的方法, 会刷新一级缓存,
    modifying在修改或者删除的时候必须使用事务的注解,@transactional

@procedure存储过程

	参数:
	value 数据库里中储存过程的名称
	procedureName数据库中储存过程的名称 
	name jpa中储存过程的名称
	outputParameterName 输出参数的名字
	
    @NamedStoredProcedureQuery标注在实体类上,表示其是一个存储过程

    在这个注解中需要使用@StoredProcedureParameter注解来的定义在存储过程中定义的输入输出参数,在此注解中的参数为mode , type, name
    @Entity
    @NamedStoredProcedureQuery(name = "Procedure.pluslinout",
		procedureName = "pluslinout", parameters = {
				@StoredProcedureParameter(mode = ParameterMode.IN,name = "arg", type = Integer.class),
				@StoredProcedureParameter(mode = ParameterMode.OUT,name = "res", type = Integer.class)
		})

@NamedQuery

	定义在实体类上,预定义的查询
	name 规则 实体名.方法名
	query jpql语法
	
	调用的时候 直接使用方法调用
	
	repository中不需要使用注解,直接定义方法
	
    @qeury> 方法定义查询 > @namedquery
	

基本注解

	@Entity	被此注解注释的对象将会成为jpa管理的实体,将映射到指定的数据库表
	@Table 指定数据库中的表名
	    @Table(name = "Table_name" schema = "db")
	@Id 数据库中的主键 , 一个实体里面必须有一个主键 
	@GeneratedValue 主键生成策略(generation.class)
		Generation.table 通过表生成主键
		sequence 通过序列生成主键
		identity 采用数据库id自增长
		auto自动选择合适的策略 
	@IdClass 利用外部类的联合主键
		作为联合主键,需要满足一下条件
		必须实现serializable接口
		必须    有默认的 public无参数的构造方法
		必须覆盖equals和hashcode方法
		idcalss注解用在entity类上
		同时需要在entity上标注一下@id
	@Basic 数据库表中字段的映射,如果实体类中属性上没有注解,默认是@basic
	@Transient 表示此字段不是一个数据库库字段,与@basic相反,JPA映射的时候忽略,
	@Colum 对应数据库中的列名
		在repository接口中需要使用实体的名字去查询,应为定义了column名字,所以需要这个字段,然后通过注解去响应的数据库查询相应的字段
	@Temporal 设置data类型的属性,(TemporalType.date/time/timestamp)
	@Enumerated 参数EnumType.ORDINAL(映射枚举的下标)或者STRING(映射枚举的name), 同时需要创建一个枚举类,作为映射参数,
	@Lob 将属性映射成数据库支持的大对象类型,一般配合@basic(fetch = fetchtype.lazy)将其设置为延迟加载,
	

关联关系注解

	@JoinColumn 定义外键关联的字段名称 
		name 目标表的字段名 必填
		referenceedColumnName 本实体的字段名歌,非必填,默认是本表的Id
		unique 外键字段是否唯一
		nullable 外键字段是否为空
		insertable 是否跟随一起新增
		updateable 是否跟随一起新增
	@OneToOne 一对一
		targetEntity 关系目标实体,非必填
		cascade 级联操作策略
			cascadeType.persist 级联新建
			cascadeType.REMOVE 级联删除
			cascadeType.pefresh 级联刷新
			cascadeType.MERGE 级联更新
			cascadeType.ALL 四项全选
			默认,关系表不会产生任何影响
		fetch 数据获取方式,eager(立即加载)/LAZY延迟加载
		optional 是否允许为空
		mappedBy  不能与@Joincolumn 或者@JoinTable联合使用
			mappedBy指定的是另外一个添加了@joinColumn或者@JoinTable注解的属性的字段名称,不是数据库字段,也不是实体的对象的名字.
			
		orphanRemoval 是否级联删除,和cascadeType.REMOVE效果是一样的,配置一个就可以了.			

	@OneToMany 一对多
	@OrderBy 关联查询时排序
		在onetomany的时候使用居多,可以对查询到的实体进行排序,规则:property|database_column_name   desc|asc
		
	@JoinTable  关联关系表
		
	@ManyToMany 多对多
	
EntityManager中常用的方法

```
	//根据主键查询实体对象
	find(Class<T> entityClass, Object primarykey)
	
	//将persistenceContext的信息同步到数据库中
	flush
	
	//refresh的作用是从数据库中将entity的状态进行更新操作,如果entity和数据库中的数据不一致,将更新数据库中的数据到entity中
	refresh
	
	//添加实体到数据库中
	persist(实体名)
	
	//删除(要先查询)
	remove()
	
	//更新
	merge()
	
	//查询支持jqpl的语法
	createQuery()
	
```

可以自定义自己的 Repository 

	@NoRepositoryBean 声明定制共享行为的接口
	再jpa中提供的repository已经实现了大部分的功能,如果自己实现的话,可以继承这些已经实现的方法,可以减少工作量,同时在实现类中需要注意的是必须实现父类的构造方法,同时整个类需要T类型的 class类型,所以需要在构造方法中传入class类型.
	
	也可以完全自己实现带泛型的repository
	注意在类中声明最简单的两个变量class<T>以及entityManager
	在entityManager上使用注解@PersistenceContext实现容器管理的entitymanager,由于是容器管理的, 所以我们不需要也不应该显示关闭注入的entitymanager,
	
auditing（审计）

	entity实体类中需要添加注解
		@CreatedBy 创建者
		@CreatedDate 创建日期
		@LastModifiedDate 最后修改日期
		@LastModifiedBy 最后修改者
		
	在实体类上添加@EntityListeners（AuditingEntityListeners.class）

	实现AuditorAware接口告诉jpa是谁在修改或者创建数据
	启动auditing, @enablejpaauding(auditorAwareRef = "auding实现类")
	
@MappedSupperclass

	相当与添加了注解entity的实体的父类，可以在这个父类中添加公共的方法
	
   

自定义entityListener

    通过在方法上添加这些注解, 可以监听一些东西, 然后将监听器注解在@entitylisteners中,
    @postpersist, 当更新的时候做一些逻辑判断, 及事件通知
    @postremove, 当删除的时候
    @postupdate, 当更新的时候,

@version, 通过aop机制, 在更新的时候会使用version字段, 
    
    乐观锁
    悲观锁
    
@enablejparepositories("包名")
    
	springboot中会帮我们自动添加,
	