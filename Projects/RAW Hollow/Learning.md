## Terms 
1. write-through cache: with a write-through caching policy, the system's processor writes data to the cache first and then immediately copies that new cache data to the corresponding memory / disk 
	1. ensures data in cache and storage is always identical 
2. Socket: one endpoint of a two-way communication lilnk between two programs running on the network
	1. an endpoint == an IP address + a port number 
## Garantees 
1. durable: once a write is confirmed, it will not be lost 
2. linearlizable: if update E1 before update E2, then it garantees that they will be updated in this order 
3. atomic reads: retrieval operations againt in-memory dataset represent a consistent view of the dataset at a moment in time
4. Availablity: no matter how the network or infrastructure fails, an entire local copy of the datset will remain usable

## Tradeoffs
1. limited size: <= 100 million records
2. huge transactions not supported 
3. CAP tradeoff: when network partition happens, RH will be unable to provide consistency garantees. 

# Architecture 

- in-flight updates are stored in a replicated log and are propagated to all clients, where they are overlaid on top the base Hollow datset for ready in-memory access until they are incorporated into the base Hollow feed. 

Each namespace will be served by a single RAWHollow deployment and is not shared with any other namespace. 
A deployment has 3 instance roles: 
1. Writers 
2. LogKeepers 
3. Producers 
**![](https://lh7-us.googleusercontent.com/V5z-x8DjT1IYx_6JesQh7CnJMgS8HrVd5k6lcdJ-UK4oCTcECml3JtjqF-WHJts-pixdgpjsyIq6yAoBCqyOYiod_yGxo22jkBEDF9fN8ZrontdYu5UyAWqHQLX6r5eLRVqojuNTLK9GC9wfHUwH5Vo)**
1. writing applications send updates to the system and may read the individual records remotely
2. reading applications load the entire dataset into their own RAM 

Write logic: 
1. send FlatRecord over gRPC 
	1. FlatRecords provide the ability to serialize records for **addition** to a remote state engine
2. upon receiving an update request, the writer creates a log entry. The log entry is assigned an **offset** exactly one greater than any previously added entry. 
3. The writer then replicates the log entry to several LogKeeper instances. 
4. The client application instances immediately make the update available to the application inmemory via an overlay on the base Hollow dataset     
5. The **producer** instance compacts the log entry into the base Hollow dataset; the producer. runs in a ~30s repeating cycle to accomplish this. 

## Writer 
There are multiple writers instance for a deployment to ensure high availablity, but only one will be **active**. The active writer is selected by ZooKeeper leader election logic. 

A writer instance contains a **Froth Map**, a **Hollow Consumer**, a **Message Queue**
- Froth Map: an internal hash table of in-flight updates. Records in the Froth Map are keyed by their primary key, and the data inside is represented as flat records. 

Leadership Protocol
- standby writer need to make sure it is in sync with all the updates by talking to all LKs and connect with most of them 

## LogKeeper 
LK tracks three offsets: 
1. the earliest offset currently retained 
2. the next offset to be written 
3. the committed offset 

## Producer 
each RH deployment has only one Producer 
*pull the latest log entries from a LK and incorporate the updates into the base Hollow dataset*

## Clients 
client instance contain a RAWHollow Engine == a Hollow Consumer + a Froth Map. 
- consistently pulls from LKs for changes 
- client is eventual consistent 

## ZooKeeper
provisioning the distributed system
- leader election among writers. 
- provides a low latency discovery and notification mechanism for understanding what nodes exist in the system at any given time, the addresses at which they are located, and their current states. 
- provides an atomic conditional update at a critical point in the consensus algorithm. 


1. running RH from mantis 
	1. what's mantis 
	2. IP objects --> gives access to all security groups in that region 
2. publish RH image so that other could work on 


### standby writer 
-- not syncing the froth map in the standby writer
LogKeerps: distributed queue (ring buffers) 

Producer: 
- every 30s it has a loop --> Hollow Producer producer a new cycle 
- push a topic in Gutenberg (publish updates)
	- inflight messages (new writes in the past 30s)
RAW Hollow Engine
- snapshot + froth

ZooKeepers
reflects the topology of the system 
- writers + ip
- producer + ip ...

writer tells LKs to move header offset once received a new Gutenberg annocement that a new snapshot of HollowDataset has been produced 


Producer is CPU bound and network bound --> CPU and network resources could improve how fast ther producer could produce (and hence clear the data in the ring buffers in the LKs) 
- producer instance size controls the network bandwidth 
- network --> upload the produced snapshot file to S3/gutenberg
- CPU --> serialize / diffing the dataset and produce the snapshot file and store it locally as a file 

double snapshots --> need the old a new snapshot when there is a schema change 

Gutenberg --> 

commit number management --> 


RawHollow Index --> a join of Hollow Index and the data in the froth map 