## Java线程

### Java线程状态

Java线程模型定义了 6 种状态，在任意一个时间点，一个线程有且只有其中一个状态：

  * `新建（New）`：新建的Thread，尚未开始。
  * `运行（Runable）`：包含操作系统线程状态中的Running、Ready，也就是处于正在执行或正在等待CPU分配时间的状态。
  * `无限期等待（Waiting）`：处于这种状态的线程不会被分配CPU时间，等待其他线程唤醒。
  * `限期等待（Timed Waiting）`：处于这种状态的线程不会被分配CPU时间，在一定时间后会由系统自动唤醒。
  * `阻塞（Blocked）`：在等待获得排他锁。
  * `结束（Terminated）`：已终止的线程。
  
### Java线程状态切换

![](/assets/java-thread-states.svg)