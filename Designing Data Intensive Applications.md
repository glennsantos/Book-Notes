Most important system concerns:

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