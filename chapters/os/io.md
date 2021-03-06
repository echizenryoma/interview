## 输入输出管理

### Linux的I/O模型

* **同步模型（synchronous I/O）**

* 阻塞IO（bloking I/O）

* 非阻塞IO（non-blocking I/O）

* 多路复用IO（multiplexing I/O）

* ~~信号驱动IO（signal-driven I/O）~~ 不常用

* **异步IO（asynchronous I/O）**

#### 简单矩阵

| | **阻塞** | **非阻塞** |
| :--: | :-------: | :-------: |
| **同步** | read/write | read/write<br />\(`O_NONBLOCK`\) |
| **异步** | multiplexing I/O<br />\(`select`/`poll`\) | asynchronous IO |

#### I/O模型比较

<table>
<tr>
  <td align="center"><b>阻塞I/O</b></td>
  <td align="center"><b>非阻塞I/O</b></td>
  <td align="center"><b>多路复用I/O</b></td>
  <td align="center"><b>信号驱动I/O</b></td>
  <td align="center"><b>异步I/O</b></td>
</tr>
<tr>
  <td align="center">
    发起<br />
     | <br />
     | <br />
     | <br />
    阻塞<br />
     | <br />
     | <br />
     ↓ <br />
    完成<br />
  </td>
  <td align="center">
    检查<br />
    检查<br />
    …<br />
    检查<br />
    检查<br />
     | <br />
    阻塞<br />
     ↓ <br />
    完成<br />
  </td>
  <td align="center">
    检查<br />
     | <br />
    阻塞<br />
     ↓ <br />
   就绪发起<br />
     | <br />
    阻塞<br />
     ↓ <br />
    完成<br />
  </td>
  <td align="center">
    <br />
    <br />
    <br />
    <br />
   通知发起<br />
      | <br />
     阻塞<br />
      ↓ <br />
    完成<br />
  </td>
  <td align="center">
    发起<br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    <br />
    通知<br />
  </td>
</tr>
</table>

### IO多路复用

#### 基本概念

I/O多路复用是指内核一旦发现进程指定的一个或者多个IO条件准备读取，它就通知该进程。I/O多路复用适用如下场合：

1. 当客户处理多个描述字时（一般是交互式输入和网络套接口），必须使用I/O复用。
2. 当一个客户同时处理多个套接口时，而这种情况是可能的，但很少出现。
3. 如果一个TCP服务器既要处理监听套接口，又要处理已连接套接口，一般也要用到I/O复用。
4. 如果一个服务器即要处理TCP，又要处理UDP，一般要使用I/O复用。
5. 如果一个服务器要处理多个服务或多个协议，一般要使用I/O复用。

与多进程和多线程技术相比，I/O多路复用技术的最大优势是系统开销小，系统不必创建进程/线程，也不必维护这些进程/线程，从而大大减小了系统的开销。

#### select

调用`select`函数会阻塞，直到有文件描述符就绪或者超时，函数返回。当`select`函数返回后，可以通过遍历`fd_set`，来找到就绪的描述符。

##### 优点

`select`目前几乎在所有的平台上支持，其良好跨平台支持也是它的一个优点

##### 缺点

1. 每次调用`select`，都需要把`fd_set`从用户态拷贝到内核态
2. 同时每次调用`select`都需要在内核遍历传递进来的所有文件描述符
3. `select`支持的文件描述符数量太小了，默认是1024

#### poll

`poll`的实现和`select`非常相似，只是描述文件描述符集合的方式不同，poll使用`pollfd`结构而不是`select`的`fd_set`结构，其他的都差不多。

#### epoll

`epoll`使用一个文件描述符管理多个文件描述符，将用户关系的文件描述符的事件存放到内核的一个事件表中，这样在用户空间和内核空间的只需一次复制操作。

`epoll`把实例创建、监听事件和事件轮询都分开，这样`epoll`的实例就可以被同一个进程中的所有线程共享。`epoll`跟`poll`一样，使用链表节点记录监听事件，但是有三个链表型结构，即就绪链表、辅助链表、红黑树。想要监听的事件节点被放入红黑树，这样可以加快事件节点的访问。事件就绪之后会被挂载到就绪链表，当`epoll_wait`从内核空间向用户空间写出就绪就绪的时候，会遍历就绪链表，但与此同时还会有新的就绪事件发生，这时的就绪事件不再添加到就绪链表，而是使用辅助链表。

##### 模式

`水平模式`（`LT`）：当`epoll_wait`检测到描述符事件发生并将此事件通知应用程序，应用程序可以不立即处理该事件。下次调用`epoll_wait`时，会再次响应。

`边缘模式`（`ET`）：当`epoll_wait`检测到描述符事件发生并将此事件通知应用程序，应用程序必须立即处理该事件。如果不处理，下次调用`epoll_wait`时，不会再次响应。

###### 区别

假设监听一个管道是否可读，当事件就绪之后，程序只读了部分内容，还有部分内容没有读。当再次`epoll_wait`遍历的时候，对水平模式来说，就绪队列里还有这个事件的节点，再次获取状态还是可读的，所以还是不会从就绪队列里删除，然后返回这个这个事件；对边缘模式来说，就绪队列里没有这个事件的节点了，所以也就不会再对它进行通知了。假设第一次`epoll_wait`的时候，管道里的内容已经全部被读完，下次`epoll_wait`遍历到这个节点，获取它的状态，它就已经不再是就绪状态，因为管道空了，这个时候水平模式就不会再把这个节点重新添加到就绪队列里了。

水平模式相对比较慢，但是比较安全，也就是如果真的是就绪的话它会再次通知你；而边缘模式相对比较快，但是有可能造成事件的丢失，这就可能让程序永远阻塞。水平模式为了可靠性，牺牲了一定的效率，而边缘模式为了效率将责任推给程序员。

##### 优点

1. 没有最大并发连接的限制（但是受内存限制）
2. 效率提升，不是轮询的方式，不会随着文件描述符数目的增加效率下降。只有活跃可用的文件描述符才会调用`callback`函数，即`epoll`最大的优点就在于只管活跃的连接，而跟连接总数无关，因此在实际的网络环境中，`epoll`的效率就会远远高于`select`和`poll`
3. 内存拷贝，利用`mmap()`文件映射内存加速与内核空间的消息传递；即`epoll`使用`mmap`减少复制开销

##### 缺点

没有大量的`idle-connection`或者`dead-connection`，`epoll`的效率并不会比`select`/`poll`高很多

