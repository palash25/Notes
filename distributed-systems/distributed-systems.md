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


### CAP Theorem
**C** *onsistency*: When we do a read from a DB we should get the most recent update written.

**A** *vailability*: The DB should always be ready for read and write operation. If we try to do a read or write it shouldn't tell us that "you cannot read/write at this time".

**P** *artition* Tolerance: When a part of the distributed system or DB is disconnected or
out of sync from the rest of the system then that is called a partition.


The CAP thereom states that we can never have all three of the properties together in a
distributed system. For any given system **P** will always be there since each node in a
distributed system fails independently. So the possible combinations one can have are **CP** and **AP**. A distributed systems engineer has to choose between either C or A.

**CP**: Lets say there is a network partition and one of the components goes out of system
with the rest of the system and we choose to have consistency. If we do a read operation at
this time it wouldn't be possible since the consistent system will choose not to share any
data that might have gone out of sync with the other component rather than being available.

**AP**: In this case a read operation will get an answer from the DB which can be out of sync and wrong but since we prioritized availability we cannot get consistency.


## Distributed Transaction
**ACID** Transaction: Stands for Atomic Consistent Isolated Durable.

- Atomic: The transaction (a bundle of DB statements) either runs completely
  (all the statetments are executed) or not at all.

- Consistent: After the transaction is complete the DB is left in a valid state
  (meaning writing to the DB / when the transaction completes we don't break
   anything in the DB)

- Isolated: Different transactions when trying to access the same elements in
  a DB can't see each other. For example there is a transaction running that is
  updating a block in the DB and another transaction tries to read or write to
  the same block then the second transaction wouldn't be able to see the changes
  made by the first one until after it completes.
  Complete isolation is hard to achieve in a distributed system.

- Durable: The DB remembers everything after a transaction commits.

**ACID with an example:** Lets take the example of a coffeshop and various steps
involved in getting a customer their coffee

*Steps*
- Receive order   -------------\     
- Process Payment ------------- >========> carried out by a separate actor (waiter)   
- Enqueue order   -------------/
- Make coffee     ---------------->  Carried out by another actor
- Deliver it      ---------------->  or two individual actors (barista(s))

This work is split up to achieve *parallelization*.
This kind of system can contain *uneven workloads* too (making the coffee takes longer than the payment or deliver).

**Note:** Enqueuing is an example of distributed messaging.

*Possible failures*
- Payment failure
- Insufficient resources
- Equipment failure
- Worker failure
- Consumer failure

In case of an acid (*emphasis on the atomic part of the acronym*) transaction all the steps described in a previous section are wrapped together as one operation and if any one of the steps fail then the whole operation fails and has to roll back

*Possible Response to Failure*
- Write-off: In case of consumer failure we throw out the drink (discard the work without advancing to the next step)
- Retry: If there is an equipment or payment failure then we can always retry
- Compensating action: Customer has paid and you cannot produce the coffee then you try to compensate it by executing a different transaction (pay them back)

**A coffee shop doesn't follow dist transactional model since that would lead to a lower business or profit (lower throughput). This is why coffe shops don't follow atomic transaction to get a higher throughput and lower latency**

## Distribution Computation

Once a distributed storage has been deployed there needs to be some analysis done on the stored date. It follows the scatter gatther paradigm.

So our date is distributed amongst a network of computers (dist storage) to do some
analysis different programs are sent to these distributed storage units to do some computation on the data contained in those locally (scatter) and then the analysis is returned back (gather). [The programs are moved close to the data and ran on it]

### Map Reduce

This mathematical model consists of two functions `Map` and `Reduce`. This model
as stated earlier works on the `Scatter-Gather` principle. Process consists of
3 steps:
1. `Mapper` function: This function accepts a bunch of key-value pairs as inputs
and produces a list of key-value pairs as the output of the function. Ex- The
inputa is a file path (key) and the text content (value) contained inside the
file. When it goes through the mapper function, it tokenizes the content into
words (keys) and maps them to their number of occurences in the text (values).
This task can be distributed over a network of servers **(Scatter)**

2. `Shuffler` function: The output obtained from the `mapper` is sent as an input
to the `Shuffler` which is internal to the framework being used and the developer
has no control over it. The shuffler is responsible to move the data around. Ex-
The key-value pairs obtained from the `mapper` the ones with the common keys are
gathered from the network the values of all the common keys is collected as a list
of values at the same locality or node **(Gather)**

3. `Reducer` function: This function is responsible for doing some kind of
computation on the shuffled output. Ex- The list of values of the common keys
are added together to produce the total number of occurences of the word in the
text. This function often outputs either a scalar (or a record of scalars) or
a list of values (but the list is smaller the any of the inputs in the previous
steps)

### Hadoop

Simply put it can be termed as a map reduce API that comes with map reduce job
management and a file system (HDFS) to make it easier for developers to write map
reduce jobs.

**HDFS:** Hadoop Distributed File System.
- It can store files and directories.
- All the metadata management is done by a single replicated master (name node/server)
  all of it being stored in memory (so fast but can take a lot of space since Hadoop
  is written in Java).
- Files are stored in large, immutable and replicated blocks. The smallest block
(chunk) is a 128 MB. Files cannot be modified but only created and deleted
  (immutable)

#### Distributed storage in HDFS



                                                     ____ CLIENT APP (There can be    
                                     _______________|                 multiple apps)
                             _______|_________
                            |                 |
          _________________ |    NAME NODE    |________________
         |                  |_________________|                |
         |                            |                        |      
         |                            |                        |
         |                            |                        |
         |                            |                        |
 ________|________           _________|_______             ____|____________
|                 |         |                 |           |                 |
|    DATA NODE 1  |         |   DATA NODE 2   |           |    DATA NODE 3  |
|_________________|         |_________________|           |_________________|


- The client apps query the name node to read/write data into the data nodes.
- The name node tells the apps which data node they need to contact for the
  read/write operation requested by the app`
- Since its a DistributedSystem designed to handle multiple requests from multiple
  apps each client app will have its own connections to the data nodes.
- The app will then query the data node and request it to perform the operation.
> Let's say a new file has to be created and the app is redirected to the node3
  for this change

- The data node 3 creates the new file and writes the data sent to it.
- Once the data is written in a block inside a node it is replicated in the others,
  this is managed by the name node.
- HDFS is built for storing large chunks of data in it. That means it is not
  built for handling high velocity of queries from the apps.
- If one data node goes down then it always calls up the name node to query which
  activaties it missed while offline and replicates them.
- For replication the data nodes talk to each other and make a copy of the blocks
  from the other nodes.

#### Distributed Computation in Hadoop

                                                     ____ CLIENT APP (There can be    
                                     _______________|                 multiple apps)
                             _______|_________
                            |                 |
          _________________ |    JobTracker   |________________
         |                  |_________________|                |
         |                            |                        |      
         |                            |                        |
         |                            |                        |
         |                            |                        |
 ________|________           _________|_______             ____|____________
|                 |         |                 |           |                 |
|    DATA NODE 1  |         |   DATA NODE 2   |           |    DATA NODE 3  |
|_________________|         |_________________|           |_________________|


- In large clusters the Job Tracker and the Name Node are isolated entities (
reside on different servers)
- The client app submits a job (a map and a reduce function) to the tracker
  in the form of a jar file.
