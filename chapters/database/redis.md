## Redis

### 使用Redis的优点

1. 速度快，数据存储在内存中，查找和操作的时间复杂度都是O\(1\)
2. 支持丰富数据类型\(字符串、散列、列表、集合、有序集合\)
3. 支持事务，操作都是原子性
4. 丰富的特性：可用于缓存、消息队列、按主键设置过期时间

### 如何保证Redis中数据都是热点数据

#### Redis提供6种数据淘汰策略

1. voltile-lru：从已设置过期时间的数据集（service.db\[i\].expires）中挑选最近最少使用的数据淘汰。
2. volatile-ttl：从已设置过期时间的数据集（service.db\[i\].expires）中挑选将要过期数据淘汰。
3. volatile-random：从已设置过期时间的数据集（service.db\[i\].expires）中任意选择数据淘汰。
4. allkeys-lru：从数据集（service.db\[i\].dict）中挑选最少使用的数据淘汰。
5. allkeys-random：从数据集（service.db\[i\].dict）中任意选择数据淘汰。
6. no-enviction：禁止驱逐数据

# Redis适用的场景

* 会话缓存（Session Cache）
* 全页缓存（FPC）
* 队列
* 排行榜/计数器
* 发布/订阅


