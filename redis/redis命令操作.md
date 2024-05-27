# redis命令操作
## 常用命令
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
###
```shell
C:\Windows\system32>redis-cli -a 135789
127.0.0.1:6379> client list # 获取连接到服务器的客户端连接列表
id=2 addr=127.0.0.1:3291 fd=7 name= age=7 idle=0 flags=N db=0 sub=0 psub=0 multi=-1 qbuf=0 qbuf-free=32768 obl=0 oll=0 omem=0 events=r cmd=client
```
## redis事务（一组命令的集合）
### 事务
```shell
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set name weipulei
QUEUED
127.0.0.1:6379> get name
QUEUED
127.0.0.1:6379> exec
1) OK
2) "weipulei"
```
```shell
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set age 12
QUEUED
127.0.0.1:6379> discard # 取消事务
OK
```
```shell
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set age 12
QUEUED
127.0.0.1:6379> ssssssss
(error) ERR unknown command 'ssssssss'
127.0.0.1:6379> get age 12
(error) ERR wrong number of arguments for 'get' command
127.0.0.1:6379> get age
QUEUED
127.0.0.1:6379> exec # 出现命令型错误，所有命令都不执行
(error) EXECABORT Transaction discarded because of previous errors.
```
### watch （乐观锁思想）
可以用WATCH命令来监视一个或多个键，如果在事务执行之前这些键被其他命令所改动，那么事务将被打断。
```shell
127.0.0.1:6379> watch price 10 #监视price变量
OK
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set price 10
QUEUED
127.0.0.1:6379> get price
QUEUED
127.0.0.1:6379> incr price
QUEUED
# 另一个客户端执行 decr price
127.0.0.1:6379> exec
(nil) # 事务未执行
127.0.0.1:6379> get price
"9"
```