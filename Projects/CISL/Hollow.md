# Hollow
Netflix Hollow is a java library and toolset for disseminating in-memory datasets from a single producer to many consumers for high performance read-only access.

Central Idea: 
- compress objects into arrays and array pointers to reduce data size, so that data could be co-located in-memory with the application 
Memory Co-location benefits: 
- don't need to think about I/O latency 

<mark class="hltr-pink">Total, high density, near cache</mark>
- compress data so that can access very quickly 
- more secure than RAW Hollow

Problem: 
1. a delay in the snapshot (30s in the case of incremental Hollow)
	1. but in the UI, will be 1:30 at least to be shown in the UI ==> a problem

Gutenberg (S3): 
- holds cinder blob 


## Index
### Field Paths 
A field path represents a path through a type hierarchy
- str_a.str_b 
- A resolved field path may be used to extract a value of the field for the last segment given an object of the starting type.
#### Primary Index
A primary key has one or more field paths 
#### Unique Key Index
A `UniqueKeyIndex` may be used to find an object given a key value where there is a one-to-one mapping between the key value and object i.e. each object of a primary type must have a _unique_ key value.
#### Hash Index
A `HashIndex` may be used to find one or more records (objects, lists, sets, or maps) given a query. Unlike `UniqueKeyIndex` there is no one-to-one mapping between a query and a record.