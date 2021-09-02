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
* 

![picture 3](https://i.loli.net/2021/09/03/oH4mBP3i8YNL7se.png)  
