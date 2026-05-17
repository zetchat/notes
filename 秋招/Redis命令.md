# Redis命令

## String

#### 3.1 读写命令

- **`SET key value [EX seconds] [PX milliseconds] [NX|XX]`**：设置键值对。
  - `EX seconds`：设置过期时间（秒）。
  - `PX milliseconds`：设置过期时间（毫秒）。
  - `NX`：只在键不存在时设置。实现分布式锁的关键。
  - `XX`：只在键已存在时设置。
- **`GET key`**：获取键的值。如果键不存在，返回`nil`。
- **`GETSET key value`**：原子性地设置键的值，并返回旧值。常用于实现计数器归零或获取旧值后更新。
- **`MSET key1 value1 [key2 value2 ...]`**：批量设置多个键值对。
- **`MGET key1 [key2 ...]`**：批量获取多个键的值。
- **`DEL key [key ...]`**：删除一个或多个键。

#### 3.2 数值操作

当String存储的是数字时，可以进行原子性的增减操作。

- **`INCR key`**：将键存储的数值增加1。键不存在时会先设置为0再加1。
- **`DECR key`**：将键存储的数值减少1。
- **`INCRBY key increment`**：将键存储的数值增加指定的增量。
- **`DECRBY key decrement`**：将键存储的数值减少指定的减量。
- **`INCRBYFLOAT key increment`**：将键存储的浮点数增加指定的浮点增量。

#### 3.3 过期和持久化

- **`EXPIRE key seconds`**：设置键的过期时间（秒）。
- **`PEXPIRE key milliseconds`**：设置键的过期时间（毫秒）。
- **`TTL key`**：查看键的剩余生存时间（秒）。返回-1表示永不过期，-2表示键不存在。
- **`PTTL key`**：查看键的剩余生存时间（毫秒）。
- **`PERSIST key`**：移除键的过期时间，使其永不过期。

#### 3.4 字符串操作（较少用于后端业务逻辑，更多是运维或特定场景）

- **`APPEND key value`**：如果键存在，将值追加到原值的末尾。如果键不存在，效果等同于`SET`。
- **`STRLEN key`**：返回键存储的字符串值的长度（字节数）。
- **`GETRANGE key start end`**：获取字符串的子字符串（范围是闭区间，包含start和end）。
- **`SETRANGE key offset value`**：从指定偏移量开始，用新值覆盖旧值。