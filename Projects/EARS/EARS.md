1. record: Mantis: record request and response 
2. replay 

Test package in Hollow
- mocking 
- test environment 


Ideas: 
1. make it available to all GQL, gRPC, and REST endpoints? 
2. Is there a way that we could automate the "deploying to Hollow" step? 

Current State: 
- able to create simple GQL/gRPC APIs to query from Hollow Cache 
	- pre-req: Hollow name and actual query name are the same 
- Ongoing step: add a transformation layer so that users could insert custom functions 
	- could map query name to Hollow name 
	- could perform computation / transformation on Hollow entities 

### Starter Repo Walkthrough
Question: 
1. might need to manually add dependencies like GQL & DGS implementation 
2. encountered error during validation --> UNION not implemented yet 

cinder-cloud-service-control-panel --> infra for cinder service 
[logs for ears validation + deployment (test env)](https://radar.prod.netflix.net/apps/cindercloudservicecontrolplane/test/logs)


Two files: 
1. schema.graphql
	1. specify the GraphQL schema the EARS DGS will expose and what EARS will register with [Reggie](https://bfgui.netflix.net/prod).
2. wiring.yaml
	1. This files specifies how to *connect GraphQL queries with data sources*
	2. data sources: Cinder feeds / Iceberg Table

# EARS Features 
## wiring.yaml 
`typeReference`: specifies which fields of the underlying entity make up the federeated entity key 
- contains `keyMapping`: GraphQL key name: Hollow Attribute Field Name

==How to separate from Cinder and Trino==: in wiring.yaml, if `cinder` block is present, then Cinder

## Business Logic Hooks 
code inside /api is generated

can define global indices 
implement `Queries.java` for business logic 
not Hollow specific. 

GQL specific ==> Data fetchers 

gRPC
- attach service to grpc server at runtime. One grpc server started at the runtime 
- will run deteleProto, generateProto at build time
- protobuff 

EARS benefits? 
- reduce your work and could let you only focus on what you care about

<mark class="hltr-pink">2023-10-19</mark>
What does it mean for EARS to going to prod? 
- testing hooks for the consumers 

<mark class="hltr-pink">2023-10-23</mark>
[[EARS Working Group Quarterly]]
Possible Improvements to EARS 
- Everytime I re-build the jar, it's causing a reloading of cinder namespaces, which significantly increased the wait time for debugging experience. 
	- Solution: maybe we could opt to disable the reloading in local dev env? 
- white-glove support: if there is issue, then we will jump in and support
	- how do we decide if a client is a white-glove client
		- based on the value how what we could get by solving / migrating on this issue
	- problems: users might not know what the rough edges are
- background support: otherwise, they will have to rely on our decision on whether we will support the feature 


<mark class="hltr-pink">2023-11-13</mark>
Next Step: 
1. init EARS repo using newt app-type, but wrap it in the CLI form
2. EARS CLI: add local validation 
3. GenerateAPI: figure out a way to do compile first -> run plugin -> compile again. Right now we are doing plugin then compile, but the problem is that the compile step is not aware of the code generated in the plugin step. 
	1. afterwards, support generating named Index (like useMovieHashIndex instead of useHashIndex("movie")) --> avoid using String to cause misspelling issue that could only be found by running the code 
