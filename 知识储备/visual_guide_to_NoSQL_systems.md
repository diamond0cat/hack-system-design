## [Visual Guide to NoSQL Systems](https://blog.nahurst.com/visual-guide-to-nosql-systems)

There are so many NoSQL systems these days that it's hard to get a quick overview of the major trade-offs involved when evaluating relational and non-relational systems in non-single-server environments. I've developed this visual primer with quite a lot of help (see credits at the end), and it's still a work in progress, so let me know if you see anything misplaced or missing, and I'll fix it.

Without further ado, here's what you came here for (and further explanation after the visual).Note: RDBMSs (MySQL, Postgres, etc) are only featured here for comparison purposes. Also, some of these systems can vary their features by configuration (I use the default configuration here, but will try to delve into others later).

![](https://phaven-prod.s3.amazonaws.com/files/image_part/asset/607361/CausfGVcU2tskB-TR5b8CMm8Keg/medium_media_httpfarm5static_mevIk.png)

As you can see, there are three primary concerns you must balance when choosing a data management system: consistency, availability, and partition tolerance.* **Consistency** means that each client always has the same view of the data.

* **Availability** means that all clients can always read and write.
* **Partition tolerance** means that the system works well across physical network partitions.

According to the [CAP Theorem](http://www.julianbrowne.com/article/viewer/brewers-cap-theorem), you can only pick two. So how does this all relate to NoSQL systems?

One of the primary goals of NoSQL systems is to bolster horizontal scalability. To scale horizontally, you need strong network partition tolerance which requires giving up either consistency or availability. NoSQL systems typically accomplish this by relaxing relational abilities and/or loosening transactional semantics.

In addition to CAP configurations, another significant way data management systems vary is by the data model they use: relational, key-value, column-oriented, or document-oriented (there are [others](http://nosql-database.org/), but these are the main ones).* **Relational** systems are the databases we've been using for a while now. RDBMSs and systems that support ACIDity and joins are considered relational.

* **Key-value** systems basically support get, put, and delete operations based on a primary key.
* **Column-oriented** systems still use tables but have no joins (joins must be handled within your application). Obviously, they store data by column as opposed to traditional row-oriented databases. This makes aggregations much easier.
* **Document-oriented** systems store structured "documents" such as JSON or XML but have no joins (joins must be handled within your application). It's very easy to map data from object-oriented software to these systems.

Now for the particulars of each CAP configuration and the systems that use each configuration:

**Consistent, Available ** **(CA) ** **Systems** have trouble with partitions and typically deal with it with replication. Examples of CA systems include:* Traditional RDBMSs like Postgres, MySQL, etc (relational)

* Vertica (column-oriented)
* Aster Data (relational)
* Greenplum (relational)

**Consistent, Partition-Tolerant ** **(CP) ** **Systems** have trouble with availability while keeping data consistent across partitioned nodes. Examples of CP systems include:

* [BigTable](http://labs.google.com/papers/bigtable.html) (column-oriented/tabular)
* [Hypertable](http://hypertable.org/) (column-oriented/tabular)
* [HBase](http://hadoop.apache.org/hbase/) (column-oriented/tabular)
* [MongoDB](http://www.mongodb.org/display/DOCS/Home) (document-oriented)
* [Terrastore](http://code.google.com/p/terrastore/) (document-oriented)
* [Redis](http://code.google.com/p/redis/) (key-value)
* [Scalaris](http://code.google.com/p/scalaris/) (key-value)
* [MemcacheDB](http://memcachedb.org/) (key-value)
* [Berkeley DB](http://en.wikipedia.org/wiki/Berkeley_DB) (key-value)

**Available, Partition-Tolerant ** **(AP) ** **Systems** achieve "eventual consistency" through replication and verification. Examples of AP systems include:

* [Dynamo](http://s3.amazonaws.com/AllThingsDistributed/sosp/amazon-dynamo-sosp2007.pdf) (key-value)
* [Voldemort](http://project-voldemort.com/) (key-value)
* [Tokyo Cabinet](http://1978th.net/) (key-value)
* [KAI](http://sourceforge.net/projects/kai/) (key-value)
* [Cassandra](http://incubator.apache.org/cassandra/) (column-oriented/tabular)
* [CouchDB](http://couchdb.apache.org/) (document-oriented)
* [SimpleDB](http://aws.amazon.com/simpledb/) (document-oriented)
* [Riak](http://riak.basho.com/) (document-oriented)

**Self promotion and Credits**

* If you're a developer and looking for a job or if you're hiring developers and these data systems are important to you, consider coming to [Hirelite: Speed Dating for the Hiring Process](http://hirelite.com/) on Tuesday.
* This guide draws heavily from a recent [Ruby](http://www.meetup.com/nycruby/calendar/12780042/) meetup (by Matthew Jording and Michael Bryzek) and a recent [MongoDB](http://www.leadit.us/hands-on-tech/MongoDB-High-Performance-SQL-Free-Database) presentation (given by Dwight Merriman).
* Thanks to [DBNess](http://twitter.com/DBNess) and [ansonism](http://twitter.com/ansonism) for their help with validating system categorizations.
* Thanks to those who helped shape the post after it was written: [Stan](http://posterous.com/people/biCDj0Hd8), [Dwight](http://twitter.com/dmerr), and others who commented here and on this [Hacker News thread](http://news.ycombinator.com/item?id=1190772).

Update: Here's a print version of the [Visual Guide To NoSQL Systems](http://nahurst.com/pdf/Visual_Guide_To_NoSQL_Systems.pdf) if you need one quickly (warning: it's not all that pretty and I may not keep it updated, but as of 3/17/2010, it's current).
