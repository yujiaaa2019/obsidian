## Distributed Database 
#### MySQL and Postgresql 
- Relational Database
- pros: 
	1. good for transactional operations (high frequency of reads & writes)
	2. satisfy ACID 
- cons: 
	1. only has one node, so will not be available if the node dies (low availability)
#### Cassandra 
Column-based database
- pros: 
	1. distributed, better reads (faster)
	2. 
- cons: 
	1. it is eventually consistent (could not guarantee RAW)

### CAP Theorem 
*C*: consistency (immediate consistent vs. eventually consistent)
*A*: availability (highly available vs. not available)
*P*: partition (partition tolerant vs. not partition tolerant)

*Distributed database could only satisfy two of the three criteria*

### ACID
*A*: atomicity 
*C*: consistency 
*I*: isolation
*D*: durability 