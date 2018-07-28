coderfun
码农翻身笔记


### 2 Java帝国
#### C的缺陷
C语言贴近硬件，运行极快，效率极高，开发了很多系统级软件，操作系统，编译器，数据库，网络系统，但是指针和内存管理极易出错，而且错误在编译期间发现不了，运行时期暴露处理极难调试；C++在图形领域和游戏上取得了很大成功，但其语法太过复杂

#### .Net的缺陷
封闭的微软开发工具是Visual Studio，应用服务器是IIS，数据库是SQL Server，基于.Net开发就会被微软绑架，而且只能运行在Windows服务器上；另外Ruby On Rails结合了PHP快速开发和Java程序规整的优点，特别适合快速开发简单的Web网站

#### Java的优势
为了实现真正的跨平台，Java在操作系统和应用程序间增加了一个抽象层：Java虚拟机，Java程序都运行在虚拟机上，除非个别情况，不用和操作系统交互。Java除了Applet，又开发了针对桌面的J2SE（Swing开发出来的界面非常难看），针对手机的S2ME（移动互联网尚未启动），针对服务器的J2EE（由于互联网大发展，强大，健壮，安全，简单，跨平台）特别适合开发复杂的大型项目

#### Java的诞生
基于Java诞生了大量的平台，系统，工具：  
```
构建工具：Ant, Maven, Jekins
应用服务器：Tomcat, Jetty, JBoss, WebSphere, WebLogic
Web开发：Spring, Hibernate, MyBatis, Struts
开发工具：Eclipse, NetBeans, IntelliJ IDEA, JBuilder
```
#### Hadoop
在大数据领域，使用Java语言，在理解了Map/Reduce，分布式文件系统在Hadoop的实现后，就能很快编写处理海量数据的程序
#### Android
2008年Android横空出世，随着移动互联网的爆发迅速普及，在Google的支持下，以一种意想不到的方式占领了手机端
#### ClassLoader
1. Bootstrap classLoader：采用native code实现，是JVM的一部分，主要加载JVM自身工作需要的类，如java.lang.* 、java.uti.* 等； 这些类位于$JAVA_HOME/jre/lib/rt.jar。Bootstrap ClassLoader不继承自ClassLoader，因为它不是一个普通的Java类，底层由C++编写，已嵌入到了JVM内核当中，当JVM启动后，Bootstrap ClassLoader也随着启动，负责加载完核心类库后，并构造Extension ClassLoader和App ClassLoader类加载器
2. ExtClassLoader：扩展的class loader，加载位于$JAVA_HOME/jre/lib/ext目录下的扩展jar
3. AppClassLoader:系统class loader，父类是ExtClassLoader，加载$CLASSPATH下的目录和jar；它负责加载应用程序主函数类

#### 虚拟机
一个运行中的Java虚拟机有着一个清晰的任务：执行Java程序。程序开始执行时他才运行，程序结束时他就停止。同一台机器上运行三个程序，就会有三个运行中的Java虚拟机。 Java虚拟机总是开始于一个main方法，这个方法必须是公有、返回void、直接受一个字符串数组。main方法是程序的起点，他被执行的线程初始化为程序的初始线程。程序中其他的线程都由他来启动

ava中的线程分为两种：守护线程 （daemon）和普通线程（non-daemon）。守护线程是Java虚拟机自己使用的线程，比如负责垃圾收集的线程就是一个守护线程。当然，你也可以把自己的程序设置为守护线程。包含main方法的初始线程不是守护线程。
只要Java虚拟机中还有普通的线程在执行，Java虚拟机就不会停止。如果有足够的权限，你可以调用exit方法终止程序  

#### 双亲委派机制
JVM在加载类时默认采用的是双亲委派机制。就是某个特定的类加载器在接到加载类的请求时，首先将加载任务委托给父类加载器，依次递归。如果父类加载器可以完成类加载任务，就成功返回；只有父类加载器无法完成此加载任务时，才自己去加载，作用：
1. 避免重复加载
2. 更安全, 如果不是双亲委派，那么用户在自己的classpath编写了一个java.lang.Object的类，那就无法保证Object的唯一性。所以使用双亲委派，即使自己编写了，但是永远都不会被加载运行

