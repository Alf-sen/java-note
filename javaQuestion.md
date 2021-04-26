#### 1、面向对象与面向过程的区别

面向对象注重参与者能做些什么，面向过程注重做的顺序。面向对象有利于代码的扩展和维护，面向过程更简单直接。

例如洗衣服

* 面向过程：人打开洗衣机放入衣服->洗衣机洗衣服->洗衣机烘干->人取出衣服->人晾衣服

* 面向对象：
  * 人：打开洗衣机放衣服、取出衣服、晾衣服
  * 洗衣机：洗衣服、烘干衣服

面向对象的三大特性：

* 封装
  * 不注重如何实现，只关注能干什么。例如mybatis把数据库的增删改查都封装起来，我们直接调用方法就行。

* 继承
  * 子类继承父类的共通，然后增加自己的实现，可以扩展父类
* 多态
  * 必须有继承
  * 必须重写父类方法
  * 父类引用指向子类对象

#### 2、jdk、jre与jvm联系

* jdk为java开发工具，包含jre
* jre为java运行时所需环境，包含jvm
* jvm为java虚拟机

#### 3、==与.equals()区别

* ==对于基本数据类型比较的是值，对于引用类型比较的是对象内存地址
* .equals()与==的底层实现是一样的，但是大部分会进行重写，例如String类型就进行了重写，变为值比较

#### 4、final用法

* 修饰类
  * 修饰的类不能被继承

* 修饰方法
  * 修饰的方法不能被重写
* 修饰变量
  * 修饰变量要赋初始值且不能再变化

#### 5、String、StringBuffer、StringBuilder区别

* String由final修饰，不可变，每次操作都会产生新的对象
* Stringbuffer和Stringbuilder是在原对象上进行操作
* Stringbuffer由synchronize修饰，线程安全。
* 优先考虑使用Stringbuilder；当需要考虑线程安全时，使用Stringbuffer；当对象几乎不变时使用String

#### 6、重载与重写

* 重载发生在同类中，方法名相同，方法参数个数、类型、顺序不同，返回类型和访问修饰符可以相同
* 重写子类重写父类方法，方法名相同，方法参数个数、类型、顺序必须相同，返回类型和抛出异常必须小于等于父类方法，访问修饰符必须大于等于父类方法访问修饰符，父类中private修饰的方法不能重写。

#### 7、抽象类与接口的区别

* 抽象类可以由普通的成员函数，接口中方法只能定义public abstract方法，java8后可以定义静态方法和默认方法，用static和default修饰。
* 抽象类单继承，接口可以实现多个，必须把接口的所有方法都实现，当多个接口有相同的默认方法，在实现多个接口时实现类需要重写默认方法。
* 抽象类中可以由各种各样的成员变量，接口中成员变量默认都是public static final类型的

##### 扩充：

* 抽象类的设计目的是方法复用，一般是先有子类，再根据子类的共性抽取出父类。
* 接口的设计目的是对类的行为进行约束，只是约束了行为的有无，不关心具体的实现。
* 抽象类表达的时is a的关系，例如小明是一个人；接口表达is like的关系，例如飞机可以像鸟一样飞。
* 使用场景：关注事物的本质时使用抽象类，关注一个操作的时候用接口。

#### 8、list与set的区别

* list是有序的，可以重复的，可以存多个null对象；遍历时可以使用iterato逐一取值，也可以用get()方法进行下标取值。
* set是无序不可重复的，只允许一个存储一个null对象；遍历时只能使用iterator进行逐一取值。

#### 9、hashCode与equals

##### hashCode简介

​	hashCode()计算一个哈希值，返回的是一个int型的整数，用于标记对象在堆中的位置。hashCode定义在JDK中的java.Object类中，所有的java类中都有hashCode()方法。采用键值对的方法，尽快的确定对象在内存中的位置。

##### hashSet举例hashCode用处

​	在往hashSet中存对象时，防止重复对象。在存对象时，调用hashCode()返回一个值确定内存地址，看看该位置是否已经存在对象，如果不存在直接插入。如果该位置已经存在对象，调用equals()方法进行比较，如果对象相等，则不允许再插入该对象；如果两个对象不相等，则再重新进行哈希散列。利用这个方法，可以大大减少比较的过程，提高执行效率。

##### hashCode与equals关系

