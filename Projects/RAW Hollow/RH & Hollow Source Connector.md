

current Hollow SC has up-to 1-min propagation 

Job types: 
- real-time CDC
- backfill entire dataset 

also requires
- high availability 

Persist CDC 
High-level architecture
RH data --> Source Connector --> DataMesh

----
doing deltas by ordinal 

Hollow SC --> delay up to publish cycle ~30s 
1. worse worse case up to 2 mins 
2. cannot check and pull from Gutenberg faster than 10s 

Connect data to graph search 

RAW Hollow 
one spin application for RH SC 
one stack for each namespace 