#### 破坏双亲委派机制
双亲委派机制并不是一种强制性的约束模型，而是Java设计者推荐给开发者的类加载器实现方式; 线程上下文类加载器，这个类加载器可以通过java.lang.Thread类的setContextClassLoader()方法进行设置，如果创建线程时还未设置，它将会从父线程中继承一个，如果在应用程序的全局范围内都没有设置过的话，那么这个类加载器就是应用程序类加载器。像JDBC就是采用了这种方式。这种行为就是逆向使用了加载器，违背了双亲委派模型的一般性原则

#### 序列化
为了解决断电内存数据丢失，可以将重要的对象转换为二进制文件存储到硬盘上，店里恢复后再反序列化，对象少的时候还好，但是如果要大规模的对Java对象进行存储，查询，效率太低，几乎不能用，此时只能使用关系数据库存储大规模数据

#### 事务
原子性，一致性，隔离性，持久性

#### JDBC
Java Database Connectivity，把Java对象转变成数据库的行/列，即Object-Relational Mapping

#### EJB
JDBC是个非常低级的借口，需要程序员处理太多的细节，打开Connection，创建Statement，执行SQL，遍历ResultSet，记得关闭Connection，还要支持安全、事务、分布式、可伸缩性、高可用性等，这些工作都可有中间件EJB来做，这些EJB实例可以在一个集群上分布式运行；但是其开发繁琐，难以测试，性能低下（后来Gavin进入后带领团队推出了EJB3.0，成功的向Hibernate看齐，有些注解一模一样）

#### 两阶段提交协议
分布式数据库，有一个全局的事务管理器，负责协调各个数据库的事务提交
1. 全局事务管理器向各个数据库发准备消息，各个数据库负责执行操作，锁住资源，记录redo/undo日志，但是不提交，进入时刻准备提交或回滚的状态，然后向全局的事务管理器报告
2. 如果所有数据库都准备好了，全局的事务管理器就发消息，提交，如果任何一个数据库没准备好，就发消息，回滚
但是如果全局的事务管理器出问题，或者事务管理器发的提交消息由于网络问题导致某些数据库没有收到，该如何处理？

#### JTA
Java Transanction API， 分布式事务伴随着大量节点的通信交换，协调者要确定其他节点是否完成，加上网络超时，导致JTA性能低下，在分布式、高并发和要求高性能场景下举步维艰

#### 最终一致性
BASE模型：事务1的发起后，向消息队列插入一条消息，事务即可结束，后台比如定期读取消息队列的消息，再处理事务2，虽然数据某些时候看起来不一致，但是保证最终是一致的，但是数据库和消息队列是不同的东西，可以添加一张事件表，事务1发起的同时向事件表中插入一条记录，定时运行程序从事件表取出记录，向消息队列写入消息，再把记录的状态改成DONE，如果写入消息，还没改状态程序奔溃了，多次写入重复消息，也不要紧，幂等性保证事务2在判断的时候如果已经执行过，就会简单的抛弃重复消息

#### Hibernate
Gavin King开发出一个轻量级的O/R Mapping框架，好像到了冬天让内存的数据进入冬眠， 春天来了再醒来进入内存工作，可以把Java属性用声明的方式映射到数据库表，完全不用操心Connection、SQL这些细节，而且脱离了庞大的、昂贵的WebSphere、WebLogic容器也能用

#### iBatis
另一个O/R Mapping框架

#### Spring
Rod Johnson开发出来的框架集合，不但提供了轻量级的访问数据库的方法JdbcTemplate，还能轻松的集成Hibernate、iBatis等一大批根据，慢慢的成为了事实的标准

#### JPA
Java Persistence API，整合O/R Mapping的标准，Hibernate、EclipseLink、OpenJPA等都提供了针对JPA的实现，可是现在大家似乎又喜欢上了写SQL语句的MyBatis

