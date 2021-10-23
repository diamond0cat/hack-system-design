
Horizontal scaling means that you can scale by adding more machines into your pool of resources, whereas vertical scaling means that you scaled by adding more power (CPU, RAM) to an existing machine.


we can increase system's scalability by:
Horizontal Scaling:-
 * good for load balancing
 * resilient
 * network calls(RPC)  --- slow
 * loose transaction guarantee --> data inconsistency is an issue
 * scale well as users increase

Vertical Scaling
* N/A
* single point failure
* inter process communication -- fast
* consistent
* hardware limit


![20210801084732](https://i.loli.net/2021/08/01/n463rzjZdS1QmgY.png)


### Deciding how to scale
The decision to scale horizontally vs. vertically depends on a number of factors.

With horizontal scaling, you can benefit by keeping your current resources running. A new machine can be brought online independently of the current infrastructure and be added to the resource pool when it is ready. This is not the case with vertical scaling as upgrading the current system will need a period of downtime to apply these changes.

While vertical scaling can benefit from a smaller physical footprint (which in turn may be more environmentally friendly), solely upgrading specs leads to a ceiling on scalability. In contrast, the limits of adding new machines lie more with cost ​than with capacity.

Which is right for you? That depends, but you should take great care when choosing how to scale. In particular, consider the long-term goals and how any decisions in the short-term can affect this plan.

If you enjoyed reading this post, consider reading some of my other definition posts:

Denormalize：Denormalization is a strategy used on a previously-normalized database to increase performance. In computing, denormalization is the process of trying to improve the read performance of a database, at the expense of losing some write performance, by adding redundant copies of data or by grouping data.

Normalize：Normalization is the process of reorganizing data in a database so that it meets two basic requirements:

There is no redundancy of data (all data is stored in only one place).

Data dependencies are logical (all related data items are stored together).

Propagate：propagate is “to send out or spread light or sound waves, movement, etc., or to be sent out or spread”， What it basically means，To spread or to flow

You may hear propagate being used when talking about changing a data structure or the features within a parent component. For instance, a change is said to 'propagate’ through to the child components that are dependent on a feature which is changing higher up in the application