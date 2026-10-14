# coalesce
## InstanceDetail
creates a Bean that inserts `InstanceDetail` --> id, ip, cluster, ASG, timestamp 

@BackendAPI 

EARS runtime will register this bean with the IPC server, 
inject the IPC server, 
create a IPC model --> has a reflection of all the methods with @BackendAPI


## IPCSchema
--> Modelled in RH 
IPC model (doesn't have the service id yet)
each method/type has a service Id
service id --> load the current schema, match up existing types, if matches, then use the same service id, if not, get a new service id

key: servicename
1. methods 
	1. signature
	2. return type
	3. arguments
	4. deprecated?
2. types 
	1. objects
		1. fields
	2. enums
		1. values
	3. list/set
	4. map
IPC Schema per service
--> used for Codegen

## IPCDeployedSchemaSubset
(by instance)
takes the IPC schema and cuts out unused part 
take IPC schema and trim to IPC model 
happens when instance comes up 

## IPCSchemaUseSubset

# coalesce-usage
build time --> scan all class files and find annotations and output to a text file. 

# Routing 
@IPCClient
main stack only make connections to feature stack iff main is not there 

don't generate code at build time until achieve hermatic (repeatable & reproducible) build

--- 
separate schema registry with the IPC stuff 