#### MySQL
最早支持Java的数据库，而且其与Java交互的应用层协议统一了，不管是Java，还是PHP、Ruby、Python，只要能使用Socket，都可以进行访问；但是Oracle、DB2的应用层协议和MySQL完全不一样，这就意味着要切换数据库，就得重新写一套代码；无论是Connection、Statement、ResultSet，都只是借口，让各个数据库自己去实现，Java如果用简单工厂的方式，如果要增加一种实现，就要修改代码，如果用配置文件和反射实现，太麻烦，而且实现类的类名要暴露出来，应该让各个厂商在各自的.jar种去创建，如果用工厂方法的方式，将工厂本身也变成接口，然后再用一个DriverManager将所有厂家的Driver进行添加报保存，各个厂家再自行注册

#### MyBatis
MyBatis 是支持普通 SQL 查询，存储过程和高级映射的优秀持久层框架。 MyBatis 消除了几乎所有的 JDBC 代码和参数的手工设置以及对结果集的检索。 MyBatis 可以使用简单的XML 或注解用于配置和原始映射，将接口和 Java 的 POJO（ Plain Old Java Objects，普通的Java 对象）映射成数据库中的记录, 提供一种“半自动化”的ORM实现,
相对Hibernate等提供了全面的数据库封装机制的“全自动化”ORM实现，即实现了POJO和数据库表之间的映射，以及 SQL 的自动生成和执行。
MyBatis的着力点，则在于POJO与SQL之间的映射关系

#### ASP
Web编程刚开始的时候，只能用Perl、C语言等以CGI的方式来输出HTML，就是单纯的字符串拼接；微软的ASP能够支持在HTML中嵌入代码（主要是VBScript），美工可以用可视化编辑器FrontPage、Dreamweaver把界面创建好

#### JSP
Sun开发的服务器端的装配工之王（使用Java），把页面模板和数据装配起来，变成HTML发送给浏览器, 和APS一样，存在业务逻辑和界面展示无法区分的问题, 必须依赖Web容器如Tomcat才能使用

#### JSTL
Servlet充当控制器，Java类充当模型，JSP充当视图，显示逻辑再做一次封装，即JSTL（JSP Standard Tag Library）

#### Freemaker&Velocity
可以完全脱离Web环境使用，单纯的为了页面展示，无法编写复杂的业务逻辑，不仅可以作为View，还可以定义邮件模板来发邮件，用作代码模板来生成代码

#### AJAX
Asynchronous Javascript And XML: 用于创建快速动态网页, 在无需重新加载整个网页的情况下，能够更新部分网页的技术

#### CSS
层叠样式表：Cascading Style, HTML是网页的骨架，而CSS就是对骨架内容的修饰
, 样式和内容写在一起会显得非常臃肿，使用css可以单独的将样式抽离出来，可以提高开发效率, CSS提供了很多HTML无法完成的显示效果, 抽离出来的CSS可以单独加载，能够实现多个页面的共享，节约网站的带宽，节约成本

#### JavaScript
可以在浏览器中运行的脚本, 前后端分离， 后端只负责提供接口和页面模板，JavaScript则在浏览器端把页面模板和JSON数据装配起来，形成HTML呈现给用户; 从浏览器端发出异步的HTTP调用，基于此发展了很多框架，比如jQuery，可以灵活的在浏览器中操作界面

#### 消息队列
在大型分布式系统中，异步通信是个大问题，消息队列是个好办法，保存在其中的数据需做持久化，以应对断电或者重启；但是各个系统都自定义MQ，又互不兼容，使用极不方便

#### 生产者-消费者模型
Point-to-Point Messaging Domain: 不管生产者向队列发送消息，还是消费者接受消息，都是在和消息队列进行交互；由Connection创建会话Session，Session负责创建消息并引入事务，各种连接参数以及队列名称之类，提供一个ConncectionFactory接口，各个厂商实现此接口，但是这是Point to Point模型，一个发送方对应一个接收方

#### 发布-订阅模型
Publish/Subscribe Messaging Domain: 如果一个客户端对Topic发布了消息，很多订阅了这个Topic的其他客户端就都可以收到这个消息的副本

#### JMS
Java Message Service：设计良好，概念清晰，成为了事实标准; 在JMS API出现之前，大部分产品使用“点对点”和“发布/订阅”中的任一方式来进行消息通讯。JMS定义了这两种消息发送模型的规范，它们相互独立。任何JMS的提供者可以实现其中的一种或两种模型。JMS规范提供了通用接口保证我们基于JMS API编写的程序适用于任何一种模型

