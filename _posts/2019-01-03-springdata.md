spring data 

- spring data jpa

    spring data ����Ŀ
         
         spring data commons
         spring data gemfire
         spring data jpa
         spring data keyvalue
         spring data ldap
         spring data mongodb
         spring data rest
         spring data redis
         
    spring data ������ repository
    
�����Ĳ�ѯ���Ե�����

    @EnableJpaRepositories(queryLookupStrategy=QueryLookupStrategy.Key.(..))
    keyֵһ��������
    create:ֱ�Ӹ��ݷ���������
    use_declared_query:������ʽ����
    create_if_not_found:�������ֵĽ�ϰ�,��ʹ������ʽ����,��û���ҵ���ʹ�÷���������.
    
��ѯ�����Ĵ���

    find...By
    read...By
    query...By
    count...By
    get...By
    delete...By
    remove...By
    
    
�ؼ���

    and    findByNameAndEmail
    Or    findByNameOrEmail
    ...
    
��ѯ����Ĵ���

    �ڲ�ѯ������ʹ���ض����͵Ĳ���,���Դﵽ��ҳ���������Ŀ��,����:Pageable
    
���Ʋ�ѯ���

    �����ڷ������ϼ��Ϲؼ���first����top
    
��ѯ����Ĳ�ͬ��ʽ

    page
    list
    stream
    future
    
ע���ѯ��ȫ

<font size = "5"><b>һ��ע��jpql�е�ʵ�岻�����ݿ��еı�,�����Լ�������ʵ����,ʱ���ִ�Сд��</font></b>

    @Query 
    value ָ��jpql�Ĳ�ѯ�﷨
    countQuery ָ��count��jpql���
    nativeQuery Ĭ��ʱfalse,��ʾvalue�����ǲ���ԭ����sql
    

    @Query����
    ֱ��ʹ��jpql,�ڲ����м���sort
    

    @Query��ҳ
    ֱ����page������ܽӿ�,�ڲ����м���pageable
    
@param
    
    �ڷ����Ĳ����ϼ���@param("�ֶ���") String name
    �ڲ�ѯ�����ʹ�ò���ʱ,:������
    
```
	@Query(value = "select u from User u where u.name = :name")
	User findByName2(@Param("name") String name);
```

@Queryע����ʹ��spel���ʽ
<table>
<th>������</th><th>ʹ�÷�ʽ</th><th>����</th><tr><td>entityName</td><td>select x from  #{#entityName} x</td><td>����ָ����repository������ص�entityName(1)���������entityע��,ֱ��ʹ��entity������,(2)���û�ж���,ʹ��ʵ���������
</td></tr></table>

@Modifying�޸Ĳ�ѯ

@procedure�洢����

	����:
	value ���ݿ����д�����̵�����
	procedureName���ݿ��д�����̵����� 
	name jpa�д�����̵�����
	outputParameterName �������������
	
@NamedStoredProcedureQuery��ע��ʵ������,��ʾ����һ���洢����

�����ע������Ҫʹ��@StoredProcedureParameterע�����Ķ����ڴ洢�����ж���������������,�ڴ�ע���еĲ���Ϊmode , type, name

@NamedQuery

	������ʵ������,Ԥ����Ĳ�ѯ
	name ���� ʵ����.������
	query jpql�﷨
	
	���õ�ʱ�� ֱ��ʹ�÷�������
	
	repository�в���Ҫʹ��ע��,ֱ�Ӷ��巽��
	

����ע��

	@Entity	����ע��ע�͵Ķ��󽫻��Ϊjpa�����ʵ��,��ӳ�䵽ָ�������ݿ��
	@Table ָ�����ݿ��еı���
	@Id ���ݿ��е����� , һ��ʵ�����������һ������ 
	@GeneratedValue �������ɲ���
		table ͨ������������
		sequence ͨ��������������
		identity �������ݿ�id������
		auto�Զ�ѡ����ʵĲ��� 
	@IdClass �����ⲿ�����������
		��Ϊ��������,��Ҫ����һ������
		����ʵ��serializable�ӿ�
		������Ĭ�ϵ� public�޲����Ĺ��췽��
		���븲��equals��hashcode����
		
		idcalssע������entity����
	@Basic ���ݿ�����ֶε�ӳ��,���ʵ������������û��ע��,Ĭ����@basic
	@Transient ��ʾ���ֶβ���һ�����ݿ���ֶ�,��@basic�෴
	@Colum ��Ӧ���ݿ��е�����
		��repository�ӿ�����Ҫʹ��ʵ�������ȥ��ѯ,ӦΪ������column����,������Ҫ����ֶ�Ȼ��ͨ��ע��ȥ��Ӧ�����ݿ��ѯ��Ӧ���ֶ�
	@Temporal ����data���͵�����,
	@Enumerated ����EnumType.ORDINAL(ӳ��ö�ٵ��±�)����STRING(ӳ��ö�ٵ�name)
	@Lob ������ӳ������ݿ�֧�ֵĴ��������
	

