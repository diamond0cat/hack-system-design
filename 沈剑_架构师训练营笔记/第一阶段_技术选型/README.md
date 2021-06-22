## 任何脱离需求的架构设计，都是耍流氓


### 架构理念
- 架构，是能支持”高质，高效，快速，安全，低成本“系统交付的设计方法
 - 任何脱离业务的架构设计都是耍流氓
- 架构不只是设计而来的，更是进化(业务场景，流量)而来的


### 架构师干什么
- 系统，业务架构，数据库架构
- 了解系统架构，研究业务场景，


### outline
- （1）架构演进（宏观时间线-偏宏观）
  - 流量从1到10亿，架构升级，各阶段遇到的问题，以及解决方案
- （2）请求串联(微观时间线)
  - 一个http请求，端，DNS,反向代理，站点应用，服务，消息队列，缓存，数据库
- （3）数据库内核+应用
- （4）典型业务系统架构
- （5）带队成长，管理经验（从程序员，到架构师，到CTO）

### 目标
进行设计时，方向不要跑偏，立刻能够解决系统设计的问题

## 创业初期如何选型？
### 系统特点是什么？
- (1)请求量低（小于10W）
  - 平均每秒只有一两个请求
- (2) 数据量小(小于10w)
- (3)代码量小
- （4）一台机器
### 架构特点是什么？
- （1）单机系统(All in one)
- (2)程序耦合(All in one)
- (3) 逻辑核心是CRUD  

![alt txt](https://raw.githubusercontent.com/corykingsf/hack-system-design-pixel/main/imgSnipaste_2021-06-22_07-57-20.png)
### 创业早期架构特点是All in one
### 如何来选型？ 选择会的，选择熟悉的，成为了早期技术选型的主要依据


早期技术合伙人的技术水平，技术架构很重要
我会选择
PHP体系(Linux, Apache, MySQL, PHP)
JAVA体系(Linux, Tomcat, MySQl, Java)


### 这个阶段工程师的主要矛盾：写业务逻辑，CURD频繁出粗
## 如何解决这个主要矛盾？
DAO:将所有对数据源的访问操作封装到对象的接口里，需要跟数据源交互是，实际上代码跟对象交互就ok了，封装和屏蔽复杂性的过程充斥在整个系统设计中，提高工程师工程效率，快速上线
![alt txt](https://raw.githubusercontent.com/corykingsf/hack-system-design-pixel/main/imgSnipaste_2021-06-22_08-03-26.png)



## 什么时候引入开源？ 什么时候开始自研？



![alt txt](https://raw.githubusercontent.com/corykingsf/hack-system-design-pixel/main/imgSnipaste_2021-06-22_08-06-58.png)