* 两个对象相等，hashCode()返回值一定相等，equals()返回true
* hashCod()返回值相等，两个对象不一定相等（equals()返回不一定时true）
* 重写equals()方法，就必须重写对应的hashCode()，否则就会导致原有类功能出现问题（hashSet可以存相同的对象）
* hashCode是标记对象在堆中存储位置唯一值的，如果不重写hashCode()，堆中的两个类是不可能相等的（即使两个对象指向相同的数值）。

#### 10、ArrayList与LinkedList

* ArrayList底层是基于动态数组实现，连续内存存储数据，可以下标访问（随机访问）。默认容量为0，当第一次往ArrayList集合中添加元素时，底层数组扩容到10。之后再扩容按1.5倍扩容，先建立一个新的数组，然后把旧数组copy到新数组中。确定初始值，然后采用尾插法，其插入效率可能高于LinkedList（LinkedList插入数据时需要构建node对象，大量插入时很耗费资源）。
* LinkedList底层基于链表实现，存储数据的内存可以分散，便于数据的插入和删除，数据访问效率低。遍历时尽量用iterato，不要用for循环，尽量不要用indexOf返回元素索引，因为可能导致遍历整个链表，过度消耗资源。

#### 11、HashMap与HashTable的区别

* HashMap线程不安全；HashTable有synchronize修饰是线程安全的
* HashMap可以存key为null，存在下标为0的位置；hashMap的key不可以存null。
* HashMap底层结构：数组加链表。jdk8之后，数组长度大于64，链表长度大于8转换为红黑树，红黑树高度小于6时再转回链表。

#### 12、ConcurrentHashMap原理

* JDK8
  * 数据结构：synchronized+CAS+Node+红黑树。node的val和next都用volatile修饰，保证可见性。查找、修改和赋值都使用CAS
  * 锁：锁链表的head节点，不影响其他链表的读写。锁的粒度更细致，效率更高。扩容时阻塞所有的读写，并发扩容。
  * 读操作无锁：node的val和next被volatile修饰，保证可见性。数组被volatile修饰保证扩容时的可见性。

* JDK7
  * 数据结构：ReentrantLock+segment+HashEntry。segment继承ReentrantLock实现分段锁，锁整个segment，一个segment包含多个HashEntry，一个HasEntry包含一个数组和多个链表。
  * 查询：两次Hash，第一次定位segment，第二次定位数字下标。
  * 锁：锁到segment，并发度为segment的个数；扩容时只影响锁定的segment。
  * 读操作无锁：get方法用volatile修饰保证可见性。

#### 13、简单回答如何构建一个IOC容器

* 在配置类中指定需要扫描的包路径。
* 构建一些注解类，分别表示访问控制层、业务服务层、数据持久层、依赖注入的注解、配置文件配置的注解。
* 扫描指定的包路径，获取路径下的文件夹及文件信息，将路径下的所有已.class结尾的文件放到一个set集合中管理。
* 遍历这个set集合，将所有带有指定注解的类放入一个安全的map中（ConcurrentHashMap）。
* 变量这个map，获取所有类的实例，判断是否有需要依赖其他类的实例，进行递归注入。

#### 14、什么是字节码？字节码的好处？

* 字节码就是由jvm通过编辑器生成的虚拟机指令即.class文件，只面向虚拟机，不面向其他机器。
* 程序执行过程：java源码——>编译器——>字节码——>jvm中解释器（不同平台解释器不同）——>机器可执行的二进制码——>程序执行。
* 字节码好处：
  * 提高了解释性语言的执行效率（原有的解释性语言解释一句执行一句，现在可以写完了统一编译成.class文件执行）。
  * 保留了解释性语言的可移植性（跨平台性，jvm为所有的平台编译程序提供了一个相同的接口）。

#### 15、类加载器

* BootstrapClassLoader 顶层加载器，加载%JAVA_HOME%lib目录下的jar包和.class文件。
* ExtClassLoader 扩展类加载器，加载%JAVA_HOME%/lib/ext目录下的jar包和.class文件。
* AppClassLoader 系统类加载器，加载classpath下的类。
* 自定义类加载器

#### 16、双亲委派机制

