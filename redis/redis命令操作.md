# redis命令操作

### 切换数据库+数据移动
```shell
C:\Windows\system32>redis-cli -a 135789
127.0.0.1:6379> select 1 # 选择数据库 0~15
OK
127.0.0.1:6379[2]> select 16
(error) ERR invalid DB index
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> keys * # 查看所有键值
1) "set2"
2) "squad"
3) "list"
4) "info"
5) "set1"
6) "zset"
7) "age"
127.0.0.1:6379> keys s* # 查看以s开头的所有键值
1) "set2"
2) "squad"
3) "set1"
127.0.0.1:6379> select 1 
OK
127.0.0.1:6379[1]> keys *
(empty list or set)
127.0.0.1:6379[1]> set name weipulei
OK
127.0.0.1:6379[1]> move name 4 # 将所选中的数据移动至数据库4
(integer) 1
127.0.0.1:6379[1]> keys *
(empty list or set)
127.0.0.1:6379[1]> select 4
OK
127.0.0.1:6379[4]> keys *
1) "name"
```
### 设置永不过期（如果已经过期，则设置失败）
```shell

```
```shell
127.0.0.1:6379[1]> select 4
OK
127.0.0.1:6379[4]> expire name 10
(integer) 1
127.0.0.1:6379[4]> persist name
(integer) 1
127.0.0.1:6379[4]> ttl name
(integer) -1
```
### 查看key的数据类型（string set zset list hash）

```shell
127.0.0.1:6379[4]> type name
string
```