`Most important system concerns:

𝗥𝗲𝗹𝗶𝗮𝗯𝗶𝗹𝗶𝘁𝘆
+ performs functions as expected
+ fault-tolerant / resilient
+ secure
+ high performance

it can make sense to increase the rate of faults by triggering them deliberately
> by deliberately inducing faults, you ensure that the fault-tolerance machinery is continually exercised and tested

𝗦𝗰𝗮𝗹𝗮𝗯𝗶𝗹𝗶𝘁𝘆

First, we need to succinctly describe the current load on the system

Next
- given a load and fixed resources, what will be the change in performance?
- given a load and perfomance level, what will be the change in resources needed?

Performance can mean:
- Throughput: records processed per second
- Response time: time between client request and receipt of response. 
> usually the median across all response times
> sometimes it's good to optimize slow responses since these often affect power users. but only if passes the cost-benefit analysis.
> fixing higher percentiles have diminishing returns.

head-of-line blocking: it only takes a small number of slow requests to hold up the processing of subsequent requests

Tail latency amplification: the more slow calls there are, the higher the number of future slow calls.

𝗠𝗮𝗶𝗻𝘁𝗮𝗶𝗻𝗮𝗯𝗶𝗹𝗶𝘁𝘆

majority of the cost of software is not in its initial development, but in its ongoing maintenance
> design software in such a way that it will hopefully minimize pain during maintenance

Operability
> keep operations running smoothly
Responsibilities:
- monitor system health
- restore system if it fails
- tracking down problems
- patching and updating systems
- mapping out system dependencies
- capacity planning
- creating best practices for deployment
- migrations
- maintaining system security
- documentation of operations
- automating routine tasks
- making systems predictable (X causes Y) and self-healing

Simplicity
> easy to understand
- remove accidental complexity through abstraction

Evolvability
> easy to modify

----


Standard lists need their own table for dedupe (normalization, 3rd normal form?)

Anything that is meaningful to humans may need to change sometime in the future—and if that information is duplicated, all the redundant copies need to be updated.

Network model: required knowing the path to reach a certain data point, like linked list traversal.

𝗥𝗲𝗹𝗮𝘁𝗶𝗼𝗻𝗮𝗹 𝗗𝗕
Only need to build a query optimizer once
+ better support for joins (inter-record data collection)
+ many-to-one and many-to-many relationships easier to create.
shredding—splitting a document-like structure into multiple tables
- can lead to cumbersome schemas
- unnecessarily complicated code
- needs migration if adding new fields
+ can use JSON column to create a NoSQL-like structure
+ good for simple many-to-many relationships

𝗗𝗼𝗰𝘂𝗺𝗲𝗻𝘁 𝗱𝗮𝘁𝗮𝗯𝗮𝘀𝗲 / 𝗡𝗼𝗦𝗤𝗟
- also called schemaless / schema-on-read
+ can store JSON directly
- good for CRUD of individual records, heterogenous data
- bad for reports, search or any data crunching tasks
- the work of making the join is shifted from the database to the application code
+ schema is flexible
+ better performance due to locality but only if you need the entire document
+ closer to the data structure used by the app.
- need to drill down into document to get the needed data (as opposed to a simple index query)
- difficult to keep consistent if has many-to-many relationships
- joins can be difficult
+ best for small documents with managed sizes
+ good for tree / one-to-many records
+ also good when no relationship between records

imperative - needs to be run in order
declarative - easier to work with, parallel execution possible

mapreduce: map is the retrieval function, reduce is the output
- MongoDB can use Javascript or JSON syntax, reducing the need to learn another language for frontend devs

𝗚𝗿𝗮𝗽𝗵
+ good for complex many-to-many relationships
+ see grokking for more info on graphs
+ has nodes (O) and edges (---) 
+ used in social media, web links, networks
+ can be homogeneous or heterogeneous data
+ edges are mostly relationship between nodes
+ think of it like a RDB, one table for vertices another for edges.

𝗣𝗿𝗼𝗽𝗲𝗿𝘁𝘆 𝗴𝗿𝗮𝗽𝗵
+ vertex can connect to any other vertex
+ it's easy to know each vertex edge, hence traversal is easy
+ labels help identify relationships, so easy to work with
+ easy to add new types of data
+ easy to handle data of different scope or relationship
+ easy to visualize relationships
+ easy to query: just state the vertices or edge properties you are looking for, as simple as a SELECT

Vertices
- has id
- outgoing and incoming edges (has direction)
- has key value pairs

Edges
- has id
- knows its tail and head vertices
- labels relationship between vertices
- has key value pairs

𝗧𝗿𝗶𝗽𝗹𝗲-𝗦𝘁𝗼𝗿𝗲𝘀
+ basically a property graph
+ expressed in Subject-predicate-object
+ Or HeadVertex-Edge-TailVertex
+ can also mean Subject = vertex, predicate-object = key-value

Turtle format
_:usa      a       :Location.
_:idaho    :within _:usa.
_:usa      :name   "United States".

_: = vertex
: = edge, key
string = value

semantic web
- making websites easy to graph

SPARQL
- query language for triple stores

Datalog
- similar to triple-store but as predicate(subject, object)
- good for complex data

----

log-structured storage 
- append-only
- deletions are hard
- crashes lose data
- no concurrency
+ fast write operations

Indices speed up read but slow down writes

key-value stores
- key is index
- good for frequently updated keys


SSTable
+ merging is simple
+ old values are discarded
+ hash table not in memory
+ faster seek
+ saves disk space, bandwidth
- crashes lose data (mitigate with writing logs like before)

how it works:
1. write to in-memory balanced tree, called the memtable
2. when memtable gets big, write to disk
3. read first from the memtable, then from disk in order of most recent write
4. compact regularly to clean

Log-Structured Merge-Tree / LSM-Tree
+ merged in background as well
+ can work even if dataset bigger than memory
+ can efficiently form range queries
+ high write throughput
+ faster for writes vs B-Trees
+ better compression
- compaction can interfere with read/write
- compaction might not keep up with incoming writes, reads can slow down
- full disk bandwidth can be used for the initial write if initially empty

Term dictionaries
+ key: word
+ value is list of IDs of docs that contain word / postings

Bloom filter
+ approximate contents of a set
+ used for fast seek

Compaction
+ Size-tiered: newer SSTabled are merged into older, larger ones
+ Leveled: range split up into smaller tables, older data split into levels, uses less disk space

B-Trees
+ ubiquituous
+ matches hardware operations more closely
+ sorted by key
+ broken down into blocks / page
+ has a root page that refers to other pages
+ overwrites page on disk with new data 
+ uses write-ahead log / redo log for resilience against crashes
+ can use copy-on-write for concurrency control
+ can abbreviate keys  to save space
+ additional pointers reduce seek time
+ fractal trees reduce disk seeks
+ faster for reads vs LSM-Trees
- write amplification is a problem, esp for SSDs
- can be fragmented
+ good for transactions

Secondary index
+ can be reference to a heap file but not the actual data
+ index updated to new location, or forwarding pointer is left behind

Clustered index: store indexed row directly 
ex. InnoDB

concatenated index
- multi-column index
+ useful for specific term search
+ useless for reports, getting groups of terms
+ quickly seek out across multiple dimensions

Full-text search
+ uses fuzzy querying

In-memory databases
ex. memcached, memsql, redis
+ reloads from disk / network if restarted
+ faster because no overheads related to writing to disk
+ writes to disk asynchronously
+ can use db for queues and sets
+ can use anti-caching aka replace oldest unused data with new 

Transaction processing / OLTP
+ clients make low-latency reads and writes
+ few records per query read
+ random access low latency write
+ mainly for apps
+ data is latest state
+ high availability


Batch processing
+ same as transaction processing but periodically instead of as needed

OLAP (analytics)
+ for reporting
+ aggregate data
+ ETL write
+ for decision making, analysis
+ data is history
+ optimized for analytics

Data warehousing
> make a read only copy of the db

Star schema
+ center is fact table, each row is an event
+ references dimension tables, which has event details

Snowflake schema
+ dimension tables reference subdimensions

Column oriented 
+ makes querying all data in a column faster
+ compression friendly

column-families
+ row-oriented but in column groups

vectorized processing: operated on compressed data directly

materialized aggregates: cache queries most often used like data cube or hyper cube

---


evolvability: we should aim to build systems that make it easy to adapt to change

Maintain backward and forward compatibility

translation from the in-memory representation to a byte sequence is called encoding / serialization

Encoding library issues
- Language specific
- Instantiation can be insecure
- No version control
- Inefficient

Standard data formats
XML
JSON
CSV

Issues:
- XML and CSV cannot distinguish string numbers from numeric values
- JSON doesn't distinguish integers and float
- JSON and XML don't support binary string, like Blobs. Can use Base64 encoding but increases data size by 33%
- Schemas can be complicated or not even used

For faster sending, use binary internally. BSON, etc

Tag numbers help with forward and backward compatibility.
+ cannot remove required fields
+ changing data type can risk losing precision or truncation can occur

Avro 
+ uses schema for structure
+ Seems to be the best binary option

Useful to have database of schema versions for backward and forward compatibility

----

𝗗𝗮𝘁𝗮𝗳𝗹𝗼𝘄 𝘁𝗵𝗿𝗼𝘂𝗴𝗵 𝗗𝗮𝘁𝗮𝗯𝗮𝘀𝗲𝘀
think of storing something in the database as sending a message to your future self.
+ newer code can write to it
+ while older code can read it

data outlives code.

Schema evolution thus allows the entire database to appear as if it was encoded with a single schema

Data dumps can:
+ be converted into analytics friendly form
+ can usse Avro

𝗗𝗮𝘁𝗮𝗳𝗹𝗼𝘄 𝗧𝗵𝗿𝗼𝘂𝗴𝗵 𝗦𝗲𝗿𝘃𝗶𝗰𝗲𝘀: 𝗥𝗘𝗦𝗧 𝗮𝗻𝗱 𝗥𝗣𝗖

servers expose an API over the network > clients can connect to the servers to make requests

service-oriented architecture > microservices architecture
+ easier to change 
+ easier to maintain since services are independent of each other

REST
+ emphasizes simple data format
+ URLs identify resources
+ used in microservices
+ can use OpenAPI / Swagger for documentation
+ good for experimentation
+ good for debugging (only need curl)
+ well supported

SOAP
+ XML based
+ agnostic (not just HTTP)
+ uses WSDL (not human readable)

RPC issues
- can fail due to network, speed of response and other areas out of scope
- can return wrong data due to timeout
- when retrying, can create duplicate data
- when retrying can DDoS service
- expect slow responses
- difficult to pass along large data, like file blobs
- since it is language agnostic, can have issue encoding data

assume that all the servers will be updated first, and all the clients second
> backward compatibility on requests, and 
> forward compatibility on responses


𝗠𝗲𝘀𝘀𝗮𝗴𝗲-𝗣𝗮𝘀𝘀𝗶𝗻𝗴 𝗗𝗮𝘁𝗮𝗳𝗹𝗼𝘄

Uses message broker / queue for temporary storage
+ acts as buffer is server is unavailable
+ automatic retries
+ obfuscates IP and port to sender, so easy to switch servers
+ message can have multiple recipients
+ decouple sender from recipient.
+ usually one-way. responses handled in different endpoint.
+ doesn't enforce data model

Ex. RabbitMQ, Apache Kafka

actor model is a programming model for concurrency in a single process
> logic is encapsulated in actors
- Message delivery is not guaranteed

distributed actor frameworks
+ scaled actors
+ integrates a message broker and the actor programming model into a single framework
+ Ex. Akka, Orleans, Erlang OTP


Rolling upgrades allow new versions of a service to be released without downtime
+ frequent small releases
+ deployments less risky
+ faulty releases to be detected and rolled back
---

distribute a database across multiple machines
+ Scalability to spread load
+ Fault tolerance/high availability
+ Latency to process data closer to users

2x CPU > 2x cost, but not 2x capable

shared-disk architecture
+ several machines with independent CPUs and RAM
+ stores data on an array of disks 

Shared-Nothing Architectures / Horizontal Scaling
+ each machine or virtual machine running the database software is called a node
+ Each node uses its CPUs, RAM, and disks independently
+ Any coordination between nodes is done at the software level, using a conventional network
-  incurs additional complexity for applications
- limits the expressiveness of the data models

𝗥𝗲𝗽𝗹𝗶𝗰𝗮𝘁𝗶𝗼𝗻
+ same data on several different nodes, different locations even
+ redundant
+ can improve performance
+ closer to users
+ reduce latency
+ increase availability
+ increase read throughput

Trade-offs
• synchronous or asynchronous replication
• how to handle failed replicas
• issues such as eventual consistency

how do we ensure that all the data ends up on all the replicas?

leader-based replication
1. one replica is the master / leader
2. leader receives request and writes to local storage; only leader can accept writes
3. leader sends data change to all followers as replication log /change stream
4. followers updated local copy in the same order processed by leader
5. followers can accept reads

Synchronous write
+ leader waits until follower 1 confirms write success before sending response to client
+ guaranteed to have an updated copy
- cannot process writes if has failure in follower

Asynchronous write
+ leader sends response to client without waiting for confirmation from follower
+ continues to process if followers fail /lag
- updated copies not assured

Solution: Semi-synchronous > one follower is synchronous, the rest are asynchronus. if the sync follower fails, replace with an async one.

Adding Followers
1. Take snapshot of leader from replication log, without locking if possible
2. Copy snapshot to new follower
3. Follower requests diff from leader
4. Follower catches up.

Follower failure
1. Follower keeps replication log
2. In event of crash, on recovery checks the log
3. Connects to leader to request diff.
4. Catches up.

Leader failure
1. Check if leader has really failed. Usually timeout.
2. New leader chosen from replicas, either by election or via an previously elected controller node. Must be most up-to-date
3. Point clients to new leader
4. If old leader recovers, it is now a follower

Troubleshooting:
• if new leader not sync with old leader > discard unreplicated writes, but might lead to reuse of unique keys, data leaks
• split brain / two leaders > need conflict resolution to avoid data corruption > shut down node if two leaders.
• leader dead, too long timeout > longer time to recover
• leader dead, too short timeout > failovers

# Replication methods

Statement-based replication (MySQL)
1. leader logs every write request
2. sends that statement log to its followers
+ every INSERT, UPDATE, or DELETE statement is forwarded to followers
Cons:
- nondeterministic functions (RAND(), etc) likely to generate a different value
- autoincrementing must be executed in same order as leader
- side effects can occur

Write-ahead log (WAL) shipping (PostgreSQL)
1. uses append-only log
2. can be used to create new followers

Cons
- change in database storage format version not possible

Logical (row-based) log replication (MySQL)
> sequence of records describing writes to database tables at the granularity of a row
> uses binlog
> resilient to upgrades
+ inserts: log = new values for all columns
+ deletes: log identifies deleted row (primary key or all columns if no primary key)
+ updates: log identifies changed row and new values

Trigger-based replication
Trigger: custom code that executes on data change / write (like a hook)
+ flexible
- prone to bugs
- greater overhead

read-scaling architecture
+ good if read-heavy
+ needs to be async

eventual consistency: followers will eventually have the same data as the leader in time.

𝗥𝗲𝗽𝗹𝗶𝗰𝗮𝘁𝗶𝗼𝗻 𝗹𝗮𝗴 𝗶𝘀𝘀𝘂𝗲𝘀

1. User views data shortly after write, which might not be same
solution: read-after-write consistency > always show own submission with same data (but not others)
+ reads from leader if modified by user (like when viewing own profile)
+ do not read followers that are lagging significantly from leader, based on logs
+ shows timestamp to user of latest changes
+ if geographically distributed, need to route to same datacenter as required leader

2. User views on multiple devices
solution: cross-device read-after-write
+ need to route devices to same datacenter, so same leader

3. user reads backwards in time aka data seems to be lost on refresh
solution: monotonic reads
+ do not read older data versus currently help
+ reads are made from same replica

4. responses are read before the query
solution: consistent prefix reads
+ order is same as sequnce of writes
+ any writes that are causally related to each other are written to the same partition

----

Partitioning
+ database split into smaller parts
+ parts in separate nodes

𝗠𝘂𝗹𝘁𝗶-𝗟𝗲𝗮𝗱𝗲𝗿 𝗥𝗲𝗽𝗹𝗶𝗰𝗮𝘁𝗶𝗼𝗻
> each leader simultaneously acts as a follower to the other leaders
• good for multiple data centers. replicates changes between leaders in separate data centers
+ faster writes
+ tolerant to outages
+ tolerant of network problems 
- can increase write conflicts
- issues with autoincrementing
- no defined ordering of writes; final value not clear

• good for offline operations
+ ex phone, local db acts as a leader.
+ ex CouchDB

Collaborative editing
+ involves incremental changes to avoid locking

Write conflicts
solution: asynchronous conflict detection
- too late to resolve

solution: synchronous conflict detection
> wait for the write to be replicated to all replicas >> telling the user that the write was successful
- doesn't accept writes independently

solution: conflict avoidance
> all writes for a particular record go through the same leader
> single leader if editing  your own stuff
- deal with the possibility of concurrent writes on different leaders

the database must resolve the conflict in a convergent way
> all replicas must arrive at the same final value

solutions
•  last write wins based on ID
- prone to data loss
• writes that originated at a higher-numbered replica always take precedence
- prone to data loss
• merge the values
• record conflict; fix later
• use Conflict-free replicated datatypes
• use Mergeable persistent data structures (git for dbs)
• use Operational transformation (used in Google Docs)

conflict resolution usually applies at the level of an individual row or document, not for an entire transaction

----


Multi-Leader Replication Topologies

A replication topology describes the communication paths along which writes are propagated from one node to another

all-to-all 
• leader sends its writes to every other leader.
- can lead to race conditions in write

solution: version vectors

circular topology
• each node receives writes from one node and forwards those writes (plus any writes of its own) to one other node
• To prevent infinite replication loops, each node is given a unique identifier. if change = own id, ignored
- one node failure = write failure

star topology / tree
• one designated root node forwards writes to all of the other nodes
• To prevent infinite replication loops, each node is given a unique identifier. if change = own id, ignored
- one node failure = write failure


𝗟𝗲𝗮𝗱𝗲𝗿𝗹𝗲𝘀𝘀 𝗥𝗲𝗽𝗹𝗶𝗰𝗮𝘁𝗶𝗼𝗻
> allowing any replica to directly accept writes from clients
• used for Dynamo-style dbs
• each DB returns responses to client
• read requests are also sent to several nodes in parallel

writing if node is down
solution: Read repair
> detect any stale responses
+ works well for values that are frequently read

solution: Anti-entropy process
> background process that constantly looks for differences in data
+ copies any missing data from one replica to another.
- there may be a significant delay before data is copied

Quorums for reading and writing

2 replicas > 1 updated at least

w + r > n
r and w as the minimum number of votes required for the read or write to be valid.

a smaller w and r 
+  lower latency and higher availability
- more likely to read stale values

----

By subtracting a follower’s current position from the leader’s current position, you can measure the amount of replication lag.

 and Hinted Handoff

leaderless replication
+ high availability and low latency
+ tolerate occasional stale reads

 a client that is cut off from the database nodes, they might as well be dead

Sloppy Quorums
• accept writes anyway, and write them to some nodes that are reachable but aren’t among the n nodes
+ assurance of durability

hinted handoff
• any writes that one node temporarily accepted on behalf of another node are sent to the appropriate “home” nodes

----

Multi-datacenter operation for leaderless
+ higher-latency writes to other datacenters are often configured to happen asynchronously

local is leaderless, inter datacenter is multi leader

----

Last write wins 
- If losing data is not acceptable, LWW is a poor choice for conflict resolution.

two operations are concurrent if neither happens before the other (i.e., neither knows about the other)

+ for additions, unions can resolve
+ for deletion, replace the item with a deletion marker / tombstone

Version vectors: collection of version numbers
> use a version number per replica as well as per key

----

partitions are defined in such a way that each piece of data (each record, row, or document) belongs to exactly one partition
> each partition is a small database of its own
> A node may store more than one partition.
> Each node may be the leader for some partitions and a follower for other partitions.
+ a large dataset can be distributed across many disks
+ query load can be distributed across many processors.

skewed: unfair distribution of work across partitions
hot spot: a partition with extremely high load

**Partitioning by Key Range**
> like the volumes of a paper encyclopedia
+ Within each partition, we can keep keys in sorted order >> range scans are easy
- certain access patterns can lead to hot spots
+ balance by using keys for each input device (by user, by device, etc) instead of by timestamp

**Partitioning by Hash of Key**
> use a hash function to determine the partition for a given key.
+ the hash function need not be cryptographically strong
+ consistent hashing / hash partitioning: chosen pseudorandomly, uses randomly chosen partition boundaries to avoid the need for central control or distributed consensus
  - range scans are harder
  + concatenated index: columns are used as a concatenated index for sorting the data

secondary indexes don’t map neatly to partitions
*document-based partitioning / local index*
> each partition maintains its own secondary indexes, covering only the documents in that partition
+ CRUD only in partition with secondary index
- you need to send the query to all partitions (scatter/gather)
+ Most database vendors recommend that you structure your partitioning scheme so that secondary index queries can be served from a single partition

*term-based partitioning / global index*
> term we’re looking for determines the partition of the index
> partition the index by the term itself, or using a hash of the term
+ can make reads more efficient
- writes are slower and more complicated

rebalancing: moving load from one node in the cluster to another
> when adding more CPUs for better load handling
> when adding more disks for increased data
> when machines need to be replaced.

after rebalancing:
+ load should be distributed fairly
+ database should not lock during rebalancing
+ only minimal data is moved via rebalancing

strategies
1. fixed number of partitions
> create many more partitions than there are nodes, and assign several partitions to each node.
+ if a node is added to the cluster, the new node can steal a few partitions from every existing node
+ Only entire partitions are moved between nodes.
- the number of partitions is usually fixed when the database is first set up and not changed afterward
- each partition also has management overhead (minimize number of partitions to mitigate)

2. Dynamic partitioning
> split into two partitions so that approximately half of the data ends up on each side of the split
> partition shrinks below some threshold, it can be merged with an adjacent partition
> Each partition is assigned to one node, and each node can handle multiple partitions
- an empty database starts off with a single partition

3. Partitioning proportionally to nodes
> make the number of partitions proportional to the number of nodes
> fixed number of partitions per node
+ keeps the size of each partition fairly stable

**Fully automated rebalancing**
+ convenient
- unpredictable
- can overload the network or the nodes
- harm the performance of other requests while in progress
- potentially causing a cascading failure if in combination with automatic failure detection
solution: have manual approvals

service discovery solutions (finding the right  node to connect to)
1. Allow clients to contact any node.
2. route first to a routing tier /partition-aware load balancer to determine the node
3. Require that clients be aware of the partitioning and the assignment of partitions to nodes.
+ Many distributed data systems rely on a separate coordination service (ex Zookeeper) to keep the client / balancer aware of node changes

gossip protocol: nodes to disseminate any changes in cluster state
> node forwards them to the appropriate node for the requested partition
- more complexity
+ avoids the dependency on an external coordination service

----

A transaction is a way for an application to group several reads and writes together into a logical unit.
> all the reads and writes in a transaction are executed as one operation
> created to simplify the programming model for applications accessing a database

ACID vs BASE (Basically Available, Soft state, and Eventual consistency)

𝗔𝘁𝗼𝗺𝗶𝗰𝗶𝘁𝘆 / abortability
> If the writes are grouped together into an atomic transaction, and the transaction cannot be completed (committed) due to a fault, then the transaction is aborted
> if a transaction was aborted, the application can be sure that it didn’t change anything, so it can safely be retried.

If an error occurs halfway through a sequence of writes:
+ the transaction should be aborted, and 
+ the writes made up to that point should be discarded.

𝗖𝗼𝗻𝘀𝗶𝘀𝘁𝗲𝗻𝗰𝘆
invariants: certain statements about your data that must always be true
> any writes during the transaction preserve the validity, then you can be sure that the invariants are always satisfied.
> it’s the application’s responsibility to define its transactions correctly so that they preserve consistency.

𝗜𝘀𝗼𝗹𝗮𝘁𝗶𝗼𝗻 / serializability
> concurrently executing transactions are isolated from each other
> each transaction can pretend that it is the only transaction running on the entire database.

if one transaction makes several writes, 
+ another transaction should see either all or none of those writes, but not some subset.

𝗗𝘂𝗿𝗮𝗯𝗶𝗹𝗶𝘁𝘆
> the promise that once a transaction has committed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes.
uses:
+ nonvolatile storage
+ write-ahead logs
+ multiple nodes
+ replication
+ backups

Multi-object transactions require some way of determining which read and write operations belong to the same transaction.
> everything between a BEGIN TRANSACTION and a COMMIT statement is considered to be part of the same transaction.

𝗦𝗶𝗻𝗴𝗹𝗲-𝗼𝗯𝗷𝗲𝗰𝘁 𝘄𝗿𝗶𝘁𝗲𝘀
> storage engines almost universally aim to provide atomicity and isolation on the level of a single object (such as a key-value pair) on one node
> single-object operations are useful, as they can prevent lost updates when several clients try to write to the same object concurrently

𝗺𝘂𝗹𝘁𝗶-𝗼𝗯𝗷𝗲𝗰𝘁 𝘁𝗿𝗮𝗻𝘀𝗮𝗰𝘁𝗶𝗼𝗻𝘀
+ Multi-object transactions allow you to ensure that  foreign key references remain valid
+ very useful in this situation to prevent denormalized data from going out of sync.
+ without transaction isolation, it’s possible for a record to appear in one index but not another

𝗛𝗮𝗻𝗱𝗹𝗶𝗻𝗴 𝗲𝗿𝗿𝗼𝗿𝘀 𝗮𝗻𝗱 𝗮𝗯𝗼𝗿𝘁𝘀
> if the database is in danger of violating its guarantee of atomicity, isolation, or durability, it would rather abandon the transaction entirely than allow it to remain half-finished.

if leaderless:
> the database will do as much as it can, and if it runs into an error, it won’t undo something it has already done
> it’s the application’s responsibility to recover from errors

TODO: Check if the ORM we use is ACID and retries failed transactions

Example Errors
> transaction actually succeeded, but the network failed to return response
+ retrying the transaction causes it to be performed twice
> database is overloaded
+ limit the number of retries
+ use exponential backoff
+ handle overload-related errors differently from other errors
> transient errors: deadlock, isolation, violation, temporary network interruptions, and failover
+ retry ok
> permanent error: constraint violation
+ retry useless
> transaction also has side effects outside of the database
+ side effects may happen even if the transaction is aborted
> client process fails while retrying
+ any data it was trying to write to the database is lost

Concurrency issues (race conditions) only come into play when:
> one transaction reads data that is concurrently modified by another transaction, or 
> when two transactions try to simultaneously modify the same data.
>  such bugs are only triggered when you get unlucky with the timing.


Solution: transaction isolation via serializable isolation
- Serializable isolation has a performance cost, and many databases don’t want to pay that price
- weak transaction isolation have caused substantial loss of money

𝗱𝗶𝗿𝘁𝘆 𝗿𝗲𝗮𝗱𝘀: One client reads another client’s writes before they have been committed.
𝗱𝗶𝗿𝘁𝘆 𝘄𝗿𝗶𝘁𝗲𝘀: One client overwrites data that another client has written, but not yet committed.
+ we normally assume that the later write overwrites the earlier write
+ must prevent dirty writes, usually by delaying the second write until the first write’s transaction has committed or aborted.


solution: 𝗥𝗲𝗮𝗱 𝗖𝗼𝗺𝗺𝗶𝘁𝘁𝗲𝗱
+ allows aborts 
+ prevents reading the incomplete results of transactions
+ prevents concurrent writes from getting intermingled
+ fixes dirty reads

Rules:
1. When reading from the database, you will only see data that has been committed (𝗻𝗼 𝗱𝗶𝗿𝘁𝘆 𝗿𝗲𝗮𝗱𝘀)
2. When writing to the database, you will only overwrite data that has been committed ( 𝗻𝗼 𝗱𝗶𝗿𝘁𝘆 𝘄𝗿𝗶𝘁𝗲𝘀)

> Seeing the database in a partially updated state is confusing to users and may cause other transactions to take incorrect decisions.

𝘂𝘀𝗶𝗻𝗴 𝗿𝗼𝘄-𝗹𝗲𝘃𝗲𝗹 𝗹𝗼𝗰𝗸𝘀
> hold that lock until the transaction is committed or aborted.
> Only one transaction can hold the lock for any given object
> While the transaction is ongoing, any other transactions that read the object are simply given the old value


𝗥𝗲𝗽𝗲𝗮𝘁𝗮𝗯𝗹𝗲 𝗥𝗲𝗮𝗱
 **nonrepeatable read / read skew**
 > A client sees different parts of the database at different points in time.
 BUT some situations cannot tolerate such temporary inconsistency
 - backups: writes continue to be made to the database, you could end up with some parts of the backup containing an older version of the data, and other parts containing a newer version
 - Analytic queries and integrity checks: These queries are likely to return nonsensical results if they observe parts of the database at different points in time

solution: 𝗦𝗻𝗮𝗽𝘀𝗵𝗼𝘁 𝗜𝘀𝗼𝗹𝗮𝘁𝗶𝗼𝗻 / **repeatable read** / **serializable**
> each transaction reads from a consistent snapshot of the database [all the data that was committed in the database at the start of the transaction]
> typically use write locks
> readers never block writers, and writers never block readers
+ fixes read skew
+ fixes most lost updates
+ fixes phantom reads

**multi-version concurrency control**
> maintains several versions of an object side by side
+ fixes read skew
1. transaction has a unique incremental transaction id
2. the data it writes is tagged with the transaction ID of the writer.
3. Each row in a table has a created_by field
4. each row has a deleted_by field
5. f a transaction deletes a row, it is marked for deletion by setting the deleted_by field to the ID of the transaction
6. a garbage collection process in the database removes any rows marked for deletion 

*Visibility Rules*
1. Any writes to other transactions in progress are ignored
2. Any writes made by aborted transactions are ignored.
3. Any writes made by transactions with a later transaction ID are ignored
4. All other writes are visible to the application’s queries.
5. Committed transactions are visible
6. Transations marked for deletion but not yet committed are visible
7. Transactions not marked for deletion are visible.

**Lost Updates**
read-modify-write cycle: reads some value from the database, modifies it, and writes back the modified value
> if two transactions do read-modify-write concurrently, one of the modifications can be lost, because the second write does not include the first modification

solution: **Atomic write operations**
> implemented by taking an exclusive lock on the object when it is read so that no other transaction can read it until the update has been applied
> OR simply force all atomic operations to be executed on a single thread

solution: **Explicit locking**
> explicitly lock objects that are going to be updated
+ fixes lost updates

solution: **Automatically detecting lost updates**
> if the transaction manager detects a lost update, abort the transaction and force it to retry its read-modify-write cycle.
> databases can perform this check efficiently in conjunction with snapshot isolation
> detection happens automatically and is thus less error-prone

solution: **Compare-and-set**
> allowing an update to happen only if the value has not changed since you last read it.

**Conflict resolution and replication**

multi-leader or leaderless replication usually allow several writes to happen concurrently and replicate them asynchronously
> cannot guarantee that there is a single up-to-date copy of the data

solution: allow concurrent writes to create several conflicting versions of a value, then use application code or special data structures to resolve and merge these versions after the fact.

**Write Skew**
>  A transaction reads something, makes a decision based on the value it saw, and writes the decision to the database. However, by the time the write is made, the premise of the decision is no longer true.
> a generalization of the lost update problem
- Atomic single-object operations don’t help
- not automatically detected in repeatable read

Examples:
- double booking
- duplicate username creation
- double spending

How it happens
1. Checks DB if condition is clear
2. write happens
3. write skew occurs since both happen at the same time

**Phantoms**
> A transaction reads objects that match some search condition. Another client makes a write that affects the results of that search. 
> Snapshot isolation avoids phantoms in read-only queries

solution: **materializing conflicts**,
> takes a phantom and turns it into a lock conflict on a concrete set of rows that exist in the database
> should be considered a last resort if no alternative is possible

There are no good tools to help us detect race conditions.

Testing for concurrency issues is hard

solution: **serializable isolation**
implementation
1. Actual Serial Execution: executing transactions in a serial order 
2. Two-phase locking
3. Optimistic concurrency control
+ fixes write skew

**Actual Serial Execution**
> a single-threaded loop for executing transactions was feasible
> A system designed for single-threaded execution can sometimes perform better than a system that supports concurrency, because it can avoid the coordination overhead of locking.
> equivalent to each transaction having an exclusive lock on the entire database
> compensate by making each transaction very fast to execute
+ Every transaction must be small and fast
+ limited to use cases where the active dataset can fit in memory.
+ throughput must be low enough to be handled on a single CPU core
+ fixes write skew

solution: **Encapsulating transactions in stored procedures**
> the application must submit the entire transaction code to the database ahead of time
- Each database vendor has its own language 
- ugly and archaic from today’s point of view
- lack the ecosystem of libraries that you find with most programming languages
- harder to debug
- more awkward to keep in version control and deploy
- trickier to test
- difficult to integrate with a metrics collection
- A badly written stored procedure in a database can cause much more trouble than equivalent badly written code in an application server.
- data with multiple secondary indexes is likely to require a lot of cross-partition coordination
+ avoid the overhead of other concurrency control mechanisms
+ achieve quite good throughput on a single thread
> needs to be performed in lock-step across all partitions to ensure serializability across the whole system.

**Two-Phase Locking (2PL)**
> Several transactions are allowed to concurrently read the same object as long as nobody is writing to it.
> as soon as anyone wants to write (modify or delete) an object, exclusive access is required
> pessimistic concurrency control mechanism
- transaction throughput and response times of queries are significantly worse
- unstable latencies
- deadlocks can freeze system due to retries
+ fixes write skew

implementation
1. to read an object, it must first acquire the lock in shared mode (other transactions proceed if no exclusive lock)
2. to write to an object, it must first acquire the lock in exclusive mode (no other transactions proceed)
3. If a transaction first reads and then writes an object, it may upgrade its shared lock to an exclusive lock.
4. continue to hold the lock until the end of the transaction (commit or abort)

deadlock: transaction A is stuck waiting for transaction B to release its lock, and vice versa
> database automatically detects deadlocks between transactions and aborts one of them so that the others can make progress

solution: **predicate lock**
> belongs to all objects that match some search condition
> locks based on WHERE
+ applies even to objects that do not yet exist in the database
- do not perform well
- checking for matching locks becomes time-consuming

solution: **index-range locking** / next-key locking
> approximates locking acriss entire range, column or table
> simply attach a shared lock to this index entry
> if another transaction wants to insert, update, or delete, it will have to update the same part of the index, encounter the shared lock, and it will be forced to wait until the lock is released.
+ much lower overheads
+ fixes phantom writes

 Are serializable isolation and good performance fundamentally at odds with each other?

**Serializable Snapshot Isolation**
> When a transaction wants to commit, it is checked, and it is aborted if the execution was not serializable.
> Only transactions that executed serializably are allowed to commit.
- performs badly if there is high contention
+ Contention can be reduced with commutative atomic operations
+ By avoiding unnecessary aborts, SSI preserves snapshot isolation’s support for long-running reads from a consistent snapshot.
+ one transaction doesn’t need to block waiting for locks held by another transaction.
+ transactions can read and write data in multiple partitions while ensuring serializable isolation
+  very appealing for read-heavy workloads.
+   query latency much more predictable and less variable
- requires that read-write transactions be fairly short 
+ fixes write skew

the database must detect situations in which a transaction may have acted on an outdated premise and abort the transaction in that case.

**stale MVCC reads**
Prevention
1. track when a transaction ignores another transaction’s writes due to MVCC visibility rules.
2.  When the transaction wants to commit, database checks whether any of the ignored writes have now been committed.
3. If so, the transaction must be aborted.

----

 An individual computer with good software is usually either fully functional or entirely broken, but not something in between.
 > we prefer a computer to crash completely rather than returning a wrong result, because wrong results are difficult and confusing to deal with.

 Partial Failure
 > Some parts working, others are not
 > nondeterministic performance
 > nondeterministic responses

 in a system with thousands of nodes, it is reasonable to assume that something is always broken

 to make distributed systems work, we must:
 + accept the possibility of partial failure and 
 + build fault-tolerance mechanisms

In distributed systems, suspicion, pessimism, and paranoia pay off.

**Unreliable Networks**

asynchronous packet networks
> one node can send a message (a packet) to another node, but the network gives no guarantees as to when it will arrive, or whether it will arrive at all.

**timeout**: after some time you give up waiting and assume that the response is not going to arrive
- long timeout means a long wait until a node is declared dead
- short timeout detects faults faster, but carries a higher risk of incorrectly declaring a node dead

**network fault** / Network partitions / netsplit
> When one part of the network is cut off from the rest due to a network fault

 Whenever any communication happens over a network, it may fail—there is no way around it.
 > If software is put in an unanticipated situation, it may do arbitrary unexpected things.
 > If you want to be sure that a request was successful, you need a positive response from the application itself 

**network congestion**
> On a busy network link, a packet may have to wait a while until it can get a slot

noisy neighbor: another user adjacent is using a lot of resources

datacenter networks and the internet are optimized for bursty traffic since the traffic doesn't have a fixed size; the network just want to finish the delivery as soon as possible.
> telephone lines are always on, hence always reliable but send a fixed amount of data per second

 if you use software that requires synchronized clocks, it is essential that you also carefully monitor the clock offsets between all the machines.

 Thus, it doesn’t make sense to think of a clock reading as a point in time—it is more like a range of times, within a confidence interval

** Process Pauses**

Only one node can hold the lease at any one time—thus, when a node obtains a lease, it knows that it is the leader for some amount of time, until the lease expires.
>  If the node fails, it stops renewing the lease, so another node can take over when it expires.

paging is often disabled on server machines (if you would rather kill a process to free up memory than risk thrashing).

In embedded systems, real-time means that a system is carefully designed and tested to meet specified timing guarantees in all circumstances.

“real-time” is not the same as “high-performance”
>  since they have to prioritize timely responses above all else

**Limiting the impact of garbage collection**

treat GC pauses like brief planned outages of a node, and to let other nodes handle requests from clients

----

A node in the network cannot know anything for sure—it can only make guesses based on the messages it receives
> In a distributed system, we can state the assumptions we are making about the behavior

a node cannot necessarily trust its own judgment of a situation. 
> decisions require some minimum number of votes from several nodes in order to reduce the dependence on any one particular node.

majority quorum allows the system to continue working if individual nodes have failed

**Fencing tokens**
> some kind of check is necessary to avoid processing requests outside of the lock’s protection.

**Byzantine Faults**
> a node may claim to have received a particular message when in fact it didn’t

Byzantine Generals Problem
> problem of reaching consensus in this untrusting environment

 Byzantine fault-tolerant
 > continues to operate correctly even if some of the nodes are malfunctioning and not obeying the protocol, 
 > or if malicious attackers are interfering with the network

  simply make the server the authority on deciding what client behavior is and isn’t allowed.

**Synchronous model**
> assumes bounded network delay, bounded process pauses, and bounded clock error

**Partially synchronous model**
> behaves like a synchronous system most of the time, but it sometimes exceeds the bounds for network delay, process pauses, and clock drift

**Asynchronous model**
> not allowed to make any timing assumptions

**Crash-stop faults**
> assume that a node can fail in only one way, namely by crashing

**Crash-recovery faults**
> nodes may crash at any moment, and perhaps start responding again after some unknown time
> nodes are assumed to have stable storage

**Byzantine (arbitrary) faults**
> Nodes may do absolutely anything, including trying to trick and deceive other nodes

the **partially synchronous model** with **crash-recovery faults** is generally the most useful model

**liveness**: something good eventually happens; properties often include the word “eventually” in their definition
**Safety**: nothing bad happens

distributed algorithms
> require that safety properties always hold

----

To tolerate faults, 
1. the first step is to detect them,
2. make a system tolerate it 


Most non-safety-critical systems choose cheap and unreliable over expensive and reliable.


----

**Consistency Guarantees**

A better name for eventual consistency may be convergence
- doesn’t say anything about when the replicas will converge

When working with a database that provides only weak guarantees, you need to be constantly aware of its limitations and not accidentally assume too much.

The edge cases of eventual consistency only become apparent when:
- there is a fault in the system (e.g., a network interruption) or 
- at high concurrency.

systems with stronger guarantees may have *worse performance* or be *less fault-tolerant* than systems with weaker guarantees. 

**Linearizability**
 aka atomic consistency, strong consistency, immediate consistency, or external consistency
> make a system appear as if there were only one copy of the data, 
> and all operations on it are atomic.
> is a recency guarantee
- even RAM on a modern multi-core CPU is not linearizable

strict serializability or strong one-copy serializability 
> provide both serializability and linearizability

 a hard uniqueness constraint requires linearizability

 If it is not linearizable, there is the risk of a race condition
 > Without the recency guarantee of linearizability, race conditions between these two channels are possible

*Single-leader replication*
+ If you make reads from the leader, or from synchronously updated followers, they have the potential to be linearizable.

*Consensus algorithms*
+ contain measures to prevent split brain and stale replicas.
+ are linearizable

*Multi-leader replication*
+ not linearizable because they concurrently process writes on multiple nodes and asynchronously replicate them to other nodes

*Leaderless replication*
+ Last write wins are almost certainly nonlinearizable because clock timestamps cannot be guaranteed to be consistent
- a leaderless system with Dynamo-style replication does not provide linearizability. 


If your application requires linearizability, and some replicas are disconnected from the other replicas
- some replicas cannot process requests while they are disconnected

**CAP theorem**
> applications that don’t require linearizability can be more tolerant of network problems

The reason for dropping linearizability is **performance**, not fault tolerance.

**Causality** imposes an ordering on events: cause comes before effect;

If a system obeys the ordering imposed by causality, we say that it is **causally consistent**.

A **total order** allows any two elements to be compared

In a linearizable system, we have a total order of operations
> any two operations we can always say which one happened first

two operations are concurrent if neither happened before the other
> causality defines a partial order, not a total order
> Concurrency would mean that the timeline branches and merges again
> operations on different branches are incomparable

**linearizability** implies *causality*

to maintain causality, you need to know which operation happened before which other operation.
> ensure that all causally preceding operations have already been processed;
> database needs to know which version of the data was read by the application
> can use sequence numbers or timestamps from logical clocks to order events.
> Concurrent operations may be ordered arbitrarily

**Lamport timestamps**
> simply a pair of (counter, node ID)
> if the counter values are the same, the one with the greater node ID is the greater timestamp.
> When a node receives a request or response with a maximum counter value greater than its own counter value, it immediately increases its own counter to that maximum.
> always enforce a total ordering (unlike version vectors, which are only for partial order)

in order to implement something like a uniqueness constraint
- it’s not sufficient to have a total ordering of operations
+ you also need to know when that order is finalized aka **total order broadcast**

**Total order broadcast**
> The messaging system must decide on the order in which to deliver messages.
> protocol for exchanging messages between nodes
> two safety properties 
1. *Reliable delivery*: No messages are lost: if a message is delivered to one node, it is delivered to all nodes.
2.*Totally ordered delivery*: Messages are delivered to every node in the same order.

total order broadcast can be used to implement serializable transactions
- a node is not allowed to retroactively insert a message into an earlier position in the order 
+ also useful for implementing a lock service that provides fencing tokens

**Linearizable compare-and-set registers**
> The register needs to atomically decide whether to set its value, based on whether its current value equals the parameter given in the operation.
+ has an atomic increment-and-get operation
an atomic compare-and-set operation would also do the job

1. increment-and-get the linearizable integer
2. sequence number = register value 
3. attach sequence number to message
4. send the message to all nodes
5. recipients will deliver the messages consecutively by sequence number.

linearizable register form a sequence with no gaps
> waits for missing messages

linearizable compare-and-set (or increment-and-get) register and total order broadcast are both equivalent to consensus

**Locks and leases**
> When several clients are racing to grab a lock or lease, the lock decides which one successfully acquired it.

**Consensus**
> get several nodes to agree on something

**Leader election**
> all nodes need to agree on which node is the leader.
> needed to avoid bad failover, split brain, inconsistency, data loss

**Membership/coordination service**
> Given a failure detector (e.g., timeouts), the system must decide which nodes are alive, and which should be considered dead because their sessions timed out.

**Uniqueness constraint**
> When several transactions concurrently try to create conflicting records with the same key, the constraint must decide which one to allow and which should fail with a constraint violation.

**Atomic commit**
> A database must decide whether to commit or abort a distributed transaction.
> all nodes to agree on the outcome of the transaction
> Atomicity prevents failed transactions from littering the database with half-finished results and half-updated state.
> transaction commitment crucially depends on the order in which data is durably written to disk
> once a transaction has been committed on one node, it cannot be retracted 
> a node must only commit once it is certain that all other nodes in the transaction are also going to commit.
- this principle forms the basis of *read committed isolation*

**2PC** provides atomic commit in a distributed database
**2PL** provides serializable isolation

 2PC uses a coordinator / transaction manager

1. the participant surrenders the right to abort the transaction, but without actually committing it
2. requests a transaction ID from the coordinator
3. application begins a single-node transaction on each of the participants
4. attaches the globally unique transaction ID to the single-node transaction
   1. the coordinator or any of the participants can abort if anything goes wrong at this stage
5. application is ready to commit
6. coordinator sends a prepare request to all participants tagged with the global transaction ID
   1. coordinator sends an abort request to all participants if any of these requests fails or times out
7. participant receives the prepare request
8. makes sure that it can definitely commit the transaction under all circumstances
   1. replying “yes” to the coordinator, the node promises to commit the transaction without error if requested
   2. participant surrenders the right to abort the transaction, but without actually committing it.
9. coordinator has received responses to all prepare requests
10. makes a definitive decision on whether to commit or abort the transaction (committing only if all participants voted “yes”)
11. coordinator must write that decision to its transaction log >> commit point.
12. commit or abort request is sent to all participants
    1.  If this request fails or times out, coordinator must retry forever until it succeeds. 
    2.  If a participant has crashed in the meantime, the transaction will be committed when it recovers

what happens if the coordinator crashes.

coordinator fails before sending the prepare requests
> a participant can safely abort the transaction

the commit point of 2PC comes down to a regular single-node atomic commit on the coordinator. 

**Three-phase commit**

nonblocking atomic commit requires a perfect failure detector 

distributed transactions in MySQL are reported to be *over 10 times slower* than single-node transactions 

a message from a message queue can be acknowledged as processed if and only if the database transaction for processing the message was successfully committed.
> But if all side effects of processing a message are rolled back on transaction abort, then the processing step can safely be retried as if nothing had happened.

The database cannot release those locks until the transaction commits or aborts
> a transaction must hold onto the locks throughout the time it is in doubt

orphaned in-doubt transactions do occur 
> way out is for an administrator to manually decide whether to commit or roll back the transactions
> requires a lot of manual effort
> most likely needs to be done under high stress and time pressure 
> during a serious production outage
> most likely needs to be done under high stress and time pressure during a serious production outage
+ allowing a participant to unilaterally decide to abort or commit an in-doubt transaction without a definitive decision from the coordinator 

Distributed transactions thus have a tendency of amplifying failures, which runs counter to our goal of building fault-tolerant systems.

**Fault-Tolerant Consensus**
> one or more nodes may propose values, and the consensus algorithm decides on one of those values.

Rules:
**Uniform agreement**
> No two nodes decide differently.
**Integrity**
> No node decides twice.
**Validity**
> If a node decides value v, then v was proposed by some node.
**Termination**
> Every node that does not crash eventually decides some value.
> a consensus algorithm cannot simply sit around and do nothing forever
> any consensus algorithm requires at least a majority of nodes to be functioning correctly in order to assure termination 
> the termination property is subject to the assumption that fewer than half of the nodes are crashed or unreachable

Most of these algorithms actually decide on a sequence of values, which makes them total order broadcast algorithms
> total order broadcast is equivalent to repeated rounds of consensus

guarantee that within each epoch, the leader is unique

Every time the current leader is thought to be dead, 
> a vote is started among the nodes to elect a new leader
> This election is given an incremented epoch number
> the leader with the higher epoch number prevails.
> must first check that there isn’t some other leader with a higher epoch number 
+ must collect votes from a quorum of nodes
+ A node votes in favor of a proposal only if it is not aware of any other leader with a higher epoch.
+ if a vote on a proposal succeeds, at least one of the nodes that voted for it must have also participated in the most recent leader election
- The process by which nodes vote on proposals before they are decided is a kind of synchronous replication  some committed data can potentially be lost on failover
- you need a minimum of three nodes in order to tolerate one failure
- frequent leader elections result in terrible performance because the system can end up spending more time choosing a leader than doing any useful work.

some consensus systems support read-only caching replicas
> asynchronously receive the log of all decisions
> do not actively participate in votin

----
Part 3 intro 

**Systems of record** / source of truth
>  the authoritative version of your data
> Each fact is represented exactly once 
  + the representation is typically normalized

**Derived data systems**
> the result of taking some existing data from another system and transforming or processing it in some way.
> Denormalized values, indexes, and materialized views also fall into this category
> is redundant

By being clear about which data is derived from which other data, you can bring clarity to an otherwise confusing system architecture.

----

**Services** (online systems)
> tries to handle requests as quickly as possible and sends a response back
> *Response time* is usually the primary measure of performance
> APIs, websites, etc

**Batch processing systems** (offline systems)
> takes a large amount of input data, runs a job to process it, and produces some output data
> often scheduled to run periodically
> primary *performance* measure of a batch job is usually throughput
> ETL, migrations, big data, etc

**Stream processing systems** (near-real-time systems)
> consumes inputs and produces outputs 
> operates on events shortly after they happen

**Unix philosophy**
1. Make each program do one thing well
2. Expect the output of every program to become the input to another, as yet unknown, program.
3. Design and build software, even operating systems, to be tried early
4. Use tools in preference to unskilled help (aka people) to lighten a programming task

If you want to be able to connect any program’s output to any program’s input, that means that all programs must use the same input/output interface.
> that interface is a file

all these programs have standardized on using the same record separator allows them to interoperate.

* **stdin** comes from the keyboard
* **stdout** goes to the screen

1. The input files to Unix commands are normally treated as immutable
2. You can end the pipeline at any point, pipe the output into less, and look at it to see if it has the expected form.
3. You can write the output of one pipeline stage to a file and use that file as input to the next stage. 

**HDFS (Hadoop Distributed File System)**
> based on the shared-nothing principle (requires no special hardware)
> HDFS consists of a daemon process running on each machine, exposing a network service that allows other nodes to access files stored on that machine
> file blocks are replicated on multiple machines. 
> cost of data storage and access on HDFS, using commodity hardware and open source software, is much lower 


 **MapReduce**
> MapReduce is a programming framework with which you can write code to process large datasets in a distributed filesystem like HDFS.
> can parallelize a computation across many machines, without you having to write code to explicitly handle the parallelism
> MapReduce has no concept of indexes
> separates the physical network communication aspects of the computation (getting the data to the right machine) from the application logic (processing the data once you have it)
+ easy to retry due to immutability of input
+ mistakes are reversible
+ retries are possible since input isn't affected
+ separate logic from wiring (configuring the input and output directories), which provides a separation of concerns and enables potential reuse of code
+ very robust: you can use it to process almost arbitrarily large quantities of data on an unreliable multi-tenant system with frequent task terminations, and it will still get the job done
- implementing a complex processing job using the raw MapReduce APIs is actually quite hard and laborious
 A single MapReduce job is comparable to a single Unix process.
 -  other tools are sometimes orders of magnitude faster for some kinds of processing

1. Read a set of input files, and break it up into records.
2. The **mapper** is called once for every input record, and its job is to extract the key and value from the input record.
3. Sort all of the key-value pairs by key. (sort)
4. Call the **reducer** function to iterate over the sorted key-value pairs. (reducer)
  + reduce task takes the files from the mappers and merges them together, preserving the sort order

 putting the computation near the data
 > saves copying the input file over the network, reducing network load and increasing locality.

In order to achieve good throughput in a batch process, the computation must be (as much as possible) local to one machine.
1. take a copy of the user database
2. put it in the same distributed filesystem as the log of user activity events
3. use MapReduce to bring together all of the relevant records in the same place

Reduces does the joins.

**sort-merge join**
> mapper output is sorted by key
> reducers merge together the sorted lists of records from both sides of the join.
> mappers and the sorting process make sure that all the necessary data to perform the join operation for a particular user ID is brought together in the same place

**sessionization**
> collating all the activity events for a particular user session, in order to find out the sequence of actions that the user took

**linchpin objects** or hot keys.
if there is a very large amount of data related to a single key

**skew** (also known as hot spots)
> one reducer that must process significantly more records than the others
solution: spread the work of handling the hot key over several reducers

**reduce-side joins**
> actual join logic in the reducers

**map-side join**
> mapper simply reads one input file block from the distributed filesystem and writes one output file to the filesystem

simplest way of performing a map-side join applies in the case where a large dataset is joined with a small dataset

**broadcast hash join**
> each mapper for a partition of the large input reads the entirety of the small input (so the small input is effectively “broadcast” to all partitions of the large input)
> hash reflects its use of a hash table
> an alternative is to store the small join input in a read-only index on the local disk 

**Partitioned hash joins**
> If the two join inputs are partitioned in the same way (using the same key, same hash function, and same number of partitions), then the hash table approach can be used independently for each partition.


**Map-side merge joins**
>  applies if the input datasets are not only partitioned in the same way, but also sorted based on the same key
>  a mapper can perform the same merging operation that would normally be done by a reducer

**batch processing output**
> Search indexes
> machine learning systems: classifiers, recommendation systems

By treating inputs as immutable and avoiding side effects (such as writing to external databases), batch jobs not only achieve good performance but also become much easier to maintain
> Databases with read-write transactions do not have this property

 Hadoop opened up the possibility of indiscriminately dumping data into HDFS, and only later figuring out how to process it further
> The idea is similar to a data warehouse: simply bringing data from various parts of a large organization together in one place is valuable, because it enables joins across datasets that were previously disparate.
> Indiscriminate data dumping shifts the burden of interpreting the data: instead of forcing the producer of a dataset to bring it into a standardized format, the interpretation of the data becomes the consumer’s problem
 > sushi principle: “raw data is better”
 > distributed filesystem supports data encoded in any format.

Batch processes are less sensitive to faults than online systems, because they do not immediately affect users if they fail and they can always be run again.
 
 MapReduce approach is more appropriate for larger jobs: jobs that process so much data and run for such a long time that they are likely to experience at least one task failure along the way.

**materialization**
> writing out intermediate state to files
- can only start when all tasks in the preceding jobs (that generate its inputs) have completed
- Having to wait until all of the preceding job’s tasks have completed slows down the execution
- Mappers are often redundant
- Storing intermediate state in a distributed filesystem means those files are replicated across several nodes, which is often overkill for such temporary data.

Pipes do not fully materialize the intermediate state, but instead stream the output to the input incrementally, using only a small in-memory buffer.

**dataflow engines**
> explicitly model the flow of data through several processing stages
> repeatedly calling a user-defined function to process one record at a time on a single thread
> parallelize work by partitioning inputs
> copy the output of one function over the network to become the input to another function
> need not take the strict roles of alternating map and reduce
> uses "operators"
+ Expensive work such as sorting need only be performed in places where it is actually required
+ no unnecessary map tasks
+ the scheduler has an overview of what data is required where, so it can make locality optimizations
+  requires less I/O than writing it to HDFS
+  start executing as soon as their input is ready
+  execute significantly faster

if your graph can fit in memory on a single computer, it’s quite likely that a single-machine (maybe even single-threaded) algorithm will outperform a distributed batch process

----

a **“stream”** refers to data that is incrementally made available over time.

In a stream processing context, a record is more commonly known as an **event**
> a small, self-contained, immutable object containing the details of something that happened at some point in time.

an event is generated once by a *producer* (also known as a publisher or sender), and then potentially processed by multiple *consumers* (subscribers or recipients)

in a streaming system, related events are usually grouped together into a **topic** or **stream**.

The more often you poll, 
- the lower the percentage of requests that return new events, 
- and thus the higher the overheads become.

**messaging system**: 
> a producer sends a message containing the event, which is then pushed to consumers.
> allows multiple producer nodes to send messages to the same topic >> **publish/subscribe** model
> allows multiple consumer nodes to receive messages in a topic.

*What happens if the producers send messages faster than the consumers can process them?*
1. the system can drop messages
2. buffer messages in a queue
3. apply backpressure (also known as flow control; i.e., blocking the producer from sending more messages)

*What happens if nodes crash or temporarily go offline—are any messages lost?*
1. writing to disk 
2. replication 
3. may miss messages that were sent while it is unreachable.

if a large number of messages are dropped, it may not be immediately apparent that the metrics are incorrect
> every lost message means incorrect counters.
> generally require the application code to be aware of the possibility of message loss.

**message broker** (also known as a message queue), 
> a kind of database that is optimized for handling message streams
> can more easily tolerate clients that come and go 

consumers are generally asynchronous
1. producer sends a message
2. waits for the broker to confirm
3. does not wait for the message to be processed by consumers
4. delivery to consumers will happen at some undetermined future point in time

differences between message brokers and databases
1. automatically delete a message when it has been successfully delivered to its consumers (vs persistence in databases)
2. working set is fairly small. 
3. often support some way of subscribing to a subset of topics matching some pattern
4. do not support arbitrary queries, but they do notify clients when data changes

main patterns of messaging
**Load balancing**
> Each message is delivered to one of the consumers
> consumers can share the work of processing the messages in the topic. add consumers to parallelize the processing
> may assign messages to consumers arbitrarily

**Fan-out**
> Each message is delivered to all of the consumers
> allows several independent consumers to each “tune in” to the same broadcast of messages, without affecting each other

**Combined**
> separate groups of consumers may each subscribe to a topic
> each group collectively receives all messages
> within each group only one of the nodes receives each message

**acknowledgments**
> a client must explicitly tell the broker when it has finished processing a message so that the broker can remove it from the queue
> without the broker receiving an acknowledgment, it assumes that the message was not processed >> delivers the message again to another consumer

**message order**
> To avoid this issue, you can use a separate queue per consumer
> is not a problem if messages are completely independent of each other
> can be important if there are causal dependencies between messages

**AMQP/JMS-style**
> assigns individual messages to consumers, and consumers acknowledge individual messages when they have been successfully processed, then deleted
> exact order of message processing is not important
> OK if no need to go back and read old messages again after they have been processed.

**log-based message brokers**
log is simply an append-only sequence of records on disk
> a producer sends a message by appending it to the end of the log
> a consumer receives messages by reading the log sequentially
> If a consumer reaches the end of the log, it waits for a notification that a new message has been appended
> only needs to periodically record the consumer offsets
> a form of buffering with a large but fixed-size buffer
> more like the batch processes
+ reduced bookkeeping overhead 
+ increase the throughput of log-based systems.
use when:
+ where each message is fast to process
+ where message ordering is important

log can be partitioned
> Different partitions can then be hosted on different machines
> A topic can then be defined as a group of partitions that all carry messages of the same type
> the broker assigns a monotonically increasing sequence number, or offset, to every message
> no ordering guarantee across different partitions

*coarse-grained load balancing*
> number of nodes sharing the work of consuming a topic can be at most the number of log partitions in that topic
> If a single message is slow to process, it holds up the processing of subsequent messages in that partition

*Disk space usage*
> To reclaim disk space, the log is actually divided into segments
> time to time old segments are deleted or moved to archive storage


With data warehouses this synchronization is usually performed by ETL processes: a batch process

**dual writes**
> application code explicitly writes to each of the systems when data changes
> one of the writes may fail while the other succeeds

**change data capture**
> process of observing all data changes written to a database and extracting them in a form in which they can be replicated to other systems
> change data capture makes one database the leader (the one from which the changes are captured), and turns the others into followers.
> is usually asynchronous: the system of record database does not wait for the change to be applied to consumers before committing it

**log compaction**
> the storage engine periodically looks for log records with the same key, throws away any duplicates, and keeps only the most recent update for each key
> an update with a special null value (a tombstone) indicates that a key was deleted >> removed

**event sourcing**
> storing all changes to the application state as a log of change events
> it is more meaningful to record the user’s actions as immutable events, rather than recording the effect of those actions on a mutable database.

*Deriving current state from the event log*
> replaying the event log allows you to reconstruct the current state of the system

a request from a user first arrives, it is initially a *command*
> it may still fail
If the validation is successful and the command is accepted, it becomes an *event*
> durable and immutable
fact
> point when the event is generated

consumer of the event stream is not allowed to reject an event

mutable state and an append-only log of immutable events do not contradict each other

changelog
> represents the evolution of state over time

The truth is the log. The database is a cache of a subset of the log

 **command query responsibility segregation**
> you gain a lot of flexibility by separating the form in which data is written from the form it is read

**Concurrency control**
> One solution would be to perform the updates of the read view synchronously with appending the event to the log.

----

**Processing Streams**
1. take the data in the events and write it to storage
2. push the events to users. a human is the ultimate consumer of the stream
3. process one or more input streams to produce one or more output streams.

**operator** or a **job**
> piece of code that processes streams to produce other, derived streams

**Uses of Stream Processing**
+ Fraud detection systems
+ Trading systems
+ Manufacturing systems
+ Military and intelligence systems

**Complex event processing**
> analyzing event streams, especially geared toward the kind of application that requires searching for certain event patterns
> allows you to specify rules to search for certain patterns of events in a stream.

complex event 
> the details of the event pattern
> When a match is found, the engine emits this.

 a database stores data persistently and treats queries as transient
 vs. 
 CEP engines reverse these roles: queries are stored long-term, and events from the input streams continuously flow past them in search of a query that matches an event pattern

**Stream analytics**
> less interested in finding specific event sequences and is more oriented toward aggregations and statistical metrics over a large number of events
* Measuring the rate of some type of event
* Calculating the rolling average of a value
* Comparing current statistics to previous time intervals

*Search on streams*
> This is done by formulating a search query in advance, and then continually matching the stream of news items against this query
> the queries are stored, and the documents run past the queries

need to be able to handle such straggler events 
1. Ignore the straggler events
2. Publish a correction, an updated value for the window with stragglers included

To adjust for incorrect device clocks, one approach is to log three timestamps
1. The time at which the event occurred, according to the device clock
2. The time at which the event was sent to the server, according to the device clock
3. The time at which the event was received by the server, according to the server clock

A **tumbling window** has a fixed length, and every event belongs to exactly one window.
> 10:03:00 and 10:03:59 are grouped into one window, events between 10:04:00 and 10:04:59 into the next window

A **hopping window** also has a fixed length, but allows windows to overlap in order to provide some smoothing.
> a 5-minute window with a hop size of 1 minute would contain the events between 10:03:00 and 10:07:59, then the next window would cover events between 10:04:00 and 10:08:59

A **sliding window** contains all the events that occur within some interval of each other. 
> a 5-minute sliding window would cover events at 10:03:39 and 10:08:12

a **session window** has no fixed duration
> grouping together all events for the same user that occur closely together in time
> window ends when the user has been inactive for some time

**Stream-stream join (window join)**
> Both input streams consist of activity events, and the join operator searches for related events that occur within some window of time.

**Stream-table join (stream enrichment)**
> One input stream consists of activity events, while the other is a database changelog.

**Table-table join (materialized view maintenance)**
> Both input streams are database changelogs.

**slowly changing dimension (SCD)**
> If the ordering of events across streams is undetermined, the join becomes nondeterministic 
> addressed by using a unique identifier for a particular version of the joined record

**Fault Tolerance**

**exactly-once semantics**
> Although restarting tasks means that records may in fact be processed multiple times, the visible effect in the output is as if they had only been processed once. 

**microbatching**
> break the stream into small blocks, and treat each block like a miniature batch process.
> batch size is typically around one second
> implicitly provides a tumbling window equal to the batch size

**rolling checkpoints**
> If a stream operator crashes, it can restart from its most recent checkpoint and discard any output generated between the last checkpoint and the crash.
> checkpoints are triggered by barriers in the message stream

ensure that all outputs and side effects of processing an event take effect if and only if the processing is successful

**Idempotence**
> one that you can perform multiple times, and it has the same effect as if you performed it only once

*Rebuilding state after a failure*
> keep state local to the stream processor, and replicate it periodically
> rebuilt from the input streams.

----

Our goal is to discover how to design applications that are better than the ones of today:
> robust, 
> correct, 
> evolvable, and 
> ultimately beneficial to humanity.

**Data Integration**

The need for data integration often only becomes apparent if you zoom out and consider the dataflows across an entire organization.

> where is data written first?
> which representations are derived from which sources?
> How do you get data into all the right places, in the right formats?

transaction systems
> usually provide linearizability
> implies useful guarantees such as reading your own writes

derived data systems
> updated asynchronously
> most promising approach for integrating different data systems

**limits of total ordering**
> If the throughput of events is greater than a single machine can handle, you need to partition it across multiple machines. The order of events in two different partitions is then ambiguous.
> typically have a separate leader in each datacenter, because network delays make synchronous cross-datacenter coordination inefficient. implies an undefined ordering of events that originate in two different datacenters.
> When applications are deployed as microservices, no durable state shared between services. >> no defined order for those events.
> applications maintain client-side state >> clients and servers are very likely to see events in different orders.

Ordering for causality
> Logical timestamps can provide total ordering without coordination. require additional metadata to be passed around
> log an event to record the state of the system that the user saw before making a decision >> reference that event identifier in order to record the causal dependency
> Conflict resolution algorithms

**Batch and Stream Processing**

Batch processing has a quite strong functional flavor 
>  inputs as immutable
> outputs as append-only

distributed transactions abort if any one participant fails, 
> tend to amplify failures by spreading them to the rest of the system

Stream processing allows changes in the input to be reflected in derived views with low delay
vs
batch processing allows large amounts of accumulated historical data to be reprocessed in order to derive new views onto an existing dataset

with reprocessing it is possible to restructure a dataset into a completely different model in order to better serve new requirements.

Derived views allow gradual evolution
> do not need to perform the migration as a sudden switch.
> maintain the old schema and the new schema side by side as two independently derived views

**lambda architecture**
> incoming data should be recorded by appending immutable events to an always-growing dataset
> proposes running two different systems in parallel
+ batch processing system >> simpler and thus less prone to bug
+ stream-processing system >>  fast approximate algorithms

Unifying batch and stream processing in one system requires the following features
> ability to replay historical events through the same processing engine that handles the stream of recent events
> Exactly-once semantics for stream processors
> windowing by event time, not by processing time

information management philosophies
> **Unix**:  a logical but fairly low-level hardware abstraction >> simpler in complexity
> **Relational DB**:  high-level abstraction that would hide the complexities of data structures on disk, concurrency, crash recovery, and so on >> simpler in commands

**Federated databases / polystore: unifying reads**
>  unified query interface

**Unbundled databases: unifying writes**
> unbundling a database’s index-maintenance features in a way that can synchronize writes across disparate technologies 

The big advantage of log-based integration is **loose coupling** between the various components
> asynchronous event streams make the system as a whole more robust to outages
> faulty consumer can catch up when it is fixed, so it doesn’t miss any data
> unbundling data systems allows different software components and services to be developed, improved, and maintained independently 

A single integrated software product may also be able to achieve better and more predictable performance on the kinds of workloads for which it is designed

The advantages of unbundling and composition only come into the picture when there is no single piece of software that satisfies all your requirements.

*Separation of application code and state*
> it makes sense to have some parts of a system that specialize in durable data storage, and other parts that specialize in running application code
> keep stateless application logic separate from state management (databases)

Instead of treating a database as a passive variable that is manipulated by the application, we think much more about the interplay and collaboration between state, state changes, and code that processes them.

**microservices approach**
> query an service or database in order to obtain the current value

**dataflow approach**
> subscribe to a stream of updates ahead of time and record the current rate in a local database whenever it changes.
> only needs to query the local database.
> faster and more robust

**Observing Derived State**

**write path**
> process for creating derived datasets (such as search indexes, materialized views, and predictive models) and keeping them up to date.
> may go through multiple stages of batch and stream processing, and eventually every derived dataset is updated to incorporate the data that was written.
> eager evaluation

**read path**
> read from the derived dataset, perhaps perform some more processing on the results, and construct the response to the user.
> lazy evaluation

*derived dataset is the place where the write path and the read path meet*

**cache of common queries**
> precompute the results for only a fixed set of the most common queries

**Stateful, offline-capable clients**

**offline-first applications**
> do as much as possible using a local database on the same device

cache of state on the server
> on-device state

If you are designing data systems, I hope that you will keep in mind the option of subscribing to changes, not just querying the current state.

store acts as the boundary between the write path and the read path

Writing read events to durable storage thus enables better tracking of causal dependencies

you could generate a unique identifier for an operation (such as a UUID) and include it as a hidden form field in the client application
> If the web browser submits the POST request twice, the two requests will have the same operation ID.
> You can then pass that operation ID all the way through to the database and check that you only ever execute one operation with a given ID

reasoning about concurrency and partial failure is difficult and counterintuitive
> The consequence is lost or corrupted data.

* violations of timeliness are “eventual consistency,” 
* violations of integrity are “perpetual inconsistency.”
>  integrity is much more important than timeliness.

In many business contexts, it is actually acceptable to temporarily violate a constraint and fix it up later by apologizing.
> Whether the cost of the apology is acceptable is a business decision. 

**Coordination-avoiding data systems**
> Synchronous coordination can still be introduced in places where it is needed 
> the sweet spot where there are neither too many inconsistencies nor too many availability problems. 

**Designing for auditability**

Checking the integrity of data is known as auditing.
> try restoring from your backups from time to time

*self-validating or self-auditing systems that continually check their own integrity, rather than relying on blind trust*

event-based systems can provide better auditability

*Having continuous end-to-end integrity checks gives you increased confidence about the correctness of your systems*