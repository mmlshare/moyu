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



### 2.3. Map 字典



### 2.4. Set 集合



### 2.5. Sort-Set 有序集

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
