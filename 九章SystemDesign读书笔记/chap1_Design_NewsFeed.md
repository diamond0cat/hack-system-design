- 数据库： 适合存储结构化数据
- 文件系统：适合存储非结构化数据(文件，图片，视频等)
- cache:是一个相对概念，可以在内存中，磁盘中，CPU中(L1 cache / L2 cache),可以在服务器端，可以在客户端(如浏览器cache)
- 内存中的cache可以理解为一个hash table, 是一种key-value结构，我们把经常访问的数据放在cache里来加速访问速度
- cache应为空间受限制，因此需要淘汰掉一些不常用的数据(常见的淘汰算法LRU)

- memcached vs redis: memcached所有的值都是简单的字符串，redis支持更丰富的数据类型。redis支持数据持久化，需要读写磁盘，所以memcached的速度比redis快很多，redis支持数据的备份，既master-slave模式的数据备份，s使用底层模型不同，他们之间底层实现方式以及客户端之间通信的应用协议不一样，redis自己构建了VM管理机制，value大小不同，redis最大可以达到512MB, 而memcached只有1MB.
  

- s3是网络文件系统:提供网络服务，可以通过web传输存储文件，提供rest接口
- dynamo db是数据库


系统设计面试怎么打分的
-

- 一个很重要的能力是trade-off的能力:各种设计方案的pros and cons


- 什么叫做SOA, pull model& push model; 数据存储系统有哪些，什么样的数据适合什么样的数据存储系统；什么是异步任务和消息队列？什么事数据的可持久化?去标准化？thundering herd?  有哪些与newsfeed类似的系统设计问题？



- memcached不支持队列结构

![20211013231111](https://raw.githubusercontent.com/corykingsf/hack-interview-handbook/main/image/20211013231111.png)


- redis可以做消息队列： redis可以实现消息中间件，可以根据列表类型的存储结构实现队列：
  - 生产者命令 LPUSH key value[value...] RPUSH key value[value...]
  - 消费者命令： RPOP key 或者 BRPOP key timeout, LPOP key 或者BLPOP key timeout
  - 阻塞消费者命令： timeout为0时，不设置超时时间，一直等待，直到有消息，如果设置数字，则单位为秒


![20211013232119](https://raw.githubusercontent.com/corykingsf/hack-interview-handbook/main/image/20211013232119.png)