* 向上委派 ：查询缓存中是否已经加载该类，没有就往上委派，找到就返回找不到继续往上，直到顶层加载器。
* 向下查询：向上委派到顶层加载器依然没有在缓存中找到要加载的类，则查询加载器加载路径中是否有该类，有则加载到缓存中，没有则向下一层加载器的加载路径中寻找，直到发起加载的加载器为止。
* 好处：
  * 保证安全，防止自己写的类污染java的核心类，例如String。
  * 仿真重复加载，提高效率。

#### 17、java中异常体系

* java中所有异常都来自顶级父类Throwable。
* Throwable下面有两个子类Exception和Error。
* Error是程序无法处理的错误，一旦发生erro程序将会停止。
* Exception分两种
  * RuntimeException运行时异常，在程序运行时发生，发生时只会导致当前线程执行失败。
  * CheckedException检查时异常，在程序编译时发生，发生了程序编译不过。

#### 18、GC如何判断那些对象可以被回收

* 引用计数法：对象被引用一次加一，引用完成减一，引用计数器为0时，对象被回收。
  * 优点：判断简单，效率高。
  * 缺点：发生循环引用时，A引用B，B引用A时导致对象永远无法被回收。
* 可达性分析：以GC Roots对象为基础，向下搜索引用的对象，走过的路径为引用链，如果一个对象到GC Roots对象没有任何引用链相连，则表示该对象不可达。
* GC Roots对象:
  * 虚拟机栈中的引用对象
  * 本地方法区中的Native方法引用的对象
  * 方法区中静态变量引用的对象
  * 方法区中常量值引用的对象
* 可达性分析时，需要两次判断才会将对象回收。对象第一次被置为不可达时，GC会判断该对象是否覆写了finalize()方法，如果没有覆写，则直接回收；否则会将该对象放入finalizer队列中执行对象的finalize()方法，执行完毕后再进行可达性分析，不可达则回收，可达则对象“复活”。注：每个对象仅可调用一次finalize()方法。尽量不用。

#### 19、线程的生命周期级状态

* 线程的声明周期
  * New：创建一个新的线程对象。
  * Runnable：线程对象执行start()方法，进入可运行状态，进入可运行线程池中，等待获取cpu的使用权。
  * Running：就绪状态的线程获取cpu的使用权，执行程序。
  * Blocked：线程由于某种原因放弃cpu的使用权，暂时停止运行，直到再次获取cpu使用权方可再次进入运行状态。
  * Dead：线程退出run()方法，或者异常终止，该线程生命周期结束。

* 线程阻塞方法
  * Thread.sleep()：当前线程调用，线程进入阻塞，但不释放对象锁，等到一定时间自动进入就绪状态。作用：给其他线程执行机会的最佳方式。
  * 发生I/O：jvm会把线程置为阻塞状态，直到I/O结束，才会再次进入就绪状态。
  * t.join()：当前线程调用其他线程的join()方法，该线程进入阻塞状态，但不释放对象锁，直到其他线程执行完或者到时后进入就绪状态。
  * Object.wait()：当前线程调用对象的wait()方法，线程进入阻塞状态，释放对象锁，进入等待队列。依靠notify()或者notifyAll()方法唤醒或者wait()时间到了才能再次获取对象锁，进而再次进入就绪状态。
* Thread.yield()：当前线程调用该方法，使该线程放弃已经获取的cpu时间片，由运行状态进入就绪状态，让OS再次选择线程。作用：让同优先权的线程轮流执行，但是并不一定能实现，因为在此选择时，OS可能还是选择该线程。

#### 20、对与线程安全的理解

* 多个线程访问同一个对象时，如果不用同步控制或其他控制，得到的结果与单线程访问这个对象的结果相同的话，就说明这个对象线程安全。
* 堆内存：线程共享的，在虚拟机启动时进行初始化。存放对象。
* 栈：线程独有的，保存线程运行状态及局部变量。在线程开始的时候对栈进行初始化，栈是线程安全的。
* 线程不安全的原因：进程中有一个公共区域，通常称为堆，进程中的所有线程都可以访问这个区域，这就是造成线程不安全的根本原因。

#### 21、Thread和Runnable的区别

Thread是类，也会实现Runnable，Thread提供的API功能更多一些，如果是多线程需要复杂的操作就继承Thread实现一个线程；Runnable是接口，如果是简单的执行一个任务就实现Runnable来实现一个类。

注：不管是继承Thread还是实现Runnable都需要new一个Thread。

