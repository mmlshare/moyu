# Redis

[TOC]



## 1. Redis-cli 客户端

### 1.1. 连接集群 

`redis-cli -c -p 6382 -h 192.168.10.12` -a password

### 1.2.查看内存

`info memory`

## 2. 数据类型

### 2.1.String 字符串

#### SET

- SET key value [expiration EX seconds|PX milliseconds] [NX|XX]

  - `EX seconds` ： 将键的过期时间设置为 `seconds` 秒。 执行 `SET key value EX seconds` 的效果等同于执行 `SETEX key seconds value` 。
  - `PX milliseconds` ： 将键的过期时间设置为 `milliseconds` 毫秒。 执行 `SET key value PX milliseconds` 的效果等同于执行 `PSETEX key milliseconds value` 。
  - `NX` ： 只在键不存在时， 才对键进行设置操作。 执行 `SET key value NX` 的效果等同于执行 `SETNX key value` 。
  - `XX` ： 只在键已经存在时， 才对键进行设置操作。

#### SETNX
- SETNX key value

  - 只在键 `key` 不存在的情况下， 将键 `key` 的值设置为 `value` 。

  - 若键 `key` 已经存在， 则 `SETNX` 命令不做任何动作。
- APPEND key value

  将一个`value`连接到另一个`key`的值的尾部。

#### SETEX
- SETEX key seconds value

  - 将键 `key` 的值设置为 `value` ， 并将键 `key` 的生存时间设置为 `seconds` 秒钟。

  - 如果键 `key` 已经存在， 那么 `SETEX` 命令将覆盖已有的值。

#### PSETEX
- PSETEX key milliseconds value

  将键 `key` 的值设置为 `value` ， 并将键 `key` 的生存时间设置为 `milliseconds ` 毫秒钟。

  如果键 `key` 已经存在， 那么 `PSETEX ` 命令将覆盖已有的值。

#### DECR
- DECR key

  - 为键 `key` 储存的数字值减去一。

  - 如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `DECR` 操作。

  - 如果键 `key` 储存的值不能被解释为数字， 那么 `DECR` 命令将返回一个错误。

  - 本操作的值限制在 64 位(bit)有符号数字表示之内。

#### DECRBY
- DECRBY key decrement

  - 将键 `key` 储存的整数值减去减量 `decrement` 。

  - 如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `DECRBY` 命令。

  - 如果键 `key` 储存的值不能被解释为数字， 那么 `DECRBY` 命令将返回一个错误。

  - 本操作的值限制在 64 位(bit)有符号数字表示之内。

#### INCR
- INCR key

  - 为键 `key` 储存的数字值加上一。

  - 如果键 `key` 不存在， 那么它的值会先被初始化为 `0` ， 然后再执行 `INCR` 命令。

  - 如果键 `key` 储存的值不能被解释为数字， 那么 `INCR` 命令将返回一个错误。

#### INCRBY
- INCRBY key increment

  - 为键 `key` 储存的数字值加上增量 `increment` 。

  - 如果键 `key` 不存在， 那么键 `key` 的值会先被初始化为 `0` ， 然后再执行 `INCRBY` 命令。

  - 如果键 `key` 储存的值不能被解释为数字， 那么 `INCRBY` 命令将返回一个错误。

#### INCRBYFLOAT
- INCRBYFLOAT key increment

  - 为键 `key` 储存的值加上浮点数增量 `increment` 。

  - `INCRBYFLOAT` 命令的计算结果最多只保留小数点的后十七位。

#### GET
- GET key

  获取某个键的字符串值

  - 如果键 `key` 不存在， 那么返回特殊值 `nil` ； 否则， 返回键 `key` 的值。

  - 如果键 `key` 的值并非字符串类型， 那么返回一个错误， 因为 `GET` 命令只能用于字符串值。

#### GETRANGE
- GETRANGE key start end

  - 返回键 `key` 储存的字符串值的指定部分， 字符串的截取范围由 `start` 和 `end` 两个偏移量决定 (包括 `start` 和 `end` 在内)。

  - 负数偏移量表示从字符串的末尾开始计数， `-1` 表示最后一个字符， `-2` 表示倒数第二个字符， 以此类推。

- GETSET key value

  将键 `key` 的值设为 `value` ， 并返回键 `key` 在被设置之前的旧值。

#### MGET
- MGET key [key ...]

  返回给定的一个或多个字符串键的值

#### MSET
- MSET key value [key value ...]

  同时为多个键设置值。

#### MSETNX
- MSETNX key value [key value ...]

  - 当且仅当所有给定键都不存在时， 为所有给定键设置值。

  - 即使只有一个给定键已经存在， `MSETNX` 命令也会拒绝执行对所有键的设置操作。

  - `MSETNX` 是一个原子性(atomic)操作， 所有给定键要么就全部都被设置， 要么就全部都不设置， 不可能出现第三种状态。