#### 动态代理
程序运行时动态生成一个新的类，调用的invoke方法可以拦截真正的方法调用，作为真实对象的代理工作, 这样可以拦截用户的访问请求，以检查用户是否有访问权限、动态为某个对象添加额外的功能
```
public class LoggerHandler implements InvocationHandler {
    private Object target;

    public LoggerHandler(Object target) {
        this.target = target;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Logger.startLog();
        Object result = method.invoke(target, args);
        Looger.endLog();

        return result;
    }
}

Subject realSubject = new RealSubject();
InvocationHandler handler = new LoggerHandler(realSubject);
Subject subject = (Subject)Proxy.newProxyInstance(handler.getClass().getClassLoader(), realSubject.getClass().getInterfaces(), handler);
```

#### 注解
很多人把注解和XML混合使用，对于需要集中配置的场合，还是用XML，对于@Controller, @RequestMapping和Java方法写在一起，显得简单而直观
```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @Interface Test{
    boolean ignore() default false;
}

public class CalculatorTest{
    @Test
    public void testAdd()
    {
        //
    }

    @Test(ignore = true)
    {
        //
    }
}
public class TestUtil
{
    public static void main(String[] args)
    {
        CalculatorTest obj = new CalculatorTest();
        run(obj);
    }

    public static void run(Object obj)
    {
        for (Method m:obj.getClass().getMethods())
        {
            Test t = m.getDeclaredAnnotations(Test.class);
            if (t != null && !t.ignore())
            {
                m.invoke(obj);
            }
        }
    }
}
```

#### 泛型类
C++每次实例化泛型/模板类就生成一个新的类，如模板类是List，分别用int, double, string, Employee去实例化，在编译的时候回生成四个类如List_int，List_double, List_string, List_Employee，系统会变得膨胀；Java中使用擦除法，如ArrayList<T>会被擦除为ArrayList，成了原始的ArrayList类型，而传入的Integer, String等参数变成了Object，在编译的时候加一个自动的转型，如Integer i = (Integer)list.get(0)
```
public class ArrayList implements List{
    public void add(Object e)
    {
        //
    }
    public Object get(int index)
    {
        //
    }
    }
```

#### 泛型方法
```
public static Object max(List list);
public static <T> Object max(List<T> list);
public static <T extends Comparable<T>> Object max(List<T> list);
public static <T extends Comparable<? super T>> Object max(List<T> list);
```

#### 泛型通配符
Apple虽然是Fruit的子类，但是ArrayList<Apple>却不是ArrayList<Fruit>的子类，它们没有任何关系，不能转型，此时可以用通配符?解决
```
public void print(ArrayList<? extends Fruit> list)
{
    for (Fruit e:list)
    {
        //
    }
}

ArrayList<Apple> list = new ArrayList<Apple>();
list.add(new Apple());
list.add(new Apple());

print(list);
```

#### 日志工具
SLF4J（Simple Logging Facade for Java）是一个抽象层，底层实现可以是Logback，Log4J，tinyLog，JDK logging等等

#### 数据交换格式
* 断电后，当线程、反射、注解都不复存在了，但是Java对象的字节流还可以存在于硬盘，序列化和反序列化可以让Jvava对象跨越时间和空间而永生，早期Java RPC中的RMI，可以轻松的调用远程服务器的Java方法，相当于调用本地方法一样，十分方便，但是RMI只能在Java环境中，对服务器不是问题，但是当Web兴起，浏览器中很难有Java环境，RMI也就没落了；序列化虽然简单方便，但是却依赖于具体语言，可以用任何语言去解析读取二进制字节流，但是如果不知道是哪个类的数据，字段的属性，无法构建出具体的对象
* XML虽然稍微复杂，但是语言中立，客户端Java/C/Python/Ruby甚至浏览器里的JavaScript都能解析，缺点是冗余标签太多，网络传输时非常浪费
* JSON不仅语言中立，而且精炼，完美的集中了二进制序列化和XML的优点，已经成为Web下数据交换的主流方式，和JavaScript联手统治了浏览器
* 对二进制序列化增加一个中间层，定义描述消息的格式，把这个自定义的消息格式转换成各种语言的实现，既语言中立，又采用二进制，体积小，解析速度快，完美的综合了各种优点，越来越受欢迎

