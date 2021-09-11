Asked by Altassian on 9/2/2021

design distributed job scheduler:

why use redis instead of database?

could you explain how worker execute the job?


input:  job definition, timestamp
client create a job by calling our api, then we need to execute job at customer defined timestamp.

requirement没有clarify好
数据库的trade off 没有defense

https://leetcode.com/discuss/general-discussion/1082786/System-Design%3A-Designing-a-distributed-Job-Scheduler-or-Many-interesting-concepts-to-learn


这里先给出基于db的light 延迟任务队列： 研究一下db-scheduler的实现

乐观锁实现并发控制，基于版本号的乐观锁



