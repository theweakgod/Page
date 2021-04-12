# java虚拟机学习

## JVM的学习记录

### JVM的生命周期
#### JAVA虚拟机的启动
JAVA虚拟机的启动是通过引导类加载器(bootstrap class loader)创建一个初始类(initial class)来完成的。初始类中会加载很多内容。
#### JAVA虚拟机的执行
+ 程序开始执行时他才运行,程序结束时停止运行
+ 所谓的执行JAVA程序,真真正正运行的是一个JAVA虚拟机的进程。可以有多个进程，这并不冲突。
#### JAVA虚拟机的结束(结束JAVA虚拟机进程)
+ 程序正常执行结束
+ 程序在执行过程中遇到了异常或错误而异常终止
+ 由于操作系统出现错误而导致JAVA虚拟机进程终止
+ 某线程调用Runtime类或System类的exit方法(exit方法仍然是调用的Runtime的halt方法),或Runtime类的halt方法,并且JAVA安全管理器也允许这次exit或halt方法.
该方法的本质还是调用halt0(int status)的本地方法.
+ JNI(Java Native Interface)规范描述了用JNI Invocation API 来加载或卸载 Java虚拟机时,Java虚拟机的退出情况。
主流虚拟机都是解释+编译，如果把解释比如成步行，编译比如成坐公交，那么坐公交等待时间太长，步行虽然不用等待，但终归没有坐公交快。所以现在主流的虚拟机都是解释+编译！

#### hotspot虚拟机
默认的虚拟机都是Hotspot,底层有通过计数器找到最具编译价值代码,触发即时编译或栈上替换。
通过编译器和解释器同时工作，在最优化的程序响应时间与最佳执行性能中取得平衡。

#### JRockit
JRockit是世界上最快的Java虚拟机。

### 类加载过程
#### 加载
1. 通过一个类的全限定名获取定义此类的二进制字节流
2. 将这个字节流所代表的静态存储结果转化为方法去的运行时数据结构
3. 在内存中生成一个代表这个类的java.lang.Class对象,作为方法区这个类的各种数据的访问入口

#### 链接
##### 验证
+ 目的在于确保Class文件的字节流中包含信息符合当前虚拟机要求,保证被加载类的正确性,不会危害虚拟机自身安全.
+ 主要包含四中验证,文件格式验证,元数据验证,字节码验证,符号引用验证.
##### 准备
+ 为类变量分配内存并且设置该类变量的默认初始值,即零值.
+ 这里不包含用final修饰的static,因为final在编译的时候就会分配了,准备阶段会显式初始化
+ 这里不会为实例变量分配初始化,类变量会分配在方法区中,而实例变量是会随着对象一起分配到Java堆中.
##### 解析
+ 将常量池内的符号引用转换为直接引用的过程.
+ 事实上,解析操作往往会伴随着JVM在执行完初始化之后再执行.
+ 符号引用就是一组符号来描述所引用的目标.符号引用的字面量形式明确定义在《java虚拟机规范》的Class文件格式中.直接引用就是直接指向目标的指针,相对偏移量或一个简洁定位到目标的句柄.
+ 解析动作主要针对类或接口,字段,类方法,接口方法,方法型等.对应常量池中的CONSTANT_Class_info,CONSTANT_Fieldref_info,CONSTANT_Methodref_info等.

#### 初始化
+ 初始化阶段就是执行类构造器方法<clinit>()过程.
+ 此方法不需要定义,是javac编译器自动收集类中的所有类变量的复制动作和静态代码块中的语句合并而来.
+ 构造器方法中指令按语句在原文件中出现的顺序执行.
+ <clinit>()不同于类的构造器.(关联:构造器是虚拟机视角下的<init>())
+ 若该类具有父类,JVM会保证子类的<clinit>()执行前,父类的<clinit>()已经执行完毕.
+ 虚拟机必须保证一个类的<clinit>()方法在多线程下被同步加锁.

<img src="/images/reload.png">

### 类加载器的分类
JVM支持两种类型的类加载器,分别为引导类加载器(Bootstrap ClassLoader)和自定义类加载器(UserDefined ClassLoader).

JVM虚拟机将所有派生于抽象类CLassLoader的类加载器都划分为自定义类加载器.

启动类加载器、扩展类加载器、系统类加载器、用户自定义加载器，这四者之间的关系是包含关系。不是上层下层，也不是子父类的继承关系。



启动类加载器(引导类加载器，Bootstrap ClassLoader)
+ 这个类加载使用c/c++语言实现的,嵌套在JVM内部,所以getClassLoader会变成null值.
+ 它用来加载Java的核心库(JAVA_HOME/jre/lib/rt.jar、resources.jar或sun.boot.class.path路径下的内容),用于提供JVM自身需要的类.
+ 并不继承自java.lang.ClassLoader,没有父加载器.
+ 加载扩展类和应用程序类加载器,并指定为他们的父类加载器.
+ 出于安全考虑,Bootstrap启动类加载器只加载包名为java、javax、sun等开头的类

扩展类加载器(Extension ClassLoader)
+ Java语言编写,由sun.misc.Launcher$ExtClassLoader实现.
+ 派生于ClassLoader类
+ 父类加载器为启动类加载器
+ 从java.ext.dirs系统属性所指定的目录中加载类库,或从JDK的安装目录的jre/lib/ext子目录(扩展目录)下加载类库.如果用户创建的JAR放在此目录下,也会自动由扩展类加载器加载.

应用程序类加载器(系统类加载器,APPClassLoader)
+ java语言编写,由sun.misc.Launcher$APPClassLoader实现
+ 派生于ClassLoader类
+ 父类加载器为扩展类加载器
+ 它负责加载环境变量classpath或系统属性 java.class.path指定路径下的类库
+ 该类加载是程序中默认的类加载器,一般来说,Java应用的类都是由它来完成加载
+ 通过ClassLoader#getSystemClassLoader()方法可以获取到该类加载器