#### 互斥锁
同一时刻只有获得锁的线程才能操作共享资源，其他线程被阻塞了，只能等待，即使搞个队列，先进先出，效率依然低下，公平实在是个稀缺货，synchronized是个悲观锁
```
public class Sequence
{
    private int value;
    public synchronized int next()
    {
        return value++;
    }
}
```

#### CAS
compareAndSwap: 互斥锁，不加锁也能实现，CAS也叫乐观锁
1. 从内存中读取value，假设是10，称为A
2. B=A+1得到B=11
3. 用A的值和内存中的值相比，如果相等说明无人修改，就把B写入内存，如果不等说明有人修改，则放弃这次修改，跳回1
```
public int next()
{
    while(true)
    {
        int A=读取内存的值;
        int B=A+1;
        if (compareAndSwap(内存的值，A, B))
        {
            return B;
        }
    }
}
```        
compareAndSwap用Java实现不了，需要JNI用C来实现，Java上层封装后的结果：
```
public class Sequence
{
    private AtomicInteger count = new AtomicInteger(0);
    public int next()
    {
        while(true)
        {
            int current = count.get();
            int next = current + 1;
            if (count.compareAndSet(current, next))
            {
                return next;
            }
        }
    }
}
```

#### ABA问题
`AtomicInteger, AtomicBoolean, AtomicLongArray`都只是基本的数据类型，如果是复杂的数据结构，可以用`AtomicReference`只比较两个对象的引用，但是如果有两个线程，1读到内存的数值是A，然后撤出CPU，2也读到了A，把它改成了B，又改成了A，接着1开始运行，读到的值还是A，完全不知道内存被操作过，如果使用`AtomicReference`并且操作复杂的数据结构就有可能出现问题
```
现有链表A->B-C
线程1，试图删除A，但是没有删，只是指向了A
线程2，删除了A，删除了B，再将A添加到表头
线程1，指向的还是A，删除A的话，表头指向的是不存在的B
```
解决办法是给每个`AtomicReference`的对象加入一个version，不仅要判断引用，也要判断对象的version，就能区分开了

#### 模板方法
将日志、安全、事务、性能统计等非功能需求都定义在一个父类，用户管理等子类去实现这个父类，缺陷是父类决定一切，要执行哪些非功能性模块，子类只能完全接受

#### 装饰模式
定义一个含有方法execute的命令接口，日志、安全、事务、性能统计等非功能性模块各自实现这个接口，订单等功能也实现这个接口，创建订单类对象的时候用日志、安全、事务、性能统计组合进行初始化，非常灵活，但是日志、安全、事务、性能统计为什么要实现业务接口呢

#### AOP
非功能需求比如日志、安全、事务、性能统计和功能需求如用户管理、订单管理、支付管理如何互不影响，好的办法是将他们完全隔离，他们之间应该是正交的。
1. 首先要定义切面类（Aspect），如事务等
2. 然后再定义切入点（PointCut），可以是一个方法或一组方法
3. “在方法调用前/后，需要执行XX”，即通知（Advice），描述这些规则，XML是不二之选
```
<bean id="tx" class="com.coderising.Transaction"/>
<aspect ref="tx">
    <pointcut id="place-order" expression="expression(*com.coderising.*.execute(..))" />
    <before pointcut-ref="place-order" method="begin" />
    <after pointcut-ref="place-order" method="commit" />
</aspect>
```
4. 现在Transanction类和业务类已经完全隔离，但是Java是一门静态强类型语言，运行时可以通过反射查看类的信息，但是要对编译时的类进行修改时不可能的，可以在编译的时候做手脚，根据AOP的配置信息，悄悄的把日志、安全、事务、性能统计等切面代码和业务类编译到一起，这需要增强编译器，而且业务类会被改变，不是很好；或者在运行时动态生成代理类，去增强现有的业务类，生成代理类的方法有两种，Java动态代理技术（要求业务类必须要有接口）和CGLib技术（只要业务类没有被标记final）
5. 经过AOP增强的类不是原有类，是自动生成的，怎么去创建和使用？需要容器去完成对象创建的任务

