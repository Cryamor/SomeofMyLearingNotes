# Redis

## Redis基本知识

- Remote Dictionary Server

- NoSQL（Not-only SQL，非关系型数据库），Key-Value

- 单进程单线程，线程安全，采用IO多路复用机制

- 5种数据类型：

  | 数据类型          | 实现                                                         | 特性                                                         | 场景                                           |
  | ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ---------------------------------------------- |
  | String            | 二进制数据                                                   | 二进制安全，可以存储任意数据（文本、图片、音频、视频 ）；支持各种字符串操作 | 缓存、计数器、会话管理                         |
  | Hash              | map<key, value>                                              | 适合存储对象，可以像只修改其中一项属性                       | 存储用户属性、商品信息、配置数据               |
  | List              | 双向链表                                                     | 增删快；提供操作某一元素的API                                | 消息队列、任务队列、新闻推送                   |
  | Set               | 哈希表                                                       | 无序无重复；增删查效率都是O(1)；同时提供集合运算操作（交集、并集、差集） | 标签系统、共同好友、去重操作、访问网站的所有IP |
  | Sorted Set (Zset) | 哈希表（存放成员到score的映射）+跳表（存放成员，查找效率高） | 有序无重复；可根据分数范围或元素值进行范围查询               | 排行榜、时间线、带权重的消息队列               |

- 

## 安装配置

