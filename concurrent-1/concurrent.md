#### 多线程

1. 什么是线程和进程？

进程是运行中的程序。

线程是CPU调度的最小单位。

区别：

运行一个程序会开启一个或多个进程，操作系统会为每一个进程开辟独立的内存空间。进程是资源分配的最小单位。

一个进程中可以开辟多个线程，这些线程之间共享堆内存和方法区。

关系：

一个线程可以开辟多个线程。

2. 线程的上下文切换是什么？

   CPU的按照时间片算法来调度线程，当时间片使用完线程的任务尚未执行完时则会在程序计数器中保存下一条带执行的指令。等到该线程再次被CPU调度则会恢复现场，继续执行。

3. 线程的并发与并行有啥区别？

   并发指的是同一段时间内，线程同时被CPU调度执行，同一时刻内不一定同时执行。

   并发指的是同一时刻内，线程被同时被CPU调度执行。

4. 使用了多线程会带来什么问题呢？

   * 带来线程安全性问题：多线程执行的结果与单线程场景下的执行结果不一致。

   导致线程安全性问题的原因是 多个线程对于同一个共享变量的写操作导致

   * 带来上下文切换问题：频繁的上下文切换导致线程任务执行的效率降低。在CPU计算密集型线程数目应设置为i+1 （i为CPU核心线程数），在IO密集型线程数应设置为2i+1（i为CPU核心线程数）
   * 带来线程饥饿问题：多线程执行中存在非公平的抢占式调度，一个线程迟迟得不到调度就会出现该问题。
   * 死锁问题。
   
5. 原子性、有序性和可见性能不能深入的谈一下

   * 原子性：一个操作要么都执行要么都不执行。在操作系统层面常常使用Lock指令前缀配合 + 指令，通过锁总线的方式保证一个指令的原子性。
   * 有序性：程序的指令按照先后顺序执行。为了提高CPU的利用率，允许指令重排序。比如：第一条指令需要访问主存，第二条指令是另外一个变量的赋值语句。两条指令之间不存在数据同步问题。此时可以先执行第二条指令，提高CPU的使用率。
   * 可见性：一个线程修改共享变量对于其他线程来说可以看的见修改后的数值。这是因为为了解决CPU计算速度和内存读取速度的不平衡性，添加了SRAM寄存器做缓存，缓存的最小单位是Cache Line，为了解决CPU之间缓存一致性问题，计算机底层遵循MESI协议。volatile解决缓存一致性的问题的方式，将storeBuffer中的修改刷入到Cache当中将对应缓行置为Modified，在将invalid queue中的失效的缓存信息，刷新到Cache中将对应缓存行置为invalid，之后MESI协议会保证缓存的一致性的问题。

5. 什么是死锁？如何排查死锁?
   
* 线程之间相互请求对方持有的临界资源，造成的相互之间阻塞的情况。
   
     死锁的必要条件：
   
     1. 互斥条件，一个共享资源同一时刻只能被一个线程占有
     2. 请求并保持条件：一个线程拥有共享变量后请求另外的被其他线程占有的共享变量。
     3. 不可剥夺条件：在任务被执行完成之后，线程不释放共享变量
     4. 循环等待：共享变量的请求，存在一种循环等待链条。
   
     排查死锁的方式：使用 jps -l先展示运行中的java进程，在使用jstack -l -PID的方式定位死锁。
   
     此时需要重点关注 处于 waiting on condition 的线程
   
   
   
