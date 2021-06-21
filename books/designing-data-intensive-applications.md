# Designing Data-Intensive Applications

_by Martin Kleppmann_

![](./resources/designing-data-intensive-applications.jpg "Designing Data-Intensive Applications")

---

## Table of contents

#### Part I. Foundations for Data Systems.
- [Chapter 1: Reliable, Scalable, and Maintainable Applications](#chapter1)
- [Chapter 2: Data Models and Query Languages](#chapter2)
- [Chapter 3: Storage and Retrieval](#chapter3)
- [Chapter 4: Encoding and Evolution](#chapter4)

#### Part II. Distributed Data.
- [Chapter 5: Replication](#chapter5)
- [Chapter 6: Partitioning](#chapter6)
- ... more to come

---

<a name="chapter1">
    <h1>Chapter 1: Reliable, Scalable, and Maintainable Applications</h1>
</a>



<a name="chapter2">
    <h1>Chapter 2: Data Models and Query Languages</h1>
</a>




<a name="chapter3">
    <h1>Chapter 3: Storage and Retrieval</h1>
</a>



<a name="chapter4">
    <h1>Chapter 4: Encoding and Evolution</h1>
</a>



<a name="chapter5">
    <h1>Chapter 5: Replication</h1>
</a>


<a name="chapter6">
    <h1>Chapter 6: Partitioning</h1>
</a>

* **Sharding** &rarr; break data up into partitions. Reason &rarr; **Scalability**.

_Note: Replication is ignored in this chapter._

## Partitioning and Replication

* Partitioning + Replication &rarr; copies of each partition stored on multiple nodes.
* Each node may be the leader for some partitions and a follower for other partitions.

## Partitioning of Key-Value Data

* **Skewed** &rarr; when some partitions have more data or queries than others.
* **Hot spot** &rarr; when a partition is disproportionately high load.

### Partitioning by Key Range

* Approach &rarr; assign a **continuous range of keys**.
* Adapt partition boundaries to data &rarr; evenly distribution.
* Keys in sorted order within each partition.
  - :thumbsup: &rarr; easy range scans. Keys ~ a concatenated index.
  - :thumbsdown: &rarr; some access patterns &rarr; hot spots.

### Partitioning by Hash of Key

* Approach &rarr; use a **hash function** to determine the partition for a given key. Reason &rarr; avoid skew and hot spots.
* Assign each partition a range of hashes. Every key whose hash falls within a partition's range &rarr; stored in that partition.
* Partition boundaries either:
  - evenly spaced, or
  - chosen pseudorandomly. Consistent hashing &rarr; avoid central control or distributed consensus. Better name &rarr; **hash partitioning**.
* :-1: efficiency in range queries as keys in all partitions (:x: order).
* **Concatenated index** approach &rarr; one-to-many relationship.

### Skewed Workloads and Relieving Hot Spots

* Still hot spots with partitioning by a hash key if all reads and writes are from the same key (ex. celebrity users on Social Media &rarr; large volume of writhes for a particular key).
* Responsibility of the application to reduce the skew (ex. adding a random number to the key &rarr; keys distributed to different partitions).

## Partitioning and Secondary Indexes

* A secondary index don't map neatly to partitions.
* Approaches:

### 1) Partitioning Secondary Indexes by Document

* Each partition is completely separate and keeps its own indexes, covering only the documents in that partition.
* **Document-partitioned** index &rarr; a **local index** &rarr; **scatter/gather** approach.
* Queries on secondary indexes &rarr; expensive. Latency.

### 2) Partitioning Secondary Indexes by Term

* **Global index** &rarr; covers data in all partitions. The index has to be partitioned too to other nodes.
* a.k.a. **term-partitioned**
* term-partitioned index > document-partitioned index:
  - Advantage &rarr; more efficient reads.
  - Disadvantage &rarr; slower writes (affect multiple partitions).

## Rebalancing Partitions

* Changes to a database with time:
  - Increase in query throughput &rarr; add more CPUs.
  - Increase in dataset size &rarr; add more RAM.
  - More machines failing.
* **Rebalancing** &rarr; move load from one node in the cluster to another.
* Requirements:
  - Load shared fairly between nodes after rebalancing.
  - While rebalancing, the DB has to continue accepting reads and writes.
  - Only move the necessary data between nodes.

### Strategies for Rebalancing

* Ways of assigning partitions to nodes.

#### _How not to do it: hash mod N_

* When partitioning by the hash of a key, divide the possible hashes into ranges and assign each range to a partition.
* Do not use _mod_ &rarr; if the number of nodes changes, most of the keys will need to be moved to a different node.

#### _Fixed number of partitions_

* Create many more partitions than there are nodes. Assign several partitions to each node.
* When a node is added to a cluster, it can "steal" a few partitions from every existing node until fairly distributed again. Same when a node is removed.
* Only entire partitions are moved between nodes. Takes time.
* When mismatched HW &rarr; assign more partitions to more powerful nodes &rarr; greater share of load in them.
* Choose a high number of fixed partitions &rarr; future growth.
* If partitions are very large, expensive rebalancing. If partitions too small, overhead.
* Hard to achive the "just right" number of partitions if it is fixed but the dataset size varies.

#### _Dynamic partitioning_

* Fixed number of partitions &rarr; inconvenient for DBs that use key range partitioning.
* Key range-partitioned DBs create partitions dynamically (configurable size for splitting).
* Each partition assigned to a node. Each node can handle multiple partitions.
* Advantage: the number of partitions adapts to the total volume.
* Pre-splitting &rarr; DBs like HBase or MongoDB allow an initial set of partitions configured on an empty DB.

#### _Partitioning proportionally to nodes_

* Dynamic partitioning &rarr; number of partitions proportional to the size of the dataset.
* Fixed number of partitions &rarr; size of each partition is proportional to the size of the dataset.
* In both cases &rarr; number of partitions is independent of the number of nodes.
* Another approach &rarr; have a fixed number of partitions per node. Size of each partition also stable.

### Operations: Automatic or Manual Rebalancing

* Fully automated rebalancing &rarr; less operational work but unpredictable. Dangerous when done with automatic failure detection.
* Better to have a human to prevent operational surprises.

## Request Routing

* After partitioning a dataset across multiple nodes and machines, how does a client know which node to connect to?
* Rebalancing &rarr; change of assignment of partitions to nodes.
* **Service discovery**. Approaches:
  - Allow clients to contact any node.
  - Send requests from clients to a routing tier &rarr; determines node.
  - Require clients be aware of the partitioning and assignments of partitions to nodes.
* How does the routing decisor learn about changes in the assignment of partitions to nodes?
* Consensus in a distributed system &rarr; hard to achieve.
* **Zookeper** as a coordination system. Nodes register to Zookeeper.
* Zookeeper for tracking partition assignment in Kafka. MongoDB uses a config server and mongos daemons as the routing tier.
* **Goosip protocol* used by Cassandra. Requests can be sent to any node and it forwards to the appropiate node. No dependency with an external coordination service such as Zookeeper.

### Parallel Query Execution

* MPP &rarr; **Massively Parallel Processing** relational DBs for analytics.

## Summary

* Ways of partitioning a large dataset into smaller subsets.
* Goal of partitioning &rarr; spread the data and query load evenly accross multiple machines, avoid hot spots
* Approaches for partitioning:
  - Key range partitioning &rarr; keys sorted, advantage &rarr; efficient range queries but risk of hotspots.
  - Hash partitioning &rarr; hash function applied to a key
