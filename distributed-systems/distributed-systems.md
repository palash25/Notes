# Introduction

## What is a distributed system?

A group of computers working together that to its user seems as if they are
a single computer.

## Characteristics

- These computers work concurrently
- They fail independently
- They do not share a global clock (The computing that each machine does is
  asynchronous to the other computers in the system).

Examples: Amazon.com and Cassandra DB.
A personal latop cannot be considered a dist-sys since all the peripherals
share the same data bus (global clock) and hence the opterations performed by
them are synchronous.

## Distributed Storage

Simple systems can have a single master database for their storage needs but
what if the consumers of that system grow and you are to scale the system to
handle the number of users. In this case we use read replication.

**Read Replication:** Let's say that the number of consumers of our system increase
then we can scale the system using multiple databases.

The original database remains the Master DB (leader node) and the rest are Slave DBs (follower node).

```
                                Slave A
                                --------
                               |        |
                               |        |
                                --------
                                   ^
 ----------              __________|
|          |            |
|  Master  | -----------|
|          |            |__________
 ----------                        |
                                   |
                                   V
                                --------
                               |        |
                               |        |
                                --------
                                Slave B
```

Whenever there is a change in an entry of the DB the change is made in the leader
DB and then it is propogated to the followers.
This has the following effects:
- In case of large read traffic read replication seems to solve the problem.
- This reduces the consistency of the DB meaning that an entry might not be properly
updated in the follower nodes at the time of the query but this is something that
a DistSys developer have to accept.
- This is only beneficial in case of read heavy workloads.

**Sharding:** In case of write traffic heavy workload we can have multiple masters
or divide the master DB into *shards*.

Now each master (or shard) will have its set of slave DBs and also each master DB
will contain the records for a specific range. For example in a DB of records
containing keys from A to Z, shard1 will cotain records from A to L, shard2 will
contain records from M to S and shard3 from T to Z and their corresponding slaves will
have the exact copies of the records.

This can also be seen as a multiple sets of read replications

This has the following demerits:
- More Complexities: This model also introduces the complexities of adding a feature
  to know which shard to query for the records that you want. For this another layer has
  to be added to the application or an ORM hack that will automate the process of directing
  the query to its corresponding shard.
- Limited Data Model: Sharding works well only with a key-value store. It has to be
  the case that every query we issue should have a single common key. For e.g. SaaS
  business like Trello every query will have the user-id as the key this kind of model
  is shardable.
- Limited Data Access Patterns: Lets say that we include some metadata in our key-value
  store and makes it difficult to shard. For example if we add who are the most frequent customers and have a query to fetch the most active customers that is an analytic query. For this kind of query we can go to each read replica on the shard and scatter that query on all of those and gather the results back (scatter-gather). Scatter-Gather is possible but shouldn't be considered as an option if there are a lot of such queries that reqiure scatter-gather
