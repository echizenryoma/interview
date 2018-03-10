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

### IO多路复用（事件驱动）

#### 基本概念

I/O多路复用是指内核一旦发现进程指定的一个或者多个IO条件准备读取，它就通知该进程。I/O多路复用适用如下场合：

1. 当客户处理多个描述字时（一般是交互式输入和网络套接口），必须使用I/O复用。
2. 当一个客户同时处理多个套接口时，而这种情况是可能的，但很少出现。
3. 如果一个TCP服务器既要处理监听套接口，又要处理已连接套接口，一般也要用到I/O复用。
4. 如果一个服务器即要处理TCP，又要处理UDP，一般要使用I/O复用。
5. 如果一个服务器要处理多个服务或多个协议，一般要使用I/O复用。

与多进程和多线程技术相比，I/O多路复用技术的最大优势是系统开销小，系统不必创建进程/线程，也不必维护这些进程/线程，从而大大减小了系统的开销。

#### 常见实现方法

* `select`\(Linux/Windows/BSD\)
* `epoll`\(Linux\)
* `kqueue`\(BSD/Mac OS X\)