#### SETRANGE
- SETRANGE key offset value

  从偏移量 `offset` 开始， 用 `value` 参数覆写(overwrite)键 `key` 储存的字符串值。

#### STRLEN
- STRLEN key

  返回键 `key` 储存的字符串值的长度。



### 2.2.List 列表

```shell
  BLPOP key [key ...] timeout
  summary: Remove and get the first element in a list, or block until one is available
  since: 2.0.0

  BRPOP key [key ...] timeout
  summary: Remove and get the last element in a list, or block until one is available
  since: 2.0.0

  BRPOPLPUSH source destination timeout
  summary: Pop a value from a list, push it to another list and return it; or block until one is available
  since: 2.2.0

  LINDEX key index
  summary: Get an element from a list by its index
  since: 1.0.0

  LINSERT key BEFORE|AFTER pivot value
  summary: Insert an element before or after another element in a list
  since: 2.2.0

  LLEN key
  summary: Get the length of a list
  since: 1.0.0

  LPOP key
  summary: Remove and get the first element in a list
  since: 1.0.0

  LPUSH key value [value ...]
  summary: Prepend one or multiple values to a list
  since: 1.0.0

  LPUSHX key value
  summary: Prepend a value to a list, only if the list exists
  since: 2.2.0

  LRANGE key start stop
  summary: Get a range of elements from a list
  since: 1.0.0

  LREM key count value
  summary: Remove elements from a list
  since: 1.0.0

  LSET key index value
  summary: Set the value of an element in a list by its index
  since: 1.0.0

  LTRIM key start stop
  summary: Trim a list to the specified range
  since: 1.0.0

  RPOP key
  summary: Remove and get the last element in a list
  since: 1.0.0

  RPOPLPUSH source destination
  summary: Remove the last element in a list, prepend it to another list and return it
  since: 1.2.0

  RPUSH key value [value ...]
  summary: Append one or multiple values to a list
  since: 1.0.0

  RPUSHX key value
  summary: Append a value to a list, only if the list exists
  since: 2.2.0

```



#### LPUSH

- LPUSH key value [value …]

  将一个或多个值 `value` 插入到列表 `key` 的表头。
  
#### LPUSHX

- LPUSHX key value

  将值 `value` 插入到列表 `key` 的表头，当且仅当 `key` 存在并且是一个列表。
  
#### RPUSH

- RPUSH key value [value ...]

#### RPUSHX

- RPUSHX key value

#### LPOP

- LPOP key

#### BLPOP

- BLPOP key [key ...] timeout
  
#### RPOP

- RPOP key


#### BRPOP

- BRPOP key [key ...] timeout

#### RPOPLPUSH

- RPOPLPUS Hsource destination

#### BRPOPLPUSH

- BRPOPLPUSH source destination timeout

#### LINDEX

- LINDEX key index



#### LINSERT

- LINSERT key BEFORE|AFTER pivot value

#### LLEN

- LLEN key

#### LRANGE

- LRANGE key start stop

  返回列表 `key` 中指定区间内的元素，区间以偏移量 `start` 和 `stop` 指定。

#### LREM

- LREM key count value

  根据参数 `count` 的值，移除列表中与参数 `value` 相等的元素。

  `count` 的值可以是以下几种：

  - `count > 0` : 从表头开始向表尾搜索，移除与 `value` 相等的元素，数量为 `count` 。
  - `count < 0` : 从表尾开始向表头搜索，移除与 `value` 相等的元素，数量为 `count` 的绝对值。
  - `count = 0` : 移除表中所有与 `value` 相等的值。

#### LSET