#### 22、守护线程的理解

* 什么是守护线程：为所有非守护线程（用户线程）提供服务的线程。一个守护线程是所有用户线程的守护线程。
* 如何定义守护线程：thread.setDaemon(true)，必须在thread.start()之前调用，无法将一个已经运行的线程置为守护线程。Daemon线程中新生成的线程也是Daemon线程。
* 守护线程应用场景：
  * 为其他线程提供支持服务的场景。
  * 在任何情况下，其他线程执行完毕后，该线程正常运行且立马终止。
* 守护线程举例：GC线程就是经典的守护线程，当jvm中不在有用户线程运行时，就没有垃圾产生，GC线程成为jvm中仅剩的线程时，GC线程就会自动终止。GC线程一直在低级别状态中运行着，用于实时监控和管理系统中可回收的资源。
* 扩展：对于Java中自带的框架，例如ExecutorService中，即使你将线程置为Daemon线程，线程也会将Daemon线程置为用户线程。

#### 23、ThreadLocal相关问题

* ThreadLocal是什么

  * 
  * 不是用来解决多线程下共享变量的问题，而是为线程内部提供共享变量，在多线程中可以保证各个线程中的变量相互隔离，相互独立；使用get()和set()方法对变量进行访问。
  * ThreadLocal实例都是private static类型的，希望将状态与线程相关联；这中变量在线程的生命周期中起作用，用于减少同一个线程中多个函数或组件之间一些公共变量传递的复杂度。

* ThreadLocal的使用场景

  * 线程间数据隔离。
  * 在对象跨层传递的时候，避免多次传递，打破层次间的约束。

  * 保存线程的上下文信息，在线程任何地方都可以获取。
  * 线程安全的，减少由于线程必须同步带来的性能损失。

  ​     由于ThreadLocal的特性，一般用在数据库连接管理、事务回滚和提交管理。例如每一个线程连接数据库都是一个独立的操作，如果连接是一个共享对象，有的线程将连接关闭，有些线程依然连接数据库进行事务提交，这样就会导致系统异常。

  ​     每个线程对ThreadLocal的读写都是线程隔离的，相互没有影响，所以无法解决多线程共享对象的更新问题，只是针对线程内的操作是共享的，建议设置为static类型。由于不存在多线程共享信息，所以不存在线程竞争，也不用考虑某些情况要保证线程安全必须进行的同步操作，减少性能损失。

#### 24、ThreadLocal内存泄露原因，如何避免

* 什么是内存泄露

  当不在被使用的对象引用或者变量无法回收时，就会导致内存泄露。一次内存泄露可以忽略，但是多次内存泄露累积就会导致OOM。

* 强引用

  一般用new创建的对象引用或者通过反射得到的引用，都是强引用。不会因为内存不足而进行垃圾回收。可以显示的将引用置位null，就可以进行垃圾回收。

* 弱引用

  jvm在进行垃圾回收的时候，无论内存够不够用，都会将弱引用进行垃圾回收。在java中，用java.lang.ref.weakReference类来表示。一般缓存可以用弱引用。

* ThreadLocal的实现原理

  每一个线程都会维护一个ThreadLocalMap，key为使用弱引用的ThreadLocal实例，value为强引用的线程变量的副本。

* ThreadLocal内存泄露原因

  ThreadLocalMap与Thread的生命周期是相同的，当ThreadLocal外部强引用断开时，ThreadLocalMap中的key为弱引用就会进行垃圾回收，但是对应的value仍未强引用，直到线程停止。就会出现key为null，value不为null的Entry，从而导致内存泄露。

* 如何避免内存泄露

  在使用ThreadLocal后调用，remove()方法。将key-value都清除掉，就能避免内存泄露。

#### 25、并行、并发和串行的区别

* 并行

  时间上可以重叠，两个任务可以同时执行且互不干扰。

* 并发

  允许两个任务彼此干扰，同一时刻只有一个任务运行，交替运行。

* 串行

  同一时刻只有一个人物可以执行，且一个任务执行完之前，其他的任务必须等待。

#### 26、并发的三大特性

* 原子性

  含义：一个操作或多个操作要么全部执行且不被打断，要么全部都不执行。（例如转账，A给B转账，A账户减钱，B账号就得加钱）

  如何保证原子性

  * synchronize关键字
  * Lock锁
  * Automic类(volatile+CAS)

