

### System Design Framework

* Purpose of the interview

The architecture interview assesses your ability to take an open ended problem and design a system or architecture to solve the problem. It is not meant to see if you're able to design the 100% perfect solution; instead it assesses your ability to drill down into a complex problem, talk through multiple options and discuss tradeoffs to find a workable solution. Often times this interview—in tandem with the manager or behavioral interview—is used to decide the level at which you are hired.

* Suggested approach
* Step 1: Define your problem space

These problems are usually left vague and it's up to you as the interviewee to discuss the problem space with your interviewer and all the constraints of the system. The following are some example questions to ask your interviewer before jumping into the design phase:

* What features of this system/product do we need to build?
  * This will help focus your thinking on the important parts. What features are in scope? Which are out of scope?
* At what scale will this system need to work?
  * How many requests does our server need to handle? How many users does our app need to support?
* Under what conditions should this system work?
  * Are there any edge cases we need to consider? Examples could include bad network connectivity, database consistency, error handling.
* What are the effects by our architectural choices?
* What kind of system will be easy for other developers to work with? What architecture will be the most future-proof when product requirements change?

### Step 2: Design your system at a high level

* Based on the constraints/features you outlined in step (1), walk through the design of each piece of the system at a high level and describe how they'll interact. Don't jump into the details too soon, or you may run out of time to get to the whole system or design something that is incompatible with the rest of your system.

High level decisions to think about:

* If it's an application, what coding framework will you use? (MVC, MVVM)
* If it involves a database, what kind of database will you use? Does it need to be sharded?
* If it involves a server and client, what kinds of APIs do you need to build?
* If it requires lots of computation, do you need to parallelize the workload?
* Will your architecture be monolithic (i.e. one service) or split into microservices?

### Step 3: Define each part of your system

* Once you have a decent high level diagram, start jumping into the details of component of the system. Define the interface between these systems: APIs between server and client, database tables and indexes, background jobs that have to be run. More likely than not, the interviewer will begin to dive into these areas and ask follow up questions, but don't rely on them to drive the entire conversation.