### 3 浪潮之巅的Web
超文本标记语言HTML -> 浏览器（Chrome, IE）展示-> 网络服务器（Appache, Ngnix）解析

#### Socket服务
端口可能被防火墙屏蔽，但是不会屏蔽Web服务的端口
Web服务：HTTP-80  HTTPS-443  GET/POST + JSON

#### 对称加密
双方持有相同的公共的密钥
#### 非对称加密-RSA
用私钥加密的数据只有对应的公钥才能解密，用公钥加密的数据只有对应的私钥才能解密。A端先用B端的公钥加密要发送的消息，B端收到消息后用自己的私钥解密
以非对称加密的方式将对称加密的密钥传输后再用对称加密算法进行通信
#### 中间人劫持
B端发送自己公钥的时候被中间人劫持后将自己的公钥发送到A端，A端用中间人的公钥发送消息，中间人用自己的私钥解密后，用A的公钥发送消息到B端，B端用自己的私钥解密后返回的消息被中间人用自己的私钥解密

#### 数字签名
B端把自己的公钥和个人信息用一种Hash算法（MD5, SHA，具有不可逆性，抗冲突性和分布均匀性）生成一个消息摘要，只要输入数据有一点变化，生成的消息摘要会发生巨变。让认证中心CA用它的私钥对消息摘要加密，形成签名

#### 数字证书
把原始信息和数字签名合并。当A端收到B端的证书时，就用同样的Hash算法再次生成消息摘要，然后用CA的公钥对数字签名解密，得到CA创建的消息摘要，两者对比就知道有没有被篡改。CA的公钥如何防止被中间人劫持？CA本身也有证书来证明自己的身份，由CA得上一级来验证，再由上一级验证。。。链条的根部就是操作系统/浏览器预制的顶层CA证书

#### HTTPS流程
1. 浏览器发出安全请求访问某个网址
2. 服务器发送数字证书（包含服务器的公钥）
3. 浏览器用预制的CA列表验证证书，如果有问题则提示风险
4. 浏览器生成随机的对称密钥，用服务器的公钥加密
5. 服务器用自己的私钥解密，得到对称密钥
6. 双方都知道了对称密钥，用它来加密通信

#### 登录流程
1. 输入用户名和密码
2. 验证通过就建立session
3. 把sessionID通过cookie 发送给浏览器
4. 下次访问URL的时候，cookie就会发过来，判断是否已经登录了

#### 单点登录
cookie是无法跨域的，即a.com产生的cookie浏览器不会发送到b.com，如果多个系统的架构不同，语言不同，共享session很麻烦
COBOL-慢
用户在某个系统登录后，在cookie中生成一个token，访问别的系统时验证下token，如果没问题，就认为用户已经登录，每个系统生成token的时候，需对有header信息和userID的数据用Hash算法和密钥生成一个签名，签名本身和header信息和userID组成完整的token，收到token后，用同样的算法对header信息和userID进行计算签名，如果双方签名一致，则取出userID使用，但是各个系统的userID可能不一样

#### CAS
Central Authentication
1. 如果网址A需要登录，先重定向到认证中心
2. 如果没登录，则让用户登录，成功后
3. 认证中心会创建一个session，一个ticket（随机字符串），注册系统A
4. 认证通过后，再重定向到网址 www.a.com/A， 此时URL中带着ticket，cookie也会发到浏览器
5. 浏览器拿着ticket去认证中心确认，用户是否已经登录
6. 如果登录过了，网站服务器就创建session，返回A这个资源给浏览器，自己的cookie也发给浏览器
7. 如果再次访问同一域名下的 www.a.com/B， 浏览器会携带网站服务器的cookie判断已经登录过了，就不会再登录
8. 如果要访问不同域名下的 www.b.com/A， 浏览器将之前保存的认证中心的cookie发给认证中心
9. 验证已经登录后，再重定向到网址A，并返回另一个ticket
10. 浏览器拿着ticket去认证中心确认，验证ticket有效后，注册系统B
11. 如果登录成功，网站服务器就创建session，返回A这个资源给浏览器，自己的cookie也发给浏览器
12. 如果再次访问同一域名下的 www.b.com/B， 浏览器会携带网站服务器的cookie判断已经登录过了，就不会再登录

