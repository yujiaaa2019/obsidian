In this Runnable guide, we’ll walk you through the steps required to create an EARS DGS for an entity that’s sourced from an existing Cinder feed. The guide will showcase a subset of EARS functionality, refer to [EARS Features](https://coda.io/d/_d_ZSz7-l-d9/_suY1p#_luUtQ) for a complete overview.

# Hollow Feed 
We have created a Hollow feed with MVS Crystal Ball prediction values. Please refer to Create Hollow Feeds if your team does not have a existing one yet. 
Feed link: 
Table link: 

# Starter Repo 


## Setup Steps
1. create a new table under ears_prod schema. 
2. Insert data from the target tables into the newly created table 
3. create a Hollow Producer directing to the created table 
	1. will trigger Hollow to populate table data into hollow cache 
4. 