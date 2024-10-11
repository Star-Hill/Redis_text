[TOC]

# 实验三_Redis使用

## 启动和关闭Redis

### 启动Redis

进入安装目录下输入cmd命令后，在cmd窗口里输入

```cmd
redis-server.exe redis.windows.conf
```

**如果报错，依次执行第一条指令：redis-cli.exe，第二条指令：shutdown第三条指令：exit**

### 关闭Redis

```cmd
redis-server --service-stop
```

## 使用redis-cli 操作Redis

（1） 启动redis客户端，设置字符串：键为hello，值为redis。

```
>redis-cli.exe -h 127.0.0.1 -p 6379		::启动客户端
>set hello redis
```

（2） 设置列表，键为educoder-list

- a) 从列表左侧推入元素hello；
- b) 从列表右侧推入元素educoder；
- c) 从列表右侧推入元素bye。

```cmd
127.0.0.1:6379> lpush educoder-list hello
(integer) 1
127.0.0.1:6379> rpush educoder-list educoder
(integer) 2
127.0.0.1:6379> rpush educoder-list bye
```

（3） 设置集合，键为educoder-set

- a) 添加元素C；
- b) 添加元素Python；
- c) 添加元素Redis；
- d) 删除元素C。

```cmd
127.0.0.1:6379> sadd educoder-set C
(integer) 1
127.0.0.1:6379> sadd educoder-set Python
(integer) 1
127.0.0.1:6379> sadd educoder-set Redis
(integer) 1
127.0.0.1:6379> srem educoder-set C
```

（4） 设置哈希，键为educoder-hash

- a) 添加键：python，值为：language；
- b) 添加键：ruby，值为：language；
- c) 添加键: redis，值为：database；
- **d)** ***\*删除\****键ruby。

```cmd
127.0.0.1:6379> hset educoder-hash python language
(integer) 1
127.0.0.1:6379> hset educoder-hash ruby language
(integer) 1
127.0.0.1:6379> hset educoder-hash redis database
(integer) 1
127.0.0.1:6379> hdel educoder-hash ruby
```

（5） 设置有序列表，键为 educoder-zset

- a) 添加成员jack，分值为200；
- b) 添加成员rose，分值为400；

- c)  添加成员lee，分值为100。

```cmd
127.0.0.1:6379> zadd educoder-zset 200 jack
(integer) 1
127.0.0.1:6379> zadd educoder-zset 400 rose
(integer) 1
127.0.0.1:6379> zadd educoder-zset 100 lee
(integer) 1
```

# 实验四_使用IDEA连接Redis && 使用Java API 操作Redis 

下载插件`Redis Helper`  (不会的百度)

下载完后右边侧栏有个

![image-20241011094451009](D:\computer\tool\Typora\Typora_workplace\pic\DEEP_OS\image-20241011094451009.png)

名字随便，填入地址  127.0.0.1  端口号  6379

`建一个测试类`   类名 ： RedisKeyOperations 

```java
import redis.clients.jedis.Jedis;
import java.util.List;
import java.util.Set;

public class RedisKeyOperations {
    public static void main(String[] args) {
        Jedis jedis = new Jedis("localhost", 6379); // 连接Redis
        System.out.println("连接成功");

        // === 操作键 ===
        // 为指定键设置值
        jedis.set("key1", "value1");

        // 为多个键值设置
        jedis.mset("key2", "value2", "key3", "value3");

        // 查看所有符合给定模式的键
        Set<String> keys = jedis.keys("key*");
        System.out.println("匹配的键: " + keys);

        // 获取多个键值的对应值
        List<String> values = jedis.mget("key1", "key2", "key3");
        System.out.println("键值: " + values);

        // 删除指定键
        jedis.del("key1");
        System.out.println("key1 已删除");

        // 修改指定键
        jedis.set("key2", "newValue");
        System.out.println("key2 新值: " + jedis.get("key2"));

        // === 操作字符串 ===
        // 获取指定字符串键的旧值并设置新值
        String oldValue = jedis.getSet("key2", "updatedValue");
        System.out.println("旧值: " + oldValue);
        System.out.println("新值: " + jedis.get("key2"));

        // 获取指定字符串键的值的长度
        long length = jedis.strlen("key2");
        System.out.println("key2 的长度: " + length);

        // 获取字符串键指定索引范围的值内容
        String substring = jedis.getrange("key2", 0, 4);
        System.out.println("key2 的部分内容: " + substring);

        // 在指定字符串键的值末尾追加新内容
        jedis.append("key2", "AppendedContent");
        System.out.println("追加后的key2: " + jedis.get("key2"));

        // === 操作列 ===
        // 将一个或者多个元素推入到列表中
        jedis.lpush("list1", "element1", "element2", "element3");
        System.out.println("list1 的所有元素: " + jedis.lrange("list1", 0, -1));

        // 获取列表指定索引范围内的元素
        List<String> listItems = jedis.lrange("list1", 0, 2);
        System.out.println("索引范围0-2的元素: " + listItems);

        // 获取列表指定索引位置上的元素
        String element = jedis.lindex("list1", 1);
        System.out.println("索引1的元素: " + element);

        // 移除列表最左端的元素
        String removedElement = jedis.lpop("list1");
        System.out.println("移除的最左端元素: " + removedElement);
        System.out.println("移除后的列表: " + jedis.lrange("list1", 0, -1));

        // 获取列表中元素的个数
        long listLength = jedis.llen("list1");
        System.out.println("list1 的长度: " + listLength);

        // 移除列表中的指定元素
        jedis.lrem("list1", 1, "element2"); // 移除一个"element2"
        System.out.println("移除指定元素后的列表: " + jedis.lrange("list1", 0, -1));

        jedis.close(); // 关闭连接
    }
}

```

