- LSET key index value

  将列表 `key` 下标为 `index` 的元素的值设置为 `value` 。

  当 `index` 参数超出范围，或对一个空列表( `key` 不存在)进行 [LSET](http://redisdoc.com/list/lset.html#lset) 时，返回一个错误。

#### LTRIM

- LTRIM key start stop

  对一个列表进行修剪(trim)，就是说，让列表只保留指定区间内的元素，不在指定区间之内的元素都将被删除。





### 2.3.  哈希表
```shell
  HDEL key field [field ...]
  summary: Delete one or more hash fields
  since: 2.0.0

  HEXISTS key field
  summary: Determine if a hash field exists
  since: 2.0.0

  HGET key field
  summary: Get the value of a hash field
  since: 2.0.0

  HGETALL key
  summary: Get all the fields and values in a hash
  since: 2.0.0

  HINCRBY key field increment
  summary: Increment the integer value of a hash field by the given number
  since: 2.0.0

  HINCRBYFLOAT key field increment
  summary: Increment the float value of a hash field by the given amount
  since: 2.6.0

  HKEYS key
  summary: Get all the fields in a hash
  since: 2.0.0

  HLEN key
  summary: Get the number of fields in a hash
  since: 2.0.0

  HMGET key field [field ...]
  summary: Get the values of all the given hash fields
  since: 2.0.0

  HMSET key field value [field value ...]
  summary: Set multiple hash fields to multiple values
  since: 2.0.0

  HSCAN key cursor [MATCH pattern] [COUNT count]
  summary: Incrementally iterate hash fields and associated values
  since: 2.8.0

  HSET key field value
  summary: Set the string value of a hash field
  since: 2.0.0

  HSETNX key field value
  summary: Set the value of a hash field, only if the field does not exist
  since: 2.0.0

  HSTRLEN key field
  summary: Get the length of the value of a hash field
  since: 3.2.0

  HVALS key
  summary: Get all the values in a hash
  since: 2.0.0

```
#### HSET

#### HSETNX

#### HGET

#### HEXISTS
#### HDEL
#### HLEN

#### HSTRLEN
#### HINCRBY

#### HINCRBYFLOAT

#### HMSET

#### HMGET

#### HKEYS
#### HVALS
#### HGETALL
#### HSCAN



### 2.4. Set 集合

```powershell
  SADD key member [member ...]
  summary: Add one or more members to a set
  since: 1.0.0

  SCARD key
  summary: Get the number of members in a set
  since: 1.0.0

  SDIFF key [key ...]
  summary: Subtract multiple sets
  since: 1.0.0

  SDIFFSTORE destination key [key ...]
  summary: Subtract multiple sets and store the resulting set in a key
  since: 1.0.0

  SINTER key [key ...]
  summary: Intersect multiple sets
  since: 1.0.0

  SINTERSTORE destination key [key ...]
  summary: Intersect multiple sets and store the resulting set in a key
  since: 1.0.0

  SISMEMBER key member
  summary: Determine if a given value is a member of a set
  since: 1.0.0

  SMEMBERS key
  summary: Get all the members in a set
  since: 1.0.0

  SMOVE source destination member
  summary: Move a member from one set to another
  since: 1.0.0

  SPOP key [count]
  summary: Remove and return one or multiple random members from a set
  since: 1.0.0

  SRANDMEMBER key [count]
  summary: Get one or multiple random members from a set
  since: 1.0.0

  SREM key member [member ...]
  summary: Remove one or more members from a set
  since: 1.0.0

  SSCAN key cursor [MATCH pattern] [COUNT count]
  summary: Incrementally iterate Set elements
  since: 2.8.0

  SUNION key [key ...]
  summary: Add multiple sets
  since: 1.0.0

  SUNIONSTORE destination key [key ...]
  summary: Add multiple sets and store the resulting set in a key
  since: 1.0.0

```



#### SADD

#### SISMEMBER
#### SPOP
#### SRANDMEMBER
#### SREM
#### SMOVE
#### SCARD
#### SMEMBERS
#### #### SSCAN
#### SINTER
#### SINTERSTORE
#### SUNION
#### SUNIONSTORE
#### SDIFF
#### SDIFFSTORE

### 2.5. Sorted-Set 有序集

```shell
  BZPOPMAX key [key ...] timeout
  summary: Remove and return the member with the highest score from one or more sorted sets, or block until one is available
  since: 5.0.0

  BZPOPMIN key [key ...] timeout
  summary: Remove and return the member with the lowest score from one or more sorted sets, or block until one is available
  since: 5.0.0

  ZADD key [NX|XX] [CH] [INCR] score member [score member ...]
  summary: Add one or more members to a sorted set, or update its score if it already exists
  since: 1.2.0

  ZCARD key
  summary: Get the number of members in a sorted set
  since: 1.2.0

  ZCOUNT key min max
  summary: Count the members in a sorted set with scores within the given values
  since: 2.0.0

  ZINCRBY key increment member
  summary: Increment the score of a member in a sorted set
  since: 1.2.0

  ZINTERSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM|MIN|MAX]
  summary: Intersect multiple sorted sets and store the resulting sorted set in a new key
  since: 2.0.0

  ZLEXCOUNT key min max
  summary: Count the number of members in a sorted set between a given lexicographical range
  since: 2.8.9

  ZPOPMAX key [count]
  summary: Remove and return members with the highest scores in a sorted set
  since: 5.0.0

  ZPOPMIN key [count]
  summary: Remove and return members with the lowest scores in a sorted set
  since: 5.0.0

  ZRANGE key start stop [WITHSCORES]
  summary: Return a range of members in a sorted set, by index
  since: 1.2.0

  ZRANGEBYLEX key min max [LIMIT offset count]
  summary: Return a range of members in a sorted set, by lexicographical range
  since: 2.8.9

  ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
  summary: Return a range of members in a sorted set, by score
  since: 1.0.5

  ZRANK key member
  summary: Determine the index of a member in a sorted set
  since: 2.0.0

  ZREM key member [member ...]
  summary: Remove one or more members from a sorted set
  since: 1.2.0

  ZREMRANGEBYLEX key min max
  summary: Remove all members in a sorted set between the given lexicographical range
  since: 2.8.9

  ZREMRANGEBYRANK key start stop
  summary: Remove all members in a sorted set within the given indexes
  since: 2.0.0

  ZREMRANGEBYSCORE key min max
  summary: Remove all members in a sorted set within the given scores
  since: 1.2.0

  ZREVRANGE key start stop [WITHSCORES]
  summary: Return a range of members in a sorted set, by index, with scores ordered from high to low
  since: 1.2.0

  ZREVRANGEBYLEX key max min [LIMIT offset count]
  summary: Return a range of members in a sorted set, by lexicographical range, ordered from higher to lower strings.
  since: 2.8.9

  ZREVRANGEBYSCORE key max min [WITHSCORES] [LIMIT offset count]
  summary: Return a range of members in a sorted set, by score, with scores ordered from high to low
  since: 2.2.0

  ZREVRANK key member
  summary: Determine the index of a member in a sorted set, with scores ordered from high to low
  since: 2.0.0

  ZSCAN key cursor [MATCH pattern] [COUNT count]
  summary: Incrementally iterate sorted sets elements and associated scores
  since: 2.8.0

  ZSCORE key member
  summary: Get the score associated with the given member in a sorted set
  since: 1.2.0

  ZUNIONSTORE destination numkeys key [key ...] [WEIGHTS weight] [AGGREGATE SUM|MIN|MAX]
  summary: Add multiple sorted sets and store the resulting sorted set in a new key
  since: 2.0.0

```



#### ZADD
#### ZSCORE

#### ZINCRBY
#### ZCARD
#### ZCOUNT
#### ZRANGE
#### ZREVRANGE
#### ZRANGEBYSCORE
#### ZREVRANGEBYSCORE
#### ZRANK
#### ZREVRANK
#### ZREM
#### ZREMRANGEBYRANK
#### ZREMRANGEBYSCORE
#### ZRANGEBYLEX
#### ZLEXCOUNT
#### ZREMRANGEBYLEX
#### ZSCAN
#### ZUNIONSTORE
#### ZINTERSTORE
### 2.6. 位图

#### SETBIT 

- SETBIT key offset value

  - 对 `key` 所储存的字符串值，设置或清除指定偏移量上的位(bit)。
- 位的设置或清除取决于 `value` 参数，可以是 `0` 也可以是 `1` 。
#### BITCOUNT
- BITCOUNT key [start end]

  计算给定字符串中，被设置为 `1` 的比特位的数量

#### GETBIT
- GETBIT key offset

  - 对 `key` 所储存的字符串值，获取指定偏移量上的位(bit)。

  - 当 `offset` 比字符串值的长度大，或者 `key` 不存在时，返回 `0` 。

#### BITFIELD

- BITFIELD key [GET type offset] [SET type offset value] [INCRBY type offset increment] [OVERFLOW WRAP|SAT|FAIL]

  比特位操作集合，返回值为数组

  - **GET** `<type> <offset>`- 返回指定的位域。
  - **SET**  `<type> <offset> <value>`- 设置指定的位域并返回其旧值。
  - **INCRBY**  `<type> <offset> <increment>`- 递增或递减（如果给定负递增）指定的位域并返回新值。
  - **OVERFLOW** `[WRAP|SAT|FAIL]`- 设置`INCRBY`溢出行为
    - `WRAP`环绕，包含有符号和无符号整数。
    - `SAT` 使用饱和算术，即在下溢时将该值设置为最小整数值，并在溢出时将其设置为最大整数值。
    - `FAIL`在这种模式下，没有检测到溢出或下溢操作。相应的返回值设置为 NULL，以向调用者发送信号。

#### BITOP
- BITOP operation destkey key [key ...]

  对一个或多个保存二进制位的字符串 `key` 进行位元操作，并将结果保存到 `destkey` 上。

  `operation` 可以是 `AND` 、 `OR` 、 `NOT` 、 `XOR` 这四种操作中的任意一种：

  - `BITOP AND destkey key [key ...]` ，对一个或多个 `key` 求逻辑并，并将结果保存到 `destkey` 。
  - `BITOP OR destkey key [key ...]` ，对一个或多个 `key` 求逻辑或，并将结果保存到 `destkey` 。
  - `BITOP XOR destkey key [key ...]` ，对一个或多个 `key` 求逻辑异或，并将结果保存到 `destkey` 。
  - `BITOP NOT destkey key` ，对给定 `key` 求逻辑非，并将结果保存到 `destkey` 。

#### BITPOS
- BITPOS key bit [start] [end]

  返回位图中第一个值为 `bit` 的二进制位的位置。