������ϵע��

	@JoinColumn ��������������ֶ����� 
		name Ŀ�����ֶ��� ����
		referenceedColumnName ��ʵ����ֶ�����,�Ǳ���,Ĭ���Ǳ����Id
		unique ����ֶ��Ƿ�Ψһ
		nullable ����ֶ��Ƿ�Ϊ��
		insertable �Ƿ����һ������
		updateable �Ƿ����һ������
	@OneToOne һ��һ
		targetEntity ��ϵĿ��ʵ��,�Ǳ���
		cascade ������������
			cascadeType.persist �����½�
			cascadeType.REMOVE ����ɾ��
			cascadeType.pefresh ����ˢ��
			cascadeType.MERGE ��������
			cascadeType.ALL ����ȫѡ
			Ĭ��,��ϵ��������κ�Ӱ��
		fetch ���ݻ�ȡ��ʽ,eager(��������)/LAZY�ӳټ���
		optional �Ƿ�����Ϊ��
		mappedBy  ������@Joincolumn ����@JoinTable����ʹ��
			mappedByָ����������һ�������@joinColumn����@JoinTableע������Ե��ֶ�����,�������ݿ��ֶ�,Ҳ����ʵ��Ķ��������.
			
		orphanRemoval �Ƿ���ɾ��,��cascadeType.REMOVEЧ����һ����,����һ���Ϳ�����.			

	@OneToMany һ�Զ�
	@OrderBy ������ѯʱ����
		��onetomany��ʱ��ʹ�þӶ�,���ԶԲ�ѯ����ʵ���������,����:property|database_column_name   desc|asc
		
	@JoinTable  ������ϵ��
		
	@ManyToMany ��Զ�
	
EntityManager�г��õķ���

```
	//����������ѯʵ�����
	find(Class<T> entityClass, Object primarykey)
	
	//��persistenceContext����Ϣͬ�������ݿ���
	flush
	
	//refresh�������Ǵ����ݿ��н�entity��״̬���и��²���,���entity�����ݿ��е����ݲ�һ��,���������ݿ��е����ݵ�entity��
	refresh
	
	//���ʵ�嵽���ݿ���
	persist(ʵ����)
	
	//ɾ��(Ҫ�Ȳ�ѯ)
	remove()
	
	//����
	merge()
	
	//��ѯ
	createQuery()
	
```

�����Զ����Լ��� Repository 

	@NoRepositoryBean �������ƹ�����Ϊ�Ľӿ�
	��jpa���ṩ��repository�Ѿ�ʵ���˴󲿷ֵĹ���,����Լ�ʵ�ֵĻ�,���Լ̳���Щ�Ѿ�ʵ�ֵķ���,���Լ��ٹ�����,ͬʱ��ʵ��������Ҫע����Ǳ���ʵ�ָ���Ĺ��췽��,ͬʱ��������ҪT���͵� class����,������Ҫ�ڹ��췽���д���class����.
	
	Ҳ������ȫ�Լ�ʵ�ִ����͵�repository
	ע��������������򵥵���������class<T>�Լ�entityManager
	��entityManager��ʹ��ע��@PersistenceContextʵ�����������entitymanager
	
auditing����ƣ�

	entityʵ��������Ҫ���ע��
		@CreatedBy ������
		@CreatedDate ��������
		@LastModifiedDate ����޸�����
		@LastModifiedBy ����޸���
		
	��ʵ���������@EntityListeners��AuditingEntityListeners.class��

	ʵ��AuditorAware�ӿڸ���jpa��˭���޸Ļ��ߴ�������
	����auditing
	
@MappedSupperclass

	�൱�������ע��entity��ʵ��ĸ��࣬�����������������ӹ����ķ���
	

�Զ���entityListener

	