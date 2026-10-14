## RAW Hollow & Security 
- Grpc service / dataset sharing
- how this hollow dataset can be published to other clients (Meechum primarily)

### RAW Hollow Intro
1. a data store that get pushed around 
2. data in RAW Hollow could be separated in two parts  
	1. snapshot version of the state (in gutenberg)
	2. any in-flight changes (in RAW Hollow)
3. a writer application 
4. a logKeeper application (store in-flight changes)
	1. write immediately gets into logkeeper
5. a producer (Hollow producer)

Could customize a `security group` specific to cluster/namespace  

`Gandalf Policy`: grants access to the RAW Hollow namespace

the 30 seconds wait is for compression, not for writes

Current structure of Pandora: 
Pandora --> Cassandra --> distributor --> elasticSearch + ... 

### Requested Features / Concerns 
1. mutiple regions -- for failure recovery 
2. meechum (3rd party product) -- very cumbersome -- database account is extremely restrictive
	1. only runs in us-west-? only one region 
3. delete namespace? 
	1. no
	2. only thing that is exposed to user ==> metadata (user w/o permission)
	3. UI is secure

`2023-08-25`
Concentrate on the Read part 
- API that Meechum use 
Create a lib that supports the Mecchum API

Full Dataset (~ 400 MB) on unstable now 

Pandora Hollow, focus on `gets`: search queries
- how to model some search patterns

Meechum: 
- find id
- given groups, find members

How to use RAW Hollow SQL so that it's like ElasticeSearch
Meechum currently only use REST 
