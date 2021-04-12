# 高并发编程理解

AtomicInteger:
Java中的关键字,不用加锁也可以实现递增.
底层通过CAS(compare and swap)方法实现.
CAS方法:
<img src="/images/cas.png">
<img src="/images/cmpxchg.png">

>线程读取AtomicInteger的值,加1之后再比较新的值,如果新的值未改变,则更新新值,如果改变,则重复上述步骤.

解决线程读取过程中的ABA问题.0->2->0,线程是不可见这种改变的.但这种改变有很有意义,
解决方法:增加版本号,并且读取当前值时也读取版本号.

synchronized:是Java中的关键字,是一种同步锁.它修饰的对象有以下集中:
+ 修饰一个代码块
+ 修饰一个方法
+ 修改一个静态的方法
+ 修改一个类

关于锁的信息是记录在对象的markword里面,markword占8个字节,锁的升级过程的信息都会存在markword中.

<img src = "/images/lockValue.png">

新建的对象为无锁态.当有线程调用时,会将锁变成偏向锁,当有线程争用这个锁时,就会升级为轻量级锁,轻量级锁(自旋锁)会一直自旋(消耗内存),也是同样的使用CAS操作,但是当这个自旋过程超过10次时,或者自旋线程超过CPU核数的一半,就会从用户态陷入内核态,交由操作系统,让他去控制wait,这样不消耗内存,让操作系统去调用它.

一个有趣的乱序证明:
``` java
public class funnyDisturb {
    private static int x=0,a=0;
    private static int y=0,b=0;

    public static void main(String[] args) throws InterruptedException {
        int i=0;
        for(;;){
            i++;
            x=0;y=0;
            a=0;b=0;
            Thread one = new Thread(new Runnable() {
                @Override
                public void run() {
                    a=1;
                    x=b;
                }
            });
            Thread two = new Thread(new Runnable() {
                @Override
                public void run() {
                    b=1;
                    y=a;
                }
            });
            one.start();two.start();
            one.join();two.join();
            if(x==0&&y==0){
                System.out.println("第"+i+"次"+x+","+y);
                System.out.println("有趣的事情发生了,运行乱序被证明了");
                break;
            }
        }
    }
}

```

MESI Cache一致性协议
Modified Exclusive Shared Invalid
保持缓存的数据一致性协议.缓存行一般多用64字节.
有些无法被缓存的数据或者跨越多个缓存行的数据依然必须使用总线锁!