* 可见性

  含义：一个线程改变共享变量的值，其他线程能够立马得到共享变量最新值。

  原理：java内存模型通过共享变量修改后同步到主内存中，在读取变量前从主内存刷新变量的值这种依赖主内存作为传递媒介的方式保证可见性。

  如何保证可见性：

  * volatile关键字
  * Synchronize关键字
  * final关键字
  * Lock锁
  * Automic类

* 有序性

  含义：程序执行的顺序按照代码的先后顺序执行。JVM存在指令重拍，所以存在有序性问题。

  如何保证有序性：

  * Synchronize关键字
  * Lock锁

#### 27、为什么使用线程池，线程池参数

1. 降低资源消耗，提高线程利用率。减少线程创建和销毁所需要消耗的资源。
2. 提高响应效率。来了任务可以直接调用线程执行，不用现创建。
3. 提高线程的管理。在线程池中可以统一调度、优化监控线程。

* corePoolSize核心线程数，线程池中建立后不会被销毁，常驻的线程。
* maximumPoolSize最大线程数，线程池中允许存在最多的线程数量。
* keepAliveTime、unit线程空闲存活时间和单位。当线程池中超过核心线程数的线程处于空闲状态，且空闲的时间达到setKeepAliveTime所设置的时间时，就会被销毁。
* workQueue阻塞队列，当线程池中持续进入任务，且线程池中数量已达到核心线程数量，就会将新进的队列放入阻塞队列队尾，直到队列也放满了，才会创建新的线程执行任务，直到线程数量达到最大线程数量，进行拒绝策略。
  * SynchronousQueue直达队列，对任务不进行缓存，当线程数达到核心线程数时，直接创建新的线程执行任务。
  * ArrayBlockingQueue基于数组的有界阻塞队列，当线程数达到核心线程数时，先将任务放入阻塞队列中，阻塞队列也放满后，再创建新的线程执行任务。
  * LinkedBlockingQueue基于链表的无界队列（实际最大Integer.max），当线程数达到核心线程数时，先将任务放入阻塞队列中，由于阻塞队列无界，可以一直放入，maximumPoolSize没有什么用处。
  * PriorityBlockingQueue优先级的无界阻塞队列，优先级可以通过Comparator实现。

* handler拒绝策略
  * CallerRunsPolicy执行新进入的任务，但是不是由线程池中的线程执行，而是由调用者执行该任务。
  * AbortPolicy忽略新进的任务并抛出异常。
  * DiscardPolicy忽略新进的任务，其他什么也不干。
  * DiscardOldestPolicy将最先进入队列的任务出队，为新进入的任务腾出空间。

#### 28、为什么线程池使用阻塞队列

1. 普通队列只能保存有限缓存空间的任务，当缓存空间满了，新来的任务就会消失，阻塞队列则可以让任务进入阻塞状态进而保留。
2. 阻塞队列自带阻塞和唤醒功能，可以将处于空闲的核心线程进入wait状态，释放cpu资源，使得不用一直占用cpu资源，当有任务进来时有可以自动唤醒线程执行任务。

#### 29、为什么超过核心线程数量先放入阻塞队列，而不是直接创建线程执行任务

1. 创建线程需要获取全局锁，其他线程进入阻塞队列，影响并发效率。（让程序员加班，而不是扩招）
2. 创建过多线程消耗过多的资源，且cpu切换过多。

#### 30、如何理解线程池线程复用原理

1. 线程池将线程与任务进行解耦，线程是线程，任务是任务，摆脱了通过Thread实现线程时必须与任务相对应。
2. 线程池中的线程，可以一直从阻塞队列中获取任务执行，是因为线程池对Thread进行了封装，线程池中的线程在执行任务的时候不用调用Thread.start()来创建新的任务，而是让每个线程去执行一个“循环任务”，在执行这个“循环任务”的时候检查是否有需要执行的任务，如果有就直接调用run方法进行执行，这样就用固定的线程数将所有的任务的run方法串联起来。

#### 31、什么是spring

​    spring是一个轻量级的控制反转（IOC）和面向切面（AOP）的容器框架，用来存储bean对象；可以帮助我们创建对象，管理对象的相互依赖和生命周期。而且可以作为中间框架，把其他框架融合在一起。

