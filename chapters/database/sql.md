## SQL语句

### 数据定义

#### 定义基本表

```sql
CREATE TABLE <表名>
(<列名> <数据类型> [<列级完整性约束条件>]
[, <列名> <数据类型> [<列级完整性约束条件>]]
...
[, <表级完整性约束条件>])
```

#### 修改基本表

```sql
ALTER TABLE <表名>
[ADD <新列名> <数据类型> [<完整性约束条件>]
[DROP <完整性约束名>]
[MODIFY COLUMN <列名> <数据类型>]
```

#### 删除基本表

```sql
DROP TABLE <表名> [RESTRICT | CASCADE]
```

* `RESTRICT`：如果存真依赖该表的对象，则此表不能被删除
* `CASCADE`：在删除该表的同时，相关的依赖对象都将被一起删除

### 索引

#### 普通索引

```sql
ALTER TABLE <表名> ADD INDEX <索引名> (<列名>)
```

```sql
CREATE INDEX <表名> ON <表名> (<列名>)
```

#### 唯一索引

```sql
ALTER TABLE <表名> ADD UNIQUE <索引名> (<列名>)
```

```sql
CREATE UNIQUE INDEX <表名> ON <表名> (<列名>)
```

#### 主键索引

```sql
ALTER TABLE <表名> ADD PRIMARY KEY (<列名>)
```

#### 全文索引

```sql
ALTER TABLE <表名> ADD FULLTEXT (<列名>)
```

#### 组合索引

```sql
ALTER TABLE <表名> ADD INDEX <索引名> (<列名>, <列名>, <列名>, ...)
```

### 优化

#### 基本原则

1. 永远用小结果集驱动大结果集
2. 尽可能在索引中完成排序
3. 只取出子集需要的列
4. 使用最有效的过滤条件
5. 尽可能避免复杂的连接和子查询

