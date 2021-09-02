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

# token bucket

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



### leaking bucket
* The leaking bucket algorithm is similar to the token bucket except that requests are processed at a fixed rate. It is usually implemented with a first-in-first-out (FIFO) queue. The algorithm works as follows:
* · When a request arrives, the system checks if the queue is full. If it is not full, the request is added to the queue.

· Otherwise, the request is dropped.

· Requests are pulled from the queue and processed at regular intervals.

Figure 4-7 explains how the algorithm works.
![picture 10](https://i.loli.net/2021/09/03/smUY69lAvpGr5uJ.png)  

Leaking bucket algorithm takes the following two parameters:

· Bucket size: it is equal to the queue size. The queue holds the requests to be processed at a fixed rate.

· Outflow rate: it defines how many requests can be processed at a fixed rate, usually in seconds.

Pros:

· Memory efficient given the limited queue size.

· Requests are processed at a fixed rate therefore it is suitable for use cases that a stable outflow rate is needed.

Cons:

· A burst of traffic fills up the queue with old requests, and if they are not processed in time, recent requests will be rate limited.

· There are two parameters in the algorithm. It might not be easy to tune them properly.

### Fix windows counter
Fixed window counter algorithm works as follows: 

· The algorithm divides the timeline into fix-sized time windows and assign a counter for each window.

· Each request increments the counter by one.

· Once the counter reaches the pre-defined threshold, new requests are dropped until a new time window starts.

Let us use a concrete example to see how it works. In Figure 4-8, the time unit is 1 second and the system allows a maximum of 3 requests per second. In each second window, if more than 3 requests are received, extra requests are dropped as shown in Figure 4-8. 

![picture 11](https://i.loli.net/2021/09/03/tPpbz1fX9eFOqVr.png)  
A major problem with this algorithm is that a burst of traffic at the edges of time windows could cause more requests than allowed quota to go through. Consider the following case:

![picture 12](https://i.loli.net/2021/09/03/HOGiaqP3Ll98TBy.png)  
In Figure 4-9, the system allows a maximum of 5 requests per minute, and the available quota resets at the human-friendly round minute. As seen, there are five requests between 2:00:00 and 2:01:00 and five more requests between 2:01:00 and 2:02:00. For the one-minute window between 2:00:30 and 2:01:30, 10 requests go through. That is twice as many as allowed requests.

Pros:

· Memory efficient.

· Easy to understand.

· Resetting available quota at the end of a unit time window fits certain use cases.

Cons:

·Spike in traffic at the edges of a window could cause more requests than the allowed quota to go through.

### sliding windows log
As discussed previously, the fixed window counter algorithm has a major issue: it allows more requests to go through at the edges of a window. The sliding window log algorithm fixes the issue. It works as follows:

· The algorithm keeps track of request timestamps. Timestamp data is usually kept in cache, such as sorted sets of Redis [8].

· When a new request comes in, remove all the outdated timestamps. Outdated timestamps are defined as those older than the start of the current time window. 

· Add timestamp of the new request to the log.

· If the log size is the same or lower than the allowed count, a request is accepted. Otherwise, it is rejected.

We explain the algorithm with an example as revealed in Figure 4-10.

![picture 13](https://i.loli.net/2021/09/03/Vsro1IKxJU5elny.png)  

In this example, the rate limiter allows 2 requests per minute. Usually, Linux timestamps are stored in the log. However, human-readable representation of time is used in our example for better readability.

· The log is empty when a new request arrives at 1:00:01. Thus, the request is allowed.

· A new request arrives at 1:00:30, the timestamp 1:00:30 is inserted into the log. After the insertion, the log size is 2, not larger than the allowed count. Thus, the request is allowed.

· A new request arrives at 1:00:50, and the timestamp is inserted into the log. After the insertion, the log size is 3, larger than the allowed size 2. Therefore, this request is rejected even though the timestamp remains in the log.

· A new request arrives at 1:01:40. Requests in the range [1:00:40,1:01:40) are within the latest time frame, but requests sent before 1:00:40 are outdated. Two outdated timestamps, 1:00:01 and 1:00:30, are removed from the log. After the remove operation, the log size becomes 2; therefore, the request is accepted.

Pros:

· Rate limiting implemented by this algorithm is very accurate. In any rolling window, requests will not exceed the rate limit.

Cons:

· The algorithm consumes a lot of memory because even if a request is rejected, its timestamp might still be stored in memory. 
### sliding window counter 


The sliding window counter algorithm is a hybrid approach that combines the fixed window counter and sliding window log. The algorithm can be implemented by two different approaches. We will explain one implementation in this section and provide reference for the other implementation at the end of the section. Figure 4-11 illustrates how this algorithm works.