##### Oauth-token（资源所有者密码凭据许可-Resource Owner Password Credentials Grant）
从邮箱自动读取与信用卡有关的信息进行分析汇总后形成报表，但是需要邮箱用户名和密码
重定向到网易认证系统，用用户名和密码登录后，系统会询问是否允许“信用卡”管家访问邮箱，确认后，再重定向到管家网址，同时带token（认证系统授权）过来，用token就可以通过API访问邮箱，在这个过程中不会接触到用户名和密码
##### Oauth-Hash Fragment（隐式许可-Implicit Grant）
只会停留在浏览器端，只有JavaScrip才能访问，不会再次通过HTTP Request发送到别的服务器，token是明文的，虽然以HTTPS传输，但在浏览器的历史记录和访问日志能找到
##### Oauth-授权码（授权码许可-Authorization Code Grant）
通过认证系统的授权后，不直接发token，而是发送授权码，管家拿着授权码再次访问认证系统，该授权码和管家的app_id和app_code关联，验证成功后才发送token给管家，浏览器不直接接触token
```
资源所有者：用户  资源服务器：邮箱  客户端：信用卡管家  授权服务器：认证系统
```

#### 数据库缓存
建立数据库连接的代价极为昂贵
```
计算机行业的所有问题都可以通过增加一个抽象层来解决
```
把数据库数据以java对象的形式缓存在内存，在进行数据库操作时先在缓存中查询，如果没有再去调用数据库连接；如果把缓存和应用程序都放在应用服务器Tomcat，则在同一进程中，可以直接访问，效率最高；然而当用户量大的时候处理起来会力不存心，缓存需要存放到单独的服务器中去，此时需要将java对象序列化保存传输，效率会降低
#### Redis
可以快速的存储海量key-value字符串，如一个User对象有三个属性，id, name, email，可以生产两个key-value来存储（"name_id", "sky"）和（"email_id", "sky@gamil.com"），可以先序列化再以二进制方式存储，不过以JSON格式存储更为普遍，可以支持List（可当做队列或栈）, Set（无序集合，包含不重复的字符串），Sorted Set（有序集合）， Hash（键值对的无序散列表）
#### Jedis
Tomcat的客户端，Tomcat把数据转换为JSON后，其余工作由Jedis处理，无需关心跨网络访问细节，当数据越来越多时，Redis也需要部署多台
##### 余数算法
对于需要存储的key-value，计算出key的Hash，对服务器数目取余数，方便快速，然而如果要增加一台服务器，则难以处理
##### 一致性Hash算法
对于0-2的32次方的数据画一个圆，范围每个Redis服务器等分，每个服务器的IP或hostname取Hash值，对于数据key-value，key取个Hash值，按顺时针，key的Hash值接近哪个服务器，则存储到哪个如武器；如果增加一台服务器，则会使这台服务器和逆时针相邻的那台服务器之间的缓存数据失效
##### Hash槽
每台服务器平均分管16384个槽，对数据key-valu，先对key运用CRC16算法产生一个整数值，再进行Hash值后对16384取余，属于哪个槽就将数据存储到哪个服务器；如果增加一台服务器，则把其他服务器的一些数据key-value都迁移到新的服务器
#### Redis Cluster
负责统筹协调各个Resi服务器之间的通信
#### 故障转移
如果是集群，需要支持故障转移，卡槽分为两组服务器，特别提供了master-slave功能，如nodeMster-A, nodeSlave-A1, nodeSlave-A2和nodeMaster-B, nodeSlave-B1, nodeSlave-B2，master和slave的数据是一样的，如果master挂掉，马上用备份slave替换为master
#### Nginx
处理简单的静态资源，HTTP请求, HTML, JS, CSS ，虽然稳定强大，却只有一个实例，增加一个服务器，让keepalive把他们形成master-slave结构，对外只提供一个IP地址，一个服务器挂了，另一个马上接管，只转发请求，不保存状态，但要保证Tomcat的负载均衡
#### Tomcat
处理动态资源请求，有Session
##### 失效转移
在Tomcat1上创建的购物车，服务器挂了，则购物车，登录信息这些状态将不复存在，将这些Session都缓存到Redis的集群
#### 数据库的读写分离
Web应用的读操作远远多于写操作，可以把一个master服务器设置为可读可写，其他的slave服务器只读，读写分离还可以极大的缓解程序对X（排它锁）锁和S（共享锁）锁的争用，如果master挂了，slave马上成为master；可以增加一个代理层，来负责各个服务器之间的协调，控制把Tomcat的写操作发到master，读操作发到slave
#### 本地过程调用
所有的函数调用都在同一个进程
#### 远程过程调用RPC
客户端的代理Stub和服务器端的代理Skeleton，通过socket或者http通信，把复杂的网络细节隐藏，在客户端看来和本地过程调用一样，但是要慢很多，不管传递的是基本数据类型还是对象都要进行序列化
#### SOA
Service-Oriented Architecture, SOA解决多服务凌乱问题，SOA架构解决数据服务的复杂程度，同时SOA又有一个名字，叫做服务治理, SOA的提出是在企业计算领域，就是要将紧耦合的系统，划分为面向业务的，粗粒度，松耦合，无状态的服务。服务发布出来供其他服务调用，一组互相依赖的服务就构成了SOA架构下的系统
#### 微服务
各个小组件之间最好通过最轻量级的基于HTTP的RESTful对外提供接口，WSDL、SOAP太重了,
微服务与SOA相比，更强调分布式系统的特性，比如横向伸缩性，服务发现，负载均衡，故障转移，高可用。互联网开发对服务治理提出了更多的要求，比如多版本，比如灰度升级，比如服务降级，比如分布式跟踪，这些都是在SOA实践中重视不够的
#### Docker
自动化快速部署，将代码和环境一起形成镜像，把其放在服务器端的docker运行环境中，开发环境，测试环境，生产环境轻松保持一致; Docker容器技术的出现，为微服务提供了更便利的条件，比如更小的部署单元，每个服务可以通过类似Node.js或Spring Boot的技术跑在自己的进程中。可能在几十台计算机中运行成千上万个Docker容器，每个容器都运行着服务的一个实例。随时可以增加某个服务的实例数，或者某个实例崩溃后，在其他的计算机上再创建该服务的新的实例
#### 框架
框架是一个半成品，无法独立运行，必须有开发人员去定义他的规则，把项目的代码放到指定的地方，由框架整合起来，才是一个完整的程序;单纯的通过
Java Web基础：HTTP, HTML, JavaScript, CSS, Servlet, JSP, Tomcat...去做整合，非常复杂，Spring MVC，Hibernate，MyBatis都是流行框架，但是框架只实现了很小一部分业务，还有系统架构设计、缓存、性能、高可用性、安全、备份等等
#### HTTP Server
浏览器向服务器发送HTTP Request，服务器收到后让操作系统建立HTTP层下面的TCP连接通道socket，并提供socket，bind，listen，accept等接口，如果是单进程，就会阻塞，主要是receive会很慢，获取到数据后再send出去；可以让主进程监听80端口，但只负责接收连接请求，具体事务receive和send通过创建子进程处理，但是如果并发的连接请求任务特别多，每个进程要耗费大量的系统资源，切换进程就非常耗时，把子进程改成线程，也无法根本解决问题
#### select模型
一个socket连接是一个所谓的文件描述符fd_set的简单的数据结构，如果用重量级的进程对其读写太浪费，HTTP Server发送需要检查socket有没有数据的请求给操作系统，然后阻塞，操作系统会在后台检查这些编号的socket，如果发现可以读写，则做一个标记，再唤醒HTTP Server，去处理这些socket数据，处理完毕之后，把那些socket fd通知操作系统，再进入阻塞；只是HTTP Server 需要把 fd_set 不断的复制给操作系统，很耗资源，从阻塞中唤醒后需要遍历1024个socket，看看有没有标志位需要处理，实际上很多socket并不活跃，需要处理的并不多
#### epoll模型
1. 操作系统维护一个需要监控的socket集合
2. HTTP Server告知操作系统哪些socket需要处理，然后阻塞
3. 操作系统返回需要处理的fd_set给HTTP Server
4. HTTP Server仅仅处理操作系统发回来的socket
