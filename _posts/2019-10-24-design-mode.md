---
layout: post
---
####设计模式

- 原则

    1. 单一职责
    2. 开闭原则, 开放扩展, 关闭修改,
    3. 里氏替换, 
    4. 依赖倒置, 调用者和被调用者都依赖抽象, 这样具体实现类如何修改, 不会影响,
    5. 接口隔离, 
    6. 最少知道, 
    7. 合成/聚合复用,
- 分类

    - 创建模型

        单例, 工厂方法, 抽象工厂, 静态工厂, 建造者模式, 原型模式, 
    - 结构性

        适配器, 桥接, 装饰, 组合, 外观, 享元, 代理, 
        
    - 行为型

        模板, 命令, 迭代器, 观察者, 中介者, 备忘录, 解释器, 状态模式, 策略, 责任链, 访问者,

- 模式
 
1. 静态工厂模式

2. 工厂方法模式

3. 抽象工厂模式

4. 单例模式(singleton)

    1. 饿汉式,`private static final singleton instance = new singleton();`
    2. 懒汉式,`if(instance == null) instance = new singleton();`
    
    在多个虚拟机里面, 会在每个虚拟机下面创建一个实例, 因此在分布式环境下, 应该避免使用,
    
    在多个类加载器中, 单例模式在多个类加载器中会存在不同的实例, 
    
    使用饿汉式单例模式可以避免同步带来的死锁, 
    
5. 建造模式(builder)

6. 原型模型(prototype)

    通过clone来实现的,clone来实现对象的复制, 动态的抽取当前工作对象的运行机制的状态并克隆到新的对象中, 
    
    ###深拷贝, 基本数据类型值传递, 引用数据类型, 创建一个新的对象,
    ```
        //可以进行多次克隆, 或者利用序列化技术,
        public Object clone() {
		
		ByteArrayOutputStream baos = new ByteArrayOutputStream();
		ObjectOutputStream oos;
		Object readObject = null;
		byte[] b = null;
		try {
			oos = new ObjectOutputStream(baos);
			oos.writeObject(this);
			ObjectInputStream ois = new ObjectInputStream(new ByteArrayInputStream(baos.toByteArray()));
			readObject = ois.readObject();
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		return readObject;
		
	}
    ```
    ###浅拷贝, 引用数据类型引用传递,
    ```
    public void clone() {
        super.clone();
    }
    ```
    
7. 适配器模式(adapter)

    1. 类适配器

        customer类, news接口, linuxNews继承customer实现news, 
    1. 对象适配器

        loginEvent抽象类, 直接对此抽象类实现特殊的,
        
8. 桥梁模式(bridge)

    将抽象和实现分离,
    
9. 外观模式(facade)

    对客户屏蔽子系统组件,
1. 组合模式(composite)

    将对象一属性结构组织起来,以达成'整体- 部分'的层次结构,是的客户端对单个对象和组合对象的使用具有一致性,
    
    用户不必关系自己处理的是那种对象, 都使用相同的方法,

1. 装饰模式(decorator)

    在原有的功能上添加功能,

1. 代理模式(proxy)

    查看cglib代理和jdk动态代理

1. 享元模式(flyweight)

    

1. 命令模式(command)

1. 解释器模式(interpreter)

1. 状态模式(state)

    在状态较多的情况下, 对对象的状态进行集中管理, 通俗点, 就是在一个状态管理类中,注入需要状态管理的对象, 对对象进行操作,

1. 策略模式(strategy)

    将一个操作分布在一组相关的类中,
1. 模板模式(template)

    定义一个模板, 抽象类, 减轻子类的负担, 类如resttemplate, 有许多默认实现
    
1. 备忘录模式

    发起者, 管理者, 备忘录bean, 类似与那种需要备份的东西, 在之后可以进行恢复

1. 观察者模式

    发布订阅模式,java.util.observable, java.util.observer

1. 责任链模式

   

1. 中介者模式

    所有用户都持有一个中间者, 

1. 访问者模式

    