#### 32、对AOP的理解

​    AOP面向切面编程，在日常工作中有很多代码都是重复的，例如日志或者事务处理等，在service层都需要将事务开启、提交和回滚，在每一个类里都写一些相同的代码，AOP的思想就是将这些相同的代码抽成一个切面，切入到需要使用的类中，这样就极大的提高了代码的复用性减少了冗余代码。

​    AOP是用动态代理的方式实现的，如果说代理的类有接口就用jdk原生的动态代理;否则就用GCLIB动态代理来实现。日常工作中很少具体的用到AOP，因为框架都已经将日志事务这种处理封装好了，但是这种理念在工作中处处可以看到。

#### 33、对IOC的理解

​    IOC为我们提供了一个容器，负责容纳bean，并对bean进行管理，不用我们手动的取创建bean对象；IOC中有一个特别强大的功能DI依赖注入，我们可以通过xml配置或者带注解的配置类将我们需要的bean实例化，并且可以将bean之间的依赖关系注入到IOC容器中。这种方式可以降低类之间的耦合。

​    IOC的控制反转与依赖注入使用了工厂模式。

​    例子：jdbc在注册数据库驱动时，采用class.forName的方式，不依赖具体的驱动类，将驱动类的全限定类名写到配置文件中，通过依赖注入的思想，想要修改数据库类型时，只要改一下这个配置类中驱动类的全限定类名，就可以轻松的替换数据库类型。

#### 34、BeanFactory与ApplicationContext区别与联系

1. ​    ApplicationContext是BeanFactory的子接口，有更丰富的功能：
   * 继承了MassageSource，支持国际化。
   * 统一资源文件访问方式。
   * 提供在监听器中注册bean的事件。
   * 同时加载多个配置文件。
   * 载入多个（有继承关系）的上下文，使得每一个上下文作用于特定的层次。
2. BeanFactory是懒加载，在使用bean的时候才会对bean进行加载实例化，这样就不容易发现spring的一些配置问题，如果某些配置文件缺失，直到第一次使用这个bean时才会抛出异常。优点是占用空间少。
3. ApplicationContext直接加载，在启动时就会加载所有的bean，这样在启动时就可以发现spring一些配置的问题；而且在运行过程中，由于bean实例已经创建好，不需要花时间再创建，会提高程序运行效率。缺点是会占用大量的空间（如果bean足够多的话）。
4. BeanFactory已编程的方式进行创建，ApplicationContext还可以用声明的方式进行创建，例如通过ContextLoader创建。
5. BeanFactory和ApplicationContext都支持BeanPostProcessor和BeanFactoryPostProcessor。但是BeanFactory需要手工注册，ApplicationContext是自动注册。

#### 35、描述一下Spring中bean的生命周期

1. 解析类得到BeanDifinition。
2. 如果有多个构造器，需要推断构造器。
3. 确定构造器后，实例化一个对象。
4. 对于对象中存在@Autowired等注解进行属性填充。
5. 回调Aware方法，类如BeanNameAware、BeanFactoryAware等。
6. 调用BeanPostProcessor的初始化前方法。
7. 调用初始化方法。
8. 调用BeanPostProcessor的初始化后方法。
9. 如果是单例Bean，则放入单例池中。
10. 使用bean。
11. 关闭Spring容器时，调用DisposableBean的destroy()方法。

#### 36、Spring中支持的几种bean作用域

* singleton：默认单例，每一个Spring容器中只有一个bean实例，由BeanFactory来维护，在每次注入时创建一次，生命周期与IOC容器相同。
* Prototype：每次调用bean时都会创建一个bean对象。容器中有多个bean对象。
* request：每一个request请求中创建一个单例bean对象，该对象在此次request请求中可以复用，随着请求的结束而销毁。
* Session：与request请求相同，每个Sessdion中都有一个单例bean对象，在该Session中可以复用。
* application：bean被定义为在ServletContext的生命周期中可以复用的单例对象。
* Websocket：bean被定义为在Websocket的生命周期中可以复用的单例对象。

#### 37、Spring的单例bean是否线程安全

​    不安全！

​	不要在bean中声明任何带有存储数据功能的实例变量或类变量，如果必须如此，就用ThreadLocal将它们变成线程私有的；如果这些实例变量或类变量需要在多线程下共享，就得使用synchronize、lock或者CAS来保证线程安全。

#### 38、Spring如何保证事务获取同一个Connection

​	spring事务管理器利用ThreadLocal为每个线程维护了一套独立的Connection，使各个线程直接相互没有影响。

#### 39、Spring中使用了哪些设计模式

* 代理模式：AOP底层实现原理，Spring采用JDK proxy和CgLib类库。
* 单例模式：Spring配置文件中定义的bean默认是单例模式。
* 工厂模式：BeanFactory利用工厂模式创建bean。贯穿BeanFactory与ApplicationContext接口的核心理念。
* 委派模式：Spring中使用DispecherServlet对请求进行分发。
* 模板模式：用来处理重复的代码，例如RestTemplate、JmsTemplate等。



#### 40、Spring中事务的实现方式及其原理

​	spring中的事务概念是数据库层面上的，spring只是在数据库事务的基础上进行扩展，提供了一些利于程序员操作的方法。

​	实现方法有两种，一种是编程式，直接调用具体的事务方法；一种是注解式，利用@Transactional。在某个方法上添加@Transaction注解，就可以开启事务，这个方法下的所有sql就会在一个事务中执行，要么全成功，要么全失败。

​	一个方法添加了@Transactional注解后，spring就会给对应的类生成一个代理对象，这个代理对象做为bean放入容器中，当使用这个代理对象的方法时，如果方法上存在@Transactional注解，就会先将事务的自动提交置为false，然后执行原本的业务逻辑，原有业务逻辑没有问题，代理对象会提交事务，原本业务逻辑执行有问题，就会回滚。默认时回滚RuntimeException和Error，也可以通过@Transactional注解中的rollbackFor属性进行设置。

#### 41、Spring事务的隔离级别

​	Spring事务隔离级别就是数据库事务隔离级别，通常有以下4类：

* Read Uncommitted （读未提交）
* Read Commited（读已提交、不可重复读）Oracle默认事务隔离级别
* Repeatable Read（可重复读）Mysql默认事务隔离级别
* Serialiable（可串行化）

事务隔离级别越高，性能越低，数据安全性越高。

​	Spring与数据库设置的隔离级别不相同时，以Spring设置的隔离级别为准，如果Spring设置的隔离级别数据库不支持，则已数据库设置的为准。

#### 42、Spring的自动装配

xml配置

1. no：指Spring的自动装配时关闭的，需要手动在Bean标签中定义依赖关系。
2. byName：根据类的名称来装配。当需要对一个bean的属性自动装配时，spring会根据bean的名称在配置文件中寻找一个匹配的bean进行装配，如果找不到则抛异常。
3. byType：根据类的类型来装配。当需要对一个bean的属性自动装配时，spring根据bean的类型在配置文件中寻找一个匹配的bean进行装配，如果找不到或者找到多个就会抛异常。多个相同类型的需要。。。。。。
4. Constractor：根据构造器来配置，与byType相似。仅适用于与构造器相同参数的bean。找不到抛异常。
5. Autodetect：结合Constractor和byType，找到相同参数构造器就用构造器，如果没有再找相同类型的。

注解

@Autowired

#### 43、Spring注入java集合

```xml
<bean id="javaCollection" class="com.zxs.JavaCollection">
	<property name="javaList">
    	<list>
        	<value>zs</value>
            <value>ls"/value>
        </list>
    </property>
    <property name="javaSet">
    	<set>
        	<value>1</value>
            <value>2</value>
        </set>    
    </property>
    <property name="javaMap">
    	<map>
        	<entry key="1" value="zs"/>
            <entry key="2" value="ls"/>
        </map>    
    </property>
</bean>
```

#### 44、Spring、SpringMVC与SpringBoot的区别

* Spring提供了一个IOC容器，利用控制反转和依赖注入，帮我们创建bean并且管理bean之间的依赖关系，可以减少代码的耦合度；提供了AOP思想，可以解决OOP代码重复的问题。spring做为一个中间框架也可以帮助我们更有效的将其他框架整合在一起。
* SpringMVC提供了一套WEB开发的框架，创建了一个前端控制器DispercherServlet来统一接受web请求，然后定义了一套路由策略（url映射到handle）及适配执行handle，再利用视图解析技术将handle处理的结果返回给前端。
* SpringBoot则是利用约定大于配置的自动配置思想，简化了Spring与SpringMVC的配置，让我们更容易的开发业务代码；同时整合了多种使用场景的stater，例如redis、es、mongodb等可以开箱即用。

