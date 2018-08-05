## Nginx

### 进程

1. `Master`进程：读取及评估配置和维持
2. `Worker`进程：处理请求

`Master`进程对`Worker`进程采用信号机制进行控制。

### 负载均衡

1. 轮询：每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。 
2. 加权轮询：指定轮询几率，权值与访问比率成正比，用于后端服务器性能不均的情况。
    * 加权轮询分为先深搜索和先广搜索，那么nginx采用的是先深搜索算法，即将首先将请求都分给高权重的机器，直到该机器的权值降到了比其他机器低，才开始将请求分给下一个高权重的机器。
3. IP哈希：每个请求按访问IP地址的哈希结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。

### 性能

* 异步非阻塞事件处理机制：运用了`epoll`模型，提供了一个队列，排队解决。

### 处理阶段

1. `NGX_HTTP_POST_READ_PHASE`读取请求内容阶段
2. `NGX_HTTP_SERVER_REWRITE_PHASE`: Server请求地址重写阶段
3. `NGX_HTTP_FIND_CONFIG_PHASE`: 配置查找阶段:
4. `NGX_HTTP_REWRITE_PHASE`: Location请求地址重写阶段
5. `NGX_HTTP_POST_REWRITE_PHASE`: 请求地址重写提交阶段
6. `NGX_HTTP_PREACCESS_PHASE`: 访问权限检查准备阶段
7. `NGX_HTTP_ACCESS_PHASE`: 访问权限检查阶段
8. `NGX_HTTP_POST_ACCESS_PHASE`: 访问权限检查提交阶段
9. `NGX_HTTP_TRY_FILES_PHASE`: 配置项`try_files`处理阶段
10. `NGX_HTTP_CONTENT_PHASE`: 内容产生阶段
11. `NGX_HTTP_LOG_PHASE`: 日志模块处理阶段

