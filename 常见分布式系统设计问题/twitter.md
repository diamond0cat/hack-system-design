- clarification:
    how to ask questions?明确需求和scale 需要哪些功能？系统规模多大？


### 一个常见的错误：关键词大师

- 正确：沟通去细化

![20211014000235](https://raw.githubusercontent.com/corykingsf/hack-interview-handbook/main/image/20211014000235.png)

![20211013233222](https://raw.githubusercontent.com/corykingsf/hack-interview-handbook/main/image/20211013233222.png)

- 功能： news feed

- non-funcitonal requirement:  DAU, MAU（monthly acive users）
- 一般我们用MAU衡量网站用户数的重要指标，用MAU来代表一个网站的用户数
- 我们一般不用注册用户来衡量
- MAU和DAU的比例通常是 2:1


![20211013235652](https://raw.githubusercontent.com/corykingsf/hack-interview-handbook/main/image/20211013235652.png)

- functional requirements:
  - post a tweet
  - tiemline
  - news feed
  - follow / unfollow
  - register / login

![20211013235909](https://raw.githubusercontent.com/corykingsf/hack-interview-handbook/main/image/20211013235909.png)



- 常用的估算有用的数字：
```
1M = 10 ^ 6   百万
1 lakh = 10 ^ 5  十万
1k = 10 ^ 3    千

150M * 60 / 86400 约等于 1.5 × 10^8 * 60 / 10^5 约等于  100k
这里估计的每个用户的请求次数是 60，只要合理就行，这些数据都是猜的，过程是比较重要的

峰值再triple
如果是快速增长的产品，再double  


分析QPS有什么用？
    qps = 1k 用一台web serer

一台web server: 1k qps
一台SQL database: 1K qps
一台NoSQL database (cassandra) 约承受量是 10K QPS
一台Nosql database (Memcached)约承受量是1M 的QPS

```