[https://github.com/tporadowski/redis/releases](https://github.com/tporadowski/redis/releases)

下载压缩包并解压后，先启动`redis-server.exe`，然后启动`redis-cli.exe`

## 基本命令

### Key命令

```bash
127.0.0.1:6379> keys * # 查看库中所有key
1) "name"
2) "name2"
127.0.0.1:6379> randomkey # 随机返回一个key
"name"
127.0.0.1:6379> exists name # 当前key是否存在
(integer) 1
127.0.0.1:6379> move name 1 # 移动key='name'的数据到库1（Redis默认16个库，用户默认用库0）
(integer) 1
127.0.0.1:6379> select 1 # 切换至库1
OK
127.0.0.1:6379[1]> keys *
1) "name"
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> keys *
1) "name2"
127.0.0.1:6379> rename name2 name1 # 重命名key
OK
127.0.0.1:6379> type name1 # 查看value数据类型
string
127.0.0.1:6379> del name1 # 删除key，空格分隔以连续删除多条
(integer) 1
127.0.0.1:6379> flushall # 清空所有库的内容
OK
```
```bash
127.0.0.1:6379> expire name 15 # 设置该key过期时间，以秒为单位
(integer) 1
127.0.0.1:6379> ttl name # 查看剩余生命周期时间
(integer) 11
127.0.0.1:6379> persist name # 移除过期时间
(integer) 1
127.0.0.1:6379> ttl name
(integer) -1 # -1表示未设置过期时间 
127.0.0.1:6379> pexpire name 100 # 设置该key过期时间，以毫秒为单位
(integer) 1
127.0.0.1:6379> ttl name
(integer) -2 # -2表示已经过期  
127.0.0.1:6379> get name
(nil)
```
### 配置

```bash
127.0.0.1:6379> ping # 测试连接是否正常，正常返回PONG
PONG
127.0.0.1:6379>config get * # 查看所有配置，*改为配置名称可单独查看
127.0.0.1:6379> config set loglevel debug # 将日志等级改为debug，日志等级还包括verbose(默认)、warning、notice
```

### String

```bash
127.0.0.1:6379> set name zhangsan # 添加一个key='name'，value='zhangsan'的数据
OK
127.0.0.1:6379> setnx name lisi # 若key不存在则等同set，若存在则无操作并返回0
(integer) 0
127.0.0.1:6379> get name # 查询key='name'的value
"zhangsan"
127.0.0.1:6379> keys *
1) "name"
127.0.0.1:6379> set name1 lisi # 插入字符串若含有空格，则需要双引号
OK
127.0.0.1:6379> strlen name1 # 查看字符串长度
(integer) 4
127.0.0.1:6379> append name1 wu # 追加，返回追加后字符串长度
(integer) 6
127.0.0.1:6379> setrange name1 5 liu # 从偏移量位置填充，若偏移量大于原长度则用\x00填充
(integer) 8
127.0.0.1:6379> get name1
"lisiwliu"
```

批量操作：

```bash
127.0.0.1:6379> mset k1 v1 k2 v2 k3 v3 # 批量set
OK
127.0.0.1:6379> keys *
1) "k2"
2) "k1"
3) "k3"
127.0.0.1:6379> mget k1 k2 k3 # 批量get
1) "v1"
2) "v2"
3) "v3"
127.0.0.1:6379> msetnx k1 v2 k4 v4 # 原子操作，要么都成功要么都失败
(integer) 0
127.0.0.1:6379> keys *
1) "k2"
2) "k1"
3) "k3"
```

```bash
127.0.0.1:6379> mset num 50 num1 num
OK
127.0.0.1:6379> decr number # value--
(integer) 49
127.0.0.1:6379> decr num1
(error) ERR value is not an integer or out of range
127.0.0.1:6379> decrby num 5 # value减去给定值
(integer) 44
127.0.0.1:6379> incr num # value++
(integer) 45
127.0.0.1:6379> incrby num 5 # value加上给定值
(integer) 50
```

### Hash

```bash
127.0.0.1:6379> hset user1 name zhangsan id 123 password 123456 # 设置key的<field,value>
(integer) 3
127.0.0.1:6379> hlen user1 # key的哈希表的字段数量
(integer) 3
127.0.0.1:6379> hkeys user1 # 获取key的所有field
1) "name"
2) "id"
3) "password"
127.0.0.1:6379> hvals user1 # 获取key的所有value
1) "zhangsan"
2) "123"
3) "123456"
127.0.0.1:6379> hincrby user1 id 2 # 增加指定value
(integer) 125
127.0.0.1:6379> hget user1 name # 获取指定value
"zhangsan"
127.0.0.1:6379> hexists user1 id # 当前key中是否有该field
(integer) 1
127.0.0.1:6379> hdel user1 name # 从key的哈希表中删除<field,value>
(integer) 1
127.0.0.1:6379> hgetall user1 # 获取key的所有field和value
1) "id"
2) "125"
3) "password"
4) "123456"
```

### List

```bash
127.0.0.1:6379> lpush list v1 # 左插入，返回list中的元素个数
(integer) 1
127.0.0.1:6379> lrange list 0 -1 # 查询范围
1) "v1"
127.0.0.1:6379> lpush list v2 v3 v4 # 批量
(integer) 4
127.0.0.1:6379> lrange list 0 -1 # 自左向右
1) "v4"
2) "v3"
3) "v2"
4) "v1"
127.0.0.1:6379> rpush list v0 # 右插入
(integer) 5
127.0.0.1:6379> lrange list 0 -1
1) "v4"
2) "v3"
3) "v2"
4) "v1"
5) "v0"
127.0.0.1:6379> lpop list # 左移出
"v4"
127.0.0.1:6379> rpop list # 右移出
"v0"
127.0.0.1:6379> lindex list 1 # 查询指定下标元素
"v2"
127.0.0.1:6379> llen list # 集合长度
(integer) 3
127.0.0.1:6379> lrem list 1 v1 # 移除指定value的元素1个，0也是移除1个
(integer) 1
127.0.0.1:6379> lrem list 10 v2 # 指定数量超过实际不会报错，返回移除的个数
(integer) 1
127.0.0.1:6379> rpush list v4 v5 v6 v7 v8
(integer) 6
127.0.0.1:6379> ltrim list 0 3 # 截取并留下元素
OK
127.0.0.1:6379> lrange list 0 -1
1) "v3"
2) "v4"
3) "v5"
4) "v6"
127.0.0.1:6379> rpoplpush list list2 # 移除最后（右）一个元素并插入另一个集合
"v6"
127.0.0.1:6379> lrange list2 0 -1
1) "v6"
127.0.0.1:6379> lrange list 0 -1
1) "v3"
2) "v4"
3) "v5"
```



### Set

```bash
127.0.0.1:6379> sadd set1 s1 s2 s3 s4 # 添加，可批量
(integer) 4
127.0.0.1:6379> smembers set1 # 查看所有元素
1) "s3"
2) "s2"
3) "s1"
4) "s4"
127.0.0.1:6379> sismember set1 s2 # 元素是否存在
(integer) 1
127.0.0.1:6379> scard set1 # 长度
(integer) 4
127.0.0.1:6379> srem set1 s4 # 移除指定元素
(integer) 1
127.0.0.1:6379> srandmember set1 2 # 随机抽取，不指定个数时默认1，但返回不会带序号
1) "s2"
2) "s1"
127.0.0.1:6379> spop set1 1 # 随机移除元素，不指定个数则默认1
1) "s1"
127.0.0.1:6379> smove set1 set2 s3 # 移动指定元素到另一集合
(integer) 1
127.0.0.1:6379> smembers set1
1) "s2"
127.0.0.1:6379> smembers set2
1) "s3"
```

```bash
127.0.0.1:6379> sadd seta 1 2 3 4 5
(integer) 5
127.0.0.1:6379> sadd setb 3 4 5 6 7
(integer) 5
127.0.0.1:6379> sdiff seta setb # 差集
1) "1"
2) "2"
127.0.0.1:6379> sdiff setb seta 
1) "6"
2) "7"
127.0.0.1:6379> sinter seta setb # 交集
1) "3"
2) "4"
3) "5"
127.0.0.1:6379> sunion seta setb # 并集
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "7"
```



### Zset

```bash
127.0.0.1:6379> zadd zset1 1 one 2 two 3 there # 添加
(integer) 3
127.0.0.1:6379> zrange zset1 0 -1 # 查询
1) "one"
2) "two"
3) "there"
127.0.0.1:6379> zrangebyscore zset1 0 -1 # 按score从小到大
(empty list or set)
127.0.0.1:6379> zrangebyscore zset1 -inf +inf # 
1) "one"
2) "two"
3) "there"
127.0.0.1:6379> zrevrange zset1 0 -1 # 从大到小
1) "there"
2) "two"
3) "one"
127.0.0.1:6379> zrangebyscore zset1 -inf +inf withscores # withscores
1) "one"
2) "1"
3) "two"
4) "2"
5) "there"
6) "3"
127.0.0.1:6379> zrem zset1 there # 移除指定元素
(integer) 1
127.0.0.1:6379> zcard zset1 # 长度
(integer) 2
127.0.0.1:6379> zcount zset1 0 10 # 指定区间内元素个数
(integer) 2
```