#### 45、SpringMVC的工作流程

1. 用户将请求发给前端控制器DispatcherServlet。
2. DispatcherServlet调用HandlerMapping处理器映射器（维持url与handler对应关系）。
3. HandlerMapping确定具体的处理器（xml或者注解），返回给DispatcherServlet。
4. DispatcherServlet调用HandlerAdapter控制器适配器（适配对应的handler,执行具体的方法）。
5. HandlerAdapter经过适配得到具体的处理器，执行该处理器。
6. Controller执行返回ModelAndView。
7. HandlerAdapter将Controller执行结果ModelAndView返回给DispatcherServlet。
8. DispercherServlet调用ViewResolver视图解析器。
9. 视图解析将ModelAndView解析成具体的view返回给DispatcherServlet。
10. DispatcherServlet根据view渲染视图，并相应给用户。


#### 46、Springboot的运行机制（自动装配原理）

理论回答：
springbootapplication 说起，springbootconfiguration标注该类为配置类；
componentscan将指定包下面的需要装配的组件注册到spring容器里；
enableautoconfiguration：
auto configuration package 将主配置所在的包作为为自动配置的包进行管理； 
import 将meta-info下面spring.factories的配置的类导入到spring容器里。 
总结回答：
springboot根据配置文件，自动装配所属依赖的类，然后通过动态代理的方式将类注入到spring容器里。

#### 47、如何理解SpringBoot的starter

​	例如Spring+SpringMVC在使用mybatis时，需要在xml中定义mybatis中需要使用的bean。而SpringBoot的starter则是建立一个mybatis的starter的jar包，然后创建一个带有@Configuration的配置类，将需要使用的bean定义在配置类中，然后在META-INF/spring.factories中写入该配置类的全路径，这样SpringBoot就会根据规则加载该配置类，将bean注入到容器中。

​	开发人员只要将相应的starter包依赖进应用，进行相应的属性定义（默认的可以不配置，连接数据库url之类的需要配置），即可使用相应的功能能。

#### 48、Mybatis的优缺点

优点

* 使用SQL语句编写，相当灵活；在xml中编写，解除了sql语句与应用程序的耦合，便于统一管理；提供xml标签，可以书写动态sql语句，并可以复用。
* 与JDBC相比较，消除了大量的冗余代码，不需要手动的连接关闭数据库连接。
* 数据库支持性好，采用JDBC与数据库连接，凡是JDBC支持的数据库，mybatis就支持。
* 与spring兼容性好。
* 提供映射标签，支持对象与数据库ORM字段关系映射；提供对象关系映射标签，支持对象关系组件映射维护。

缺点

* SQL语句的编写工作量过大，尤其是字段多，关联表多的时候，对程序员sql编写能力有要求。
* SQL语句基于数据库类型，导致数据库可移植性差。

49、Mybatis中#{}与${}的区别

1. #{}是预编译处理，是占位符；${}是字符串替换，是拼接符。
2. #{}的替换是在DBMS中，变量替换后，对应的变量自动加上单引号。
3. ${}的替换是在DBMS外，变量传入的是什么就替换成什么。

使用#{}可以有效的防止SQL注入，提高系统的安全性。





#### mybatis的二级缓存怎么开启？

在mybatis的配置文件中开启二级缓存，并且在mapper.xml中打上cache标签。

#### redis缓存，如何解决雪崩（先说原因，再说怎么解决）

#### mysql索引底层数据结构是什么样的。（索引是干什么用的，底层结构是什么样的，索引优缺点，什么情况才用索引，索引失效情况，

怎么分析慢查询）

#### mysql分页查询

  limit m，n; 查询m条数据之后的n条数据。select * from dual limit 3,5;  4<= id <= 8
  m表示偏移量，初始值为0；
  n表示返回数据的最大条目；
  只有一个参数时表示返回的最大条目；
  当n为-1时表示返回从某一个偏移量到最后一条数据的记录。

#### 分页查询偏移量过大时，查询优化

   1）用子查询，先查询出偏移量的位置的索引值，再用一个索引确定返回的数据条目。（join分页，索引文件要小于数据文件，所以会快一些）
   2）分表查询
