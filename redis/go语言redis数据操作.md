# go语言redis数据操作
## 连接数据库
~~~go
package main

import (
	"fmt"
	"github.com/go-redis/redis"
	"time"
)

var DB *redis.Client

func RedisConnect() {
	client := redis.NewClient(&redis.Options{
		Addr:        string("127.0.0.1:6379"),
		Password:    string("135789"),
		DB:          0,
		DialTimeout: time.Second,
	})
	err := client.Ping().Err()
	if err != nil {
		panic("连接失败")
	}
	DB = client
}

func main() {
	RedisConnect()
	// RedisString()
	// RedisList()
	// RedisHash()
	// RedisSet()
	// RedisZSet()
}
~~~
## list列表操作
~~~go
func RedisList() {
DB.Del("list")
DB.RPush("list", "玛奇朵", "维普蕾")
DB.LPush("list", "闪电", "莱纳")
fmt.Println(DB.LRange("list", 0, -1).Val())
DB.LPop("list")
DB.RPop("list")
fmt.Println(DB.LRange("list", 0, -1).Val())
}
~~~
## set集合操作
~~~go
func RedisSet() {
DB.SAdd("set1", 1, 2, 3)
DB.SAdd("set2", 3, 4, 5)
fmt.Println(DB.SMembers("set1"))
fmt.Println(DB.SUnion("set1", "set2").Val())
fmt.Println(DB.SInter("set1", "set2").Val())
fmt.Println(DB.SDiff("set1", "set2").Val())
fmt.Println(DB.SIsMember("set1", 3).Val())
}
~~~
## hash哈希操作
~~~go
func RedisHash() {
DB.HSet("info", "name", "玛奇朵")
DB.HSet("info", "age", 19)
fmt.Println(DB.HGet("info", "name").Val())
fmt.Println(DB.HGetAll("info").Val())
fmt.Println(DB.HKeys("info").Val())
fmt.Println(DB.HLen("info").Val())
}
~~~
## zset有序列表操作
~~~go
func RedisZSet() {
DB.ZAdd("zset",
redis.Z{99, "玛奇朵"},
redis.Z{98, "闪电"},
redis.Z{90, "维普蕾"},
redis.Z{66, "黛烟"},
)
fmt.Println(DB.ZRange("zset", 0, -1).Val())
fmt.Println(DB.ZRevRange("zset", 0, -1).Val())
fmt.Println(DB.ZRevRangeByScore("zset", redis.ZRangeBy{Min: "70", Max: "100"}).Val())
fmt.Println(DB.ZRangeWithScores("zset", 0, -1).Val())
}
~~~