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
	// RedisPipeLine()
	// RedisWatch()
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

## 事务操作
~~~go
func RedisPipeLine() {
	//方法一
	{
		// 使用 Pipelined 执行事务
		_, err := DB.Pipelined(func(pipe redis.Pipeliner) error {
			// 在事务中执行命令
			pipe.Set("key1", "value1", 0)
			pipe.Set("key2", "value2", 0)
			pipe.Incr("counter")
			return nil
		})
		if err != nil {
			fmt.Println("事务执行失败:", err)
		} else {
			fmt.Println("事务执行成功")
		}
		// 获取事务中的值
		val1, _ := DB.Get("key1").Result()
		val2, _ := DB.Get("key2").Result()
		counter, _ := DB.Get("counter").Int64()

		fmt.Println("key1:", val1)
		fmt.Println("key2:", val2)
		fmt.Println("counter:", counter)
	}
	// 方法二
	{
		// 开始事务
		tx := DB.TxPipeline()

		// 在事务中执行命令
		tx.Set("key1", "value1", 0)
		tx.Set("key2", "value2", 0)
		tx.Incr("counter")

		// 执行事务
		_, err := tx.Exec()
		if err != nil {
			fmt.Println("事务执行失败:", err)
		} else {
			fmt.Println("事务执行成功")
		}

		// 获取事务中的值
		val1, _ := DB.Get("key1").Result()
		val2, _ := DB.Get("key2").Result()
		counter, _ := DB.Get("counter").Int64()

		fmt.Println("key1:", val1)
		fmt.Println("key2:", val2)
		fmt.Println("counter:", counter)
	}
}
~~~
## watch命令
~~~go
func RedisWatch() {
	DB.Watch(func(tx *redis.Tx) error {
		_, err := tx.Pipelined(func(pipe redis.Pipeliner) error {
			pipe.Set("price", 100, 0)
			return nil
		})
		if err != nil {
			fmt.Println("事务不成功")
			return err
		}
		fmt.Println(tx.Get("price").Int())
		return nil
	}, "price")
}
~~~



