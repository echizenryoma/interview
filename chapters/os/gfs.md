## GFS

### 体系结构

![](https://upload.wikimedia.org/wikipedia/commons/c/c3/GoogleFileSystemGFS.svg)

### 角色

* `Client`提供应用程序的访问接口，以库文件的形式提供
* `Master`是GFS的管理节点。在逻辑层面只有一个，保存整个系统的元数据。负责整个系统的文件管理
* `Chunk Server`负责存储工作。数据主要以文件的形式存储在数据块服务器上

### 客户端

1. 客户端代码包含文件系统的API
2. 客户端能和管理节点和数据块服务器进行通信
3. 客户端和管理节点交互元数据信息，客户端会缓存从管理节点获取的元数据信息。
4. 客户端和数据块服务器交互文件数据，客户端所有的数据相关的通信是直接和数据块服务器进行，但**不会**缓存文件数据

#### 管理节点

1. 管理节点负责管理所有的文件系统的元数据，包括文件和数据块的命名空间、访问控制信息、文件到数据块的映射关系、当前数据块的位置等等信息。
2. 管理节点控制系统级别的活动，包括数据块的分配管理、孤点数据块的垃圾回收机制、数据块在数据块服务器之间的移动。
3. 管理节点周期地跟每个数据块服务器通信，给他们以指示并收集他们的状态。
单一管理节点设计必须减小管理节点的读/写操作，以避免它成为集群瓶颈。

#### 数据块服务器

1. 每一个文件都拆成固定大小的数据块(`Chunk`)。每一个块都由管理节点根据块创建的时间产生一个全局唯一的以后不会改变的64位的数据块句柄标志。
2. 数据块服务器在本地磁盘上用Linux文件系统保存这些块，并且根据数据块句柄和字节区间，通过Linux文件系统读写这些块的数据。
3. 出于可靠性的考虑，每一个块都会在不同的数据块句柄上保存备份。默认情况下，保存3个备份。

### 机制

1. `GFS`将文件划分为大小为64M数据块，每一个数据块都有唯一的索引号。
2. 客户端首先访问管理节点，获取交互的数据块服务器信息，然后访问这些数据块服务器，完成数据存取工作。这种设计方法实现了控制流和数据流的分离
3. 客户端和管理节点之间只有控制流，而无数据流，极大地降低了管理节点的负载。
客户端与数据块服务器之间直接传输数据流，同时由于文件被分成多个数据块进行分布式存储，客户端可以同时访问多个数据块服务器，从而使得整个系统的输入输出高度并行化，系统整体性能得到提高。

#### 读取数据

1. 客户端把应用要读取的文件名和偏移量，根据固定的数据块大小，转换成为文件的数据块索引。
2. 向管理节点发送这个包含了文件名和数据块索引的请求。
3. 管理节点返回相关的数据块句柄以及对应的位置，客户端缓存这些信息。
4. 客户端就向最近的对应位置的数据块服务器发起请求，请求包含数据块句柄以及一个在这个数据块内需要读取的字节区间信息。
5. 数据块服务器返回给客户端要读取的数据块。