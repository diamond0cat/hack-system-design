what is rate limiter? 
* used to control the rate of traffic sent by a client or a service
* A rate limiter limits the number of clients requests allowed to be sent over a specfied period.
  
benefits of rate limiter:
* prevent resource starvation caused by DDos stack
* Reduce cost
* prevent servers from being overloaded


### understand the problem and establish design scope
client-side rate limiter or a service-side API rate limiter?
rate limiter throttle API requests based on IP or user ID
what is the scale of the system?  startup or a big company with a large user case
will the system work in a distributed environment?


is the rate limiter a separate service or should it be implemented in application code?

do we need to inform users who are throttled?

### step 1: requirements:
* accurately limit excessively requests
* low latency
* memory efficiency: use as little memory as possible
* distribued rate limiting
* expception handling: show clear exceptions to users when their requests are throttled
* high fault tolerance: if there are any problems with the rate limiter(for example: a cache server goes offline). it doesn't affect the entire system.


### step 2: propose high-level design and get buy-in
* let use keep things simple and use a basic client and server model for communication

* intuitively, you can implement a rate limiter at either the client or the server-side
  * client-side implementation: client side is an unreliable place to enforce rate limtiing because client request can easily be forged by malicious actors. moreover, we might not have control over the client implementation.
  * server-side implementation: 

![picture 4](https://i.loli.net/2021/09/03/m3haUqywWjcSCPx.png)  


![picture 5](https://i.loli.net/2021/09/03/CBvMp4X6mjbaIdT.png)  


Cloud microservices [4] have become widely popular and rate limiting is usually implemented within a component called API gateway. API gateway is a fully managed service that supports rate limiting, SSL termination, authentication, IP whitelisting, servicing static content, etc. For now, we only need to know that the API gateway is a middleware that supports rate limiting.
While designing a rate limiter, an important question to ask ourselves is: where should the rater limiter be implemented, on the server-side or in a gateway? There is no absolute answer.


### step 3: choose algorithms for rate limiting
* Rate limiting can be implemented using different algorithms, and each of them has distinct pros and cons.
· Token bucket

· Leaking bucket 

· Fixed window counter

· Sliding window log

· Sliding window counter

* Token bucket algorithm work as follows:
  * A token bucket is a container that has pre-defined capacity. Tokens are put in the bucket at preset rates periodically. Once the bucket is full, no more tokens are added.




![picture 3](https://i.loli.net/2021/09/03/oH4mBP3i8YNL7se.png)  


*  As shown in Figure 4-4, the token bucket capacity is 4. The refiller puts 2 tokens into the bucket every second. Once the bucket is full, extra tokens will overflow.
· Each request consumes one token. When a request arrives, we check if there are enough tokens in the bucket. Figure 4-5 explains how it works.

· If there are enough tokens, we take one token out for each request, and the request goes through. 

· If there are not enough tokens, the request is dropped.

![picture 7](https://i.loli.net/2021/09/03/uNXreDI7KQVJ1fB.png)  
* *Figure 4-6 illustrates how token consumption, refill, and rate limiting logic work. In this example, the token bucket size is 4, and the refill rate is 4 per 1 minute

![picture 9](https://i.loli.net/2021/09/03/qPo5s6lJuCpHTmy.png)  



The token bucket algorithm takes two parameters:

· Bucket size: the maximum number of tokens allowed in the bucket

· Refill rate: number of tokens put into the bucket every second
How many buckets do we need? This varies, and it depends on the rate-limiting rules. Here are a few examples.

· It is usually necessary to have different buckets for different API endpoints. For instance, if a user is allowed to make 1 post per second, add 150 friends per day, and like 5 posts per second, 3 buckets are required for each user.  

· If we need to throttle requests based on IP addresses, each IP address requires a bucket.

· If the system allows a maximum of 10,000 requests per second, it makes sense to have a global bucket shared by all requests.

Pros:

· The algorithm is easy to implement.

· Memory efficient.

· Token bucket allows a burst of traffic for short periods. A request can go through as long as there are tokens left.

Cons: 

· Two parameters in the algorithm are bucket size and token refill rate. However, it might be challenging to